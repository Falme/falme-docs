---
title: "Santa Cruz"
weight: 9
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Santa Cruz

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Como sempre, vamos verificar a função `<main>`:

```
4438 <main>
4438:  3150 ceff      add	#0xffce, sp
443c:  b012 5045      call	#0x4550 <login>
```

A função `<main>` só chama a função `<login>`. Vamos verificar essa também:

```
4550 <login>
4550:  0b12           push	r11
4552:  0412           push	r4
4554:  0441           mov	sp, r4
4556:  2452           add	#0x4, r4
4558:  3150 d8ff      add	#0xffd8, sp
455c:  c443 faff      mov.b	#0x0, -0x6(r4)
4560:  f442 e7ff      mov.b	#0x8, -0x19(r4)
4564:  f440 1000 e8ff mov.b	#0x10, -0x18(r4)
456a:  3f40 8444      mov	#0x4484 "Authentication now requires a username and password.", r15
456e:  b012 2847      call	#0x4728 <puts>
4572:  3f40 b944      mov	#0x44b9 "Remember: both are between 8 and 16 characters.", r15
4576:  b012 2847      call	#0x4728 <puts>
457a:  3f40 e944      mov	#0x44e9 "Please enter your username:", r15
457e:  b012 2847      call	#0x4728 <puts>
4582:  3e40 6300      mov	#0x63, r14
4586:  3f40 0424      mov	#0x2404, r15
458a:  b012 1847      call	#0x4718 <getsn>
458e:  3f40 0424      mov	#0x2404, r15
4592:  b012 2847      call	#0x4728 <puts>
4596:  3e40 0424      mov	#0x2404, r14
459a:  0f44           mov	r4, r15
459c:  3f50 d6ff      add	#0xffd6, r15
45a0:  b012 5447      call	#0x4754 <strcpy>
45a4:  3f40 0545      mov	#0x4505 "Please enter your password:", r15
45a8:  b012 2847      call	#0x4728 <puts>
45ac:  3e40 6300      mov	#0x63, r14
45b0:  3f40 0424      mov	#0x2404, r15
45b4:  b012 1847      call	#0x4718 <getsn>
45b8:  3f40 0424      mov	#0x2404, r15
45bc:  b012 2847      call	#0x4728 <puts>
45c0:  0b44           mov	r4, r11
45c2:  3b50 e9ff      add	#0xffe9, r11
45c6:  3e40 0424      mov	#0x2404, r14
45ca:  0f4b           mov	r11, r15
45cc:  b012 5447      call	#0x4754 <strcpy>
45d0:  0f4b           mov	r11, r15
45d2:  0e44           mov	r4, r14
45d4:  3e50 e8ff      add	#0xffe8, r14
45d8:  1e53           inc	r14
45da:  ce93 0000      tst.b	0x0(r14)
45de:  fc23           jnz	$-0x6 <login+0x88>
45e0:  0b4e           mov	r14, r11
45e2:  0b8f           sub	r15, r11
45e4:  5f44 e8ff      mov.b	-0x18(r4), r15
45e8:  8f11           sxt	r15
45ea:  0b9f           cmp	r15, r11
45ec:  0628           jnc	$+0xe <login+0xaa>
45ee:  1f42 0024      mov	&0x2400, r15
45f2:  b012 2847      call	#0x4728 <puts>
45f6:  3040 4044      br	#0x4440 <__stop_progExec__>
45fa:  5f44 e7ff      mov.b	-0x19(r4), r15
45fe:  8f11           sxt	r15
4600:  0b9f           cmp	r15, r11
4602:  062c           jc	$+0xe <login+0xc0>
4604:  1f42 0224      mov	&0x2402, r15
4608:  b012 2847      call	#0x4728 <puts>
460c:  3040 4044      br	#0x4440 <__stop_progExec__>
4610:  c443 d4ff      mov.b	#0x0, -0x2c(r4)
4614:  3f40 d4ff      mov	#0xffd4, r15
4618:  0f54           add	r4, r15
461a:  0f12           push	r15
461c:  0f44           mov	r4, r15
461e:  3f50 e9ff      add	#0xffe9, r15
4622:  0f12           push	r15
4624:  3f50 edff      add	#0xffed, r15
4628:  0f12           push	r15
462a:  3012 7d00      push	#0x7d
462e:  b012 c446      call	#0x46c4 <INT>
4632:  3152           add	#0x8, sp
4634:  c493 d4ff      tst.b	-0x2c(r4)
4638:  0524           jz	$+0xc <login+0xf4>
463a:  b012 4a44      call	#0x444a <unlock_door>
463e:  3f40 2145      mov	#0x4521 "Access granted.", r15
4642:  023c           jmp	$+0x6 <login+0xf8>
4644:  3f40 3145      mov	#0x4531 "That password is not correct.", r15
4648:  b012 2847      call	#0x4728 <puts>
464c:  c493 faff      tst.b	-0x6(r4)
4650:  0624           jz	$+0xe <login+0x10e>
4652:  1f42 0024      mov	&0x2400, r15
4656:  b012 2847      call	#0x4728 <puts>
465a:  3040 4044      br	#0x4440 <__stop_progExec__>
465e:  3150 2800      add	#0x28, sp
4662:  3441           pop	r4
4664:  3b41           pop	r11
4666:  3041           ret
```

