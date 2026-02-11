---
title: "Cusco"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Cusco

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Primeiramente, a função `main` apenas redireciona para a função `login`, então vamos verificar o que ela faz:

```nasm
login:
	add		#0xfff0, sp
	mov		#0x447c "Enter the password to continue.", r15
	call	#0x45a6 <puts>
	mov		#0x449c "Remember: passwords are between 8 and 16 characters.", r15
	call	#0x45a6 <puts>
	mov		#0x30, r14
	mov		sp, r15
	call	#0x4596 <getsn>
	mov		sp, r15
	call	#0x4452 <test_password_valid>
	tst		r15
	jz		$+0xc <login+0x32>
	call	#0x4446 <unlock_door>
	mov		#0x44d1 "Access granted.", r15
	jmp		$+0x6 <login+0x36>
	mov		#0x44e1 "That password is not correct.", r15
	call	#0x45a6 <puts>
	add		#0x10, sp
	ret
```

A função `login` faz o seguinte:
- Escreve um texto solicitando uma senha (de 8 a 16 caracteres)
- pede uma senha
- Verifica se a senha é válida
- Escreve o resultado (se foi bem-sucedido ou não)
- finaliza a função com um retorno

O console pede uma senha entre 8 e 16 caracteres. Vamos verificar se 16 caracteres é o limite para a nossa entrada. Vou digitar uma sequência de setes e depois verificar a memória:

```
43e0: 5645 0000 a045 0200 ee43 3000 1e45 3737   VE...E...C0..E77
43f0: 3737 3737 3737 3737 3737 3737 3737 3737   7777777777777777
4400: 3737 3737 3737 3737 3737 3737 3737 3737   7777777777777777
4410: 3737 3737 3737 3737 3737 3737 3737 0045   77777777777777.E
```

Bem, na verdade podemos escrever 48 caracteres nesta entrada, muito acima do esperado, indo do endereço `0x43ee` ao endereço `0x441d`.

O problema é que, de nenhuma forma a função `test_password_valid` interage com nosso intervalo de entrada. Mesmo se você rastrear o r15 para ver o que ele faz, a comparação parece nunca acontecer:

```nasm
call	#0x4452 <test_password_valid>
tst		r15
jz		$+0xc <login+0x32>
call	#0x4446 <unlock_door>
mov		#0x44d1 "Access granted.", r15
```

Em outras palavras, o registro `r15` é sempre zero, fazendo com que ele ignore a função `unlock_door`, que é a que desejamos.

Mas, depois disso, o programa parece travar. Ok, vamos verificar novamente.

O que podemos ver é que a operação RET (retorno) está chamando o retorno no endereço `0x43fe`, o que significa duas coisas.

1. Se inserirmos mais de 16 caracteres na senha, esses dados serão corrompidos pelos nossos setes.
2. A chamada de posição de retorno está entre `0x43ee` e `0x441d`, tornando possível manipularmos esse retorno.

Então, vamos tentar alterar o retorno para nos direcionar à função `unlock_door`. Primeiro, vamos encontrar o endereço da função `unlock_door`:

```nasm
4446 <unlock_door>
4446:  3012 7f00      push	#0x7f
444a:  b012 4245      call	#0x4542 <INT>
444e:  2153           incd	sp
4450:  3041           ret
```

Portanto, o endereço é `0x4446`, vamos inserir esse endereço na posição 17 dos bytes (16 bytes de zeros + 1 byte do endereço `unlock_door`).

Lembre-se de inverter os bytes, o endereço é `0x4446`, mas nossa entrada será `0x4644`:

```
000000000000000000000000000000004644
```

Com isso, a operação de retorno da função `login` retornará para a função `unlock_door`, abrindo a porta.

