---
title: "Microcorruption - New Orleans"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - New Orleans

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Comece sempre pela função principal. A função `main` é o nosso ponto de partida. Podemos ver:

```nasm
main:
	add		#0xff9c, sp
	call	#0x447e <create_password>
	mov		#0x44e4 "Enter the password to continue", r15
	call	#0x4594 <puts>
	mov		sp, r15
	call	#0x44b2 <get_password>
	mov		sp, r15
	call	#0x44bc <check_password>
	tst		r15
	jnz		$+0xc <main+0x2a>
	mov		#0x4503 "Invalid password; try again.", r15
	call	#0x4594 <puts>
	jmp		$+0xe <main+0x36>
	mov		#0x4520 "Access Granted!", r15
	call	#0x4594 <puts>
	call	#0x44d6 <unlock_door>
	clr		r15
	add		#0x64, sp
```

Ao ler as instruções, podemos ver os seguintes comandos:

- Cria uma senha
- Solicitar uma senha
- Compara a senha

O que me chama a atenção é que o programa está criando uma senha em tempo de execução, isso significa que ela está sendo armazenada na memória? Primeiro, vamos verificar a função `create_password`.

```nasm
create_password:
	mov		#0x2400, r15
	mov.b	#0x2d, 0x0(r15)
	mov.b	#0x41, 0x1(r15)
	mov.b	#0x44, 0x2(r15)
	mov.b	#0x3e, 0x3(r15)
	mov.b	#0x5d, 0x4(r15)
	mov.b	#0x6b, 0x5(r15)
	mov.b	#0x63, 0x6(r15)
	mov.b	#0x0, 0x7(r15)
	ret
```

A função `create_password` está, na verdade, escrevendo a senha na memória, linha por linha em hexadecimal, até atingir o valor `0x0`, que indica o byte nulo, como o fim da senha. 

Se fizermos uma anotação dos seguintes bytes e habilitarmos o Hex Encoded Input, poderemos inserir nossa senha e concluir este nível.

Lembre-se de não adicionar o 0x0, pois ele não faz parte da senha, é apenas um indicador.

{{% details title="Senha:New Orleans" open=false %}}
```
2d41443e5d6b63
```
{{% /details %}}

