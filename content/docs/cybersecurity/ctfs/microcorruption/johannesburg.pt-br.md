---
title: "Johannesburg"
weight: 8
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Johannesburg

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Como sempre, primeiro vamos verificar a função `main`:

```
4438 <main>
	call	#0x452c <login>
```

A única coisa que faz é chamar a função `login`, então vamos verificar essa função também:

```
452c <login>
	add		#0xffee, sp
	mov.b	#0xaa, 0x11(sp)
	mov		#0x447c "Enter the password to continue.", r15
	call	#0x45f8 <puts>
	mov		#0x449c "Remember: passwords are between 8 and 16 characters.", r15
	call	#0x45f8 <puts>
	mov		#0x3f, r14
	mov		#0x2400, r15
	call	#0x45e8 <getsn>
	mov		#0x2400, r14
	mov		sp, r15
	call	#0x4624 <strcpy>
	mov		sp, r15
	call	#0x4452 <test_password_valid>
	tst		r15
	jz		$+0xc <login+0x44>
	call	#0x4446 <unlock_door>
	mov		#0x44d1 "Access granted.", r15
	jmp		$+0x6 <login+0x48>
	mov		#0x44e1 "That password is not correct.", r15
	call	#0x45f8 <puts>
	cmp.b	#0xaa, 0x11(sp)
	jz		$+0xe <login+0x60>
	mov		#0x44ff "Invalid Password Length: password too long.", r15
	call	#0x45f8 <puts>
	br		#0x443c <__stop_progExec__>
	add		#0x12, sp
	ret
```

Então, o que ele faz, em ordem, é:

- Imprime um texto solicitando a senha com `<puts>`
- Solicitação para inserir a senha
- copia uma string para outro local `<strcpy>` (significando "string copy")
- Testar se a senha é válida `<test_password_valid>`
- Se a senha estiver correta, abra a porta.
- Se a senha estiver incorreta, encerra o programa.

Sabendo disso, vejamos qual parte do programa podemos alcançar usando um buffer overflow. Vamos digitar o número `6` várias vezes no campo de entrada de senha:

```
6666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666
```

Após colocar a senha e a executar o comando `step`, podemos ver que nossa entrada está sendo escrita na memória entre os endereços `2400` e `243e`.

Agora, vamos analisar a função `<strcpy>` para ver o que ela faz:

```
4624 <strcpy>
	mov		r15, r13
	jmp		$+0x6 <strcpy+0x8>
	inc		r14
	inc		r13
	mov.b	@r14, r12
	mov.b	r12, 0x0(r13)
	tst.b	r12
	jnz		$-0xc <strcpy+0x4>
	ret
```

Após alguns testes, podemos ver que ele copia nossa senha para outro endereço na memória. A escrita na memória ocorre entre os endereços `43ec` e `442a`.

O problema reside no `<test_password_valid>`. Ele nunca verifica os valores que inserimos como senha. Precisamos encontrar outro local.

Vamos verificar as comparações (`cmp`) novamente, talvez possamos encontrar uma maneira útil de chegar à nossa solução.

Na função `login` podemos encontrar uma comparação que é verificada mesmo se nossa senha estiver incorreta:

```
	cmp.b	#0xaa, 0x11(sp)
	jz		$+0xe <login+0x60>
	mov		#0x44ff "Invalid Password Length: password too long.", r15
	call	#0x45f8 <puts>
	br		#0x443c <__stop_progExec__>
	add		#0x12, sp
	ret
```

Este comando verifica se a senha é muito longa, mas também faz uma comparação literal com o byte `0xaa`, então vamos adicionar vários `0xaa` na senha para ver o que acontece:


```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

Desta vez, o código não chegará ao `<__stop_progExec__>` e interromperá o programa, na verdade, ele irá para `add #0x12, sp` e retornará (`ret`). Se verificarmos a posição do ponteiro `sp` na memória após essa adição, podemos ver que ele aparece entre os endereços `43ec` e `442a`, os endereços onde a função `<strcpy>` copiou nossa senha.

Como você deve saber, a operação `ret` recebe um valor do `sp` (stack pointer) sabendo para onde retornar. Talvez se inserirmos o endereço da função para destrancar a porta nessa posição do `sp`, possamos resolver este problema. 

Esta é a função para destrancar a porta:

```
4446 <unlock_door>
	push	#0x7f
	call	#0x4594 <INT>
	incd	sp
	ret
```

A função para abrir a porta está em `0x4446`, devido à ordem dos bytes (endianness), vamos inseri-la como `0x4644`. A entrada deve ser algo como:

```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa4644
```

Ao inserir essa senha, a porta se abrirá.