Caramba, essa função é enorme. Vamos ver o que ela faz:

- Imprime na tela alguns textos solicitando o nome de usuário com `<puts>`
- Define a posição na memória a ser escrita para `0x2404`
- Solicita um nome de usuário por meio de um prompt usando `<getsn>`.
- Imprime na tela o nome de usuário digitado
- Define uma nova posição na memória para copiar o nome de usuário usando `<strcpy>`
- Define a posição na memória a ser escrita para `0x2404`
- Solicita uma senha por meio de um prompt usando `<getsn>`.
- Imprime na tela a senha digitada
- Define uma nova posição na memória para copiar a senha usando `<strcpy>`
- Leia todos os valores da entrada até encontrar um byte nulo (`0x00`)
- Se for muito longa, encerra o programa e exibe a mensagem "the password is too long".
- Caso contrário, verifica se a senha está correta. Se não estiver, encerre o programa e imprime "That password is not correct".
- Se a senha estiver correta, chama a função `<unlock_door>`, abrindo a porta e resolvendo o problema.

Sabendo disso, vamos verificar o quão longo nossas informações ficam registradas na memória, adicionando vários '6's ao nome de usuário e senha.

Após alguns testes, podemos constatar que:

- O nome de usuário:
	- Antes da cópia, escreve entre os endereços `0x2404` e `0x2466`.
	- Após a cópia, escreve entre os endereços `0x43a2` e `0x4404`.
- A senha:
	- Antes da cópia, escreve entre os endereços `0x2404` e `0x2466`.
	- Após a cópia, escreve entre os endereços `0x43b5` e `0x4417`.

A distância tem aproximadamente `0x63` bytes de tamanho, provavelmente definida antes de cada chamada para `<getsn>` no registrador `r14`:

```
4582:  3e40 6300      mov	#0x63, r14
```

### Como funciona o strcpy

Após inserirmos nossa entrada (usuário e senha), os valores são copiados para outro endereço. Aqui está a função:

```
4754 <strcpy>
4754:  0d4f           mov	r15, r13
4756:  023c           jmp	$+0x6 <strcpy+0x8>
4758:  1e53           inc	r14
475a:  1d53           inc	r13
475c:  6c4e           mov.b	@r14, r12
475e:  cd4c 0000      mov.b	r12, 0x0(r13)
4762:  4c93           tst.b	r12
4764:  f923           jnz	$-0xc <strcpy+0x4>
4766:  3041           ret
```

É bem simples, ele obtem uma posição de endereço, move um byte de um endereço para outro até que o byte copiado não seja zero e, se o byte for zero, retornar.

Isso significa que, se inserirmos um valor como:

```
6666009999
```

Os valores `9999` dessa entrada não serão copiados, porque há um byte nulo/zero (`0x00`), e a função de cópia `<strcpy>` retorna após encontrar o byte zero.

