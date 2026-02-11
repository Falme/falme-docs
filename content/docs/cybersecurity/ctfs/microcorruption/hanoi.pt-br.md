---
title: "Hanoi"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Hanoi

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Primeiramente, vamos verificar a função `main`:

```nasm
main:
	call	#0x4520 <login>
	clr		r15
```

A função `main` apenas redireciona para outra função chamada `login`, vamos verificar essa também:

```nasm
login:
	mov.b	#0x0, &0x2410
	mov		#0x447e "Enter the password to continue.", r15
	call	#0x45de <puts>
	mov		#0x449e "Remember: passwords are between 8 and 16 characters.", r15
	call	#0x45de <puts>
	mov		#0x1c, r14
	mov		#0x2400, r15
	call	#0x45ce <getsn>
	mov		#0x2400, r15
	call	#0x4454 <test_password_valid>
	tst		r15
	jz		$+0x8 <login+0x32>
	mov.b	#0x2b, &0x2410
	mov		#0x44d3 "Testing if password is valid.", r15
	call	#0x45de <puts>
	cmp.b	#0x5d, &0x2410
	jnz		$+0x10 <login+0x50>
	mov		#0x44f1 "Access granted.", r15
	call	#0x45de <puts>
	call	#0x4448 <unlock_door>
	ret
	mov		#0x4501 "That password is not correct.", r15
	call	#0x45de <puts>
	ret
```


Essa é a sequência que estamos procurando, em termos simples, este código:
- Escreve um texto sobre precisar inserir uma senha (de 8 a 16 caracteres)
- pede uma senha
- Verifica se a senha é válida

Bem, o programa está pedindo para inserirmos uma senha entre 8 e 16 caracteres, talvez isso seja algum tipo de dica para nós?

Ao executar o código testando diferentes tipos e tamanhos de senha, podemos ver que nossa senha está sendo armazenada no endereço `0x2400`. Além disso, não estamos limitados a apenas 16 caracteres. A memória pode armazenar até 28 caracteres, bem acima dos 16 permitidos.

Também podemos ver que a comparação decisiva na função `login` está comparando um byte em `0x2410`:

```nasm
cmp.b	#0x5d, &0x2410
jnz		$+0x10 <login+0x50>
mov		#0x44f1 "Access granted.", r15
```

O endereço `0x2410` está sendo comparado ao byte `0x5d`. Se a comparação for correta (ou seja, se os valores forem iguais, zero), chegamos à mensagem "Access granted." e destravamos o Locker. O endereço `0x2410` está muito próximo do endereço onde nossa senha está armazenada (`0x2400`). Talvez possamos usar um buffer overflow para esse endereço?

Do endereço `0x2400` ao `0x2410` são 16 bytes, então vamos usar 16 bytes de 0x0 + 1 byte de 0x5d, assim:

```
000000000000000000000000000000005d
```

Com isso, simplesmente pulamos a comparação, e a senha real não importa, pois destrancamos o Locker.
