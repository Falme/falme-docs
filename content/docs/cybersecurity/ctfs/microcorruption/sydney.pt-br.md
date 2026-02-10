---
title: "Sydney"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Sydney

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Primeiramente, vamos verificar o que a função `main` faz:

```nasm
main:
	add		#0xff9c, sp
	mov		#0x44b4 "Enter the password to continue.", r15
	call	#0x4566 <puts>
	mov		sp, r15
	call	#0x4480 <get_password>
	mov		sp, r15
	call	#0x448a <check_password>
	tst		r15
	jnz		$+0xc <main+0x26>
	mov		#0x44d4 "Invalid password; try again.", r15
	call	#0x4566 <puts>
	jmp		$+0x14 <main+0x38>
	mov		#0x44f1 "Access Granted!", r15
	call	#0x4566 <puts>
	push	#0x7f
	call	#0x4502 <INT>
	incd	sp
	clr		r15
	add		#0x64, sp
```

Em poucos passos, podemos ver que:
- Escreve um texto solicitando a senha.
- pede uma senha
- compara a senha

A função que imprime na tela e a função `get_password` não têm nada de incomum, então vamos analisar a função `check_password` com mais detalhes:

```nasm
check_password:
	cmp		#0x5b7b, 0x0(r15)
	jnz		$+0x1c <check_password+0x22>
	cmp		#0x7477, 0x2(r15)
	jnz		$+0x14 <check_password+0x22>
	cmp		#0x703d, 0x4(r15)
	jnz		$+0xc <check_password+0x22>
	mov		#0x1, r14
	cmp		#0x2355, 0x6(r15)
	jz		$+0x4 <check_password+0x24>
	clr		r14
	mov		r14, r15
	ret
```

Parece que ele compara um byte literal (#) com os dados de memória do registro r15. Então, ele faz algo como:
- Verifica se os bytes em (r15 + 0 bytes) são iguais a 0x5b7b
- Verifica se os bytes em (r15 + 2 bytes) são iguais a 0x7477
- Verifica se os bytes em (r15 + 4 bytes) são iguais a 0x703d
- Verifica se os bytes em (r15 + 6 bytes) são iguais a 0x2355

A posição r15 na memória é onde nossa senha está armazenada. Então, em teoria, se inserirmos esses bytes na ordem (`5b7b7477703d2355`), o cadeado abrirá, certo? Bem, há um problema.

Estamos lendo byte a byte a partir do endereço de memória, o que significa que os bytes devem ser invertidos:
- 0x5b7b torna-se 0x7b5b
- 0x7477 torna-se 0x7774
- 0x703d torna-se 0x3d70
- 0x2355 torna-se 0x5523

Se inserirmos a sequência correta, teremos a resposta final para o nosso desafio.

{{% details title="Answer:Sydney" open=false %}}
```
7b5b77743d705523
```
{{% /details %}}