Isso impõe uma limitação ao nosso ataque: devemos ter cautela ao usar `00` em nossa entrada.


### Como a porta se abre

Ao contrário de alguns outros desafios, este realmente se abre na chamada `<unlock_door>` em `<login>`.

```
463a:  b012 4a44      call	#0x444a <unlock_door>
```

Portanto, nosso objetivo é alcançar essa linha ou fazer uma possível chamada direta para `<unlock_door>`.


### Como limita o tamanho da entrada (de 8 a 16 caracteres)

Após verificar a área onde o nome de usuário e a senha foram copiados, podemos encontrar dois bytes isolados em `0x43b3` e `0x43b4`, com os valores `0x08` e `0x10` respectivamente.

Que coincidência que `0x08` seja 8 em decimal e `0x10` seja 16 em decimal. Se precisarmos inserir um nome de usuário e senha mais longos, teremos que alterar esse valor. Imagine que alteremos o primeiro valor para `0x01` e o segundo para `0x30`, isso nos dará mais possibilidades de manipulação da memória.


### As verificações e comparações para atingir nosso objetivo

O que sabemos é que, para atingirmos nosso objetivo, precisamos:

- Passar na verificação "A senha é muito longa".
- Passar na verificação "Essa senha está incorreta".
- OU, no final da função `<login>`, faça com que ela retorne à função `<unlock_door>`.

A verificação de "Senha muito longa" ocorre em `0x45ea` e a verificação de "Senha muito curta" ocorre em `0x4600`.

```
45ea:  0b9f           cmp	r15, r11
45ec:  0628           jnc	$+0xe <login+0xaa>
45ee:  1f42 0024      mov	&0x2400, r15
45f2:  b012 2847      call	#0x4728 <puts>
45f6:  3040 4044      br	#0x4440 <__stop_progExec__>
45fa:  5f44 e7ff      mov.b	-0x19(r4), r15
45fe:  8f11           sxt	r15

4600:  0b9f           cmp	r15, r11
4602:  062c           jc	$+0xe <login+0xc0>
4604:  1f42 0224      mov	&0x2402, r15
4608:  b012 2847      call	#0x4728 <puts>
460c:  3040 4044      br	#0x4440 <__stop_progExec__>
```

Se pudermos modificar o número mínimo e máximo de caracteres para o usuário e senha, isso deixará de ser um problema. Portanto, para o nome de usuário, podemos escrever uma entrada como: 

```
66666666666666666666666666666666660130
```

Definindo o valor mínimo como `0x01` e o máximo como `0x30`.

Agora, para a verificação "Essa senha está incorreta", devo dizer de antemão que não vamos torná-la verdadeira, vamos falhar nessa verificação para que possamos retornar diretamente para a função `<unlock_door>`.

A posição de retorno da função `<login>` é em `0x43c0`, uma posição onde a nossa entrada alcança. Precisamos apenas modificar esses valores.

O endereço da função `<unlock_door>` é `0x444a`. Invertemos o endereço devido à ordem dos bytes (endianness), portanto, a entrada será algo como:

```
6666666666666666666666666666666666013066666666666666666666666666666666666666666666664a44
```

Só mais uma coisa: a última verificação no endereço `0x4650` verifica se o endereço é zero (`0x00`). Caso contrário, o programa simplesmente encerra e não chama o retorno. Devemos ter zero (`0x00`) no endereço de memória `0x4650`. Mas não podemos adicionar um zero no meio da entrada, certo?

Por isso que este problema tem duas entradas, nome de usuário e senha, e vamos fazê-las funcionar juntas. O nome de usuário substituirá a maioria dos valores, e a senha apenas adicionará o zero necessário em `0x4650`. O nome de usuário e a senha serão os seguintes:

```
user:
6666666666666666666666666666666666013066666666666666666666666666666666666666666666664a44

pass:
6666666666666666666666666666666666
```

Mas não há nenhum zero na senha? Sim, o byte zero será adicionado automaticamente, só precisamos saber onde parar de escrever.

Depois de tudo isso, inserimos o nome de usuário e a senha, e a porta se abre. Parabéns!
