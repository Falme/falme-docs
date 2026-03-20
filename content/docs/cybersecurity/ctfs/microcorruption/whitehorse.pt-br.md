---
title: "Whitehorse"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Whitehorse

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Primeiramente, vamos verificar a função `main`:

```
4438 <main>
4438:  b012 f444      call	#0x44f4 <login>
```

A única coisa que faz é chamar a função `login`, então vamos verificar essa função também:

```
44f4 <login>
44f4:  3150 f0ff      add	#0xfff0, sp
44f8:  3f40 7044      mov	#0x4470 "Enter the password to continue.", r15
44fc:  b012 9645      call	#0x4596 <puts>
4500:  3f40 9044      mov	#0x4490 "Remember: passwords are between 8 and 16 characters.", r15
4504:  b012 9645      call	#0x4596 <puts>
4508:  3e40 3000      mov	#0x30, r14
450c:  0f41           mov	sp, r15
450e:  b012 8645      call	#0x4586 <getsn>
4512:  0f41           mov	sp, r15
4514:  b012 4644      call	#0x4446 <conditional_unlock_door>
4518:  0f93           tst	r15
451a:  0324           jz	$+0x8 <login+0x2e>
451c:  3f40 c544      mov	#0x44c5 "Access granted.", r15
4520:  023c           jmp	$+0x6 <login+0x32>
4522:  3f40 d544      mov	#0x44d5 "That password is not correct.", r15
4526:  b012 9645      call	#0x4596 <puts>
452a:  3150 1000      add	#0x10, sp
452e:  3041           ret
```

Resumindo, esta função:
- Escreve na tela solicitando uma senha com `<puts>`
- Solicita a entrada de senha com `<getsn>`
- Verifica se a senha está correta com `<conditional_unlock_door>`
- Se a senha estiver correta, escreve que a senha está correta.
- Se a senha estiver incorreta, escreve que a senha está incorreta.

Depois de tudo isso, podemos ver também que na linha `0x452a` há uma adição ao stack pointer, talvez isso possa ser útil. Mas primeiro vamos verificar a função `conditional_unlock_door`:

```
4446 <conditional_unlock_door>
4446:  0412           push	r4
4448:  0441           mov	sp, r4
444a:  2453           incd	r4
444c:  2183           decd	sp
444e:  c443 fcff      mov.b	#0x0, -0x4(r4)
4452:  3e40 fcff      mov	#0xfffc, r14
4456:  0e54           add	r4, r14
4458:  0e12           push	r14
445a:  0f12           push	r15
445c:  3012 7e00      push	#0x7e
4460:  b012 3245      call	#0x4532 <INT>
4464:  5f44 fcff      mov.b	-0x4(r4), r15
4468:  8f11           sxt	r15
446a:  3152           add	#0x8, sp
446c:  3441           pop	r4
446e:  3041           ret
```

Essa função, basicamente, apenas envia valores entre os ponteiros usando o `sp` (stack pointer), depois envia o valor `0x7e` para a pilha e chama `<INT>`:

```
4532 <INT>
4532:  1e41 0200      mov	0x2(sp), r14
4536:  0212           push	sr
4538:  0f4e           mov	r14, r15
453a:  8f10           swpb	r15
453c:  024f           mov	r15, sr
453e:  32d0 0080      bis	#0x8000, sr
4542:  b012 1000      call	#0x10
4546:  3241           pop	sr
4548:  3041           ret
```

A função `INT` lê 2 bytes após o `sp` (ponteiro de pilha) e os envia para `r14`, que é movido para `r15` e então realizar uma ação `bis` com `0x8000` e `sr`. Pense no comando `bis` como um comando `ou` (`0x8000` | `sr`).

Em seguida, chamará a função em `0x10` (`<__trap_interrupt>`) para verificar se a porta será aberta ou não.

> **Pequeno intervalo: Como exatamente a porta se abre**
>  
> A função trap_interrupt (`<__trap_interrupt>`) lida com o hardware, enviando uma mensagem através do ponteiro `sr` para verificar o que o hardware fará com o sistema operacional.
>  
> No nosso caso, o valor `ff00` precisa ser definido em `sr` para que a porta seja aberta, mesmo que a função `<__trap_interrupt>` seja chamada, ela não abrirá com outros valores.

Ao executar nosso código com uma senha aleatória, podemos ver que o valor de `sr` na função de verificação é sempre `0xfe00`, próximo do que precisamos, mas ainda não está correto. Precisamos alterar esse valor.

Se formos alterar alguns valores, primeiro vamos verificar o limite de entrada, fazendo um buffer overflow e verificando na memória. Primeiro, vamos tentar adicionar vários `6`.

```
666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666
```

Após inserir e executar um comando de 'passo' (`s`), podemos ver que nossa entrada alcança entre `0x3cb8` e `0x3ce7`. 

Agora vamos ver se o comando em `login` nas linhas `0x452a` e `0x452e` se sobrepõe à nossa entrada. Essas linhas são:

```
452a:  3150 1000      add	#0x10, sp
452e:  3041           ret
```

Essas linhas adicionam `0x10` ao `sp` e então usam a posição do `sp` para retornar à função que o chamou. Devemos verificar se essa posição no `sp` chamada pelo retorno (`ret`) pode ser modificada pela nossa entrada.

Para fazer isso:
- Adicione um ponto de interrupção (breakpoint) em `0x452a` e execute-o. 
- Verifique a posição do `sp` na memória.
- Se a posição estiver entre `0x3cb8` e `0x3ce7`, podemos alterar o endereço de retorno.

Para nossa sorte, a posição de retorno é em `0x3cc8`, uma posição que nossa entrada pode alterar.

Com essa informação, vamos modificar o retorno para outro local, vamos para `0x451c`. Após alguns testes, nossa entrada ficará assim:

```
666666666666666666666666666666661c45
```

Essa entrada fará o salto para a linha `0x451c`, no texto "Acesso Concedido". Mesmo com esse salto, nossa porta ainda não está destrancada. Desta vez, tentaremos retornar para a função `<INT>`, talvez executando-a duas vezes nos permitirá manipular melhor o fluxo do programa. Nossa próxima entrada é:

```
666666666666666666666666666666663245
```

Após o redirecionamento para o endereço `0x4532` (`<INT>`), a pilha armazenará ainda mais dados, fazendo com que o valor em `r14` e `r15` seja lido novamente, mas agora em uma posição que nossa entrada pode manipular. Esses valores serão adicionados ao ponteiro `sr`, então talvez possamos fazer com que `sr` seja `0xff00` para destrancar a porta.

A segunda leitura ocorre 3 bytes após o último valor de retorno que modificamos. Por isso, adicionaremos 2 bytes zero (`0000`) à nossa entrada e, em seguida, adicionaremos o valor de desbloqueio ao `sr` (`0xff00`). A entrada será algo como:

```
6666666666666666666666666666666632450000ff00
```

Com isso, retornamos à função `<INT>`, adicionamos mais valores à pilha, e modificamos o `sr` para ser `0xff00` para destrancar a porta.
