---
title: "Reykjavik"
weight: 5
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Reykjavik

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Primeiramente, vamos verificar a função `main`:

```
4438 <main>
4438:  3e40 2045      mov	#0x4520, r14
443c:  0f4e           mov	r14, r15
443e:  3e40 f800      mov	#0xf8, r14
4442:  3f40 0024      mov	#0x2400, r15
4446:  b012 8644      call	#0x4486 <enc>
444a:  b012 0024      call	#0x2400
444e:  0f43           clr	r15
```

Após mover alguns dados, a função `main` faz 2 chamadas: `0x4486 <enc>` e `#0x2400`.

O curioso é que a função `enc` está na lista do disassembly, mas a chamada em `0x2400` não está disponível antes de executar o script. 

Se analisarmos a função `enc`, podemos ver que ela reorganiza alguns bytes, primeiro listando todos os bytes possíveis e depois reordenando-os na memória. Podemos supor que a função `enc` significa "encryption" (criptografar) e que ela reorganiza os bytes para formar um novo código, um código que será chamado posteriormente pela função `main`.

Logo após chamar a função `enc`, a chamada para o endereço `0x2400` será escrita na memória e poderá ser chamada novamente.

Então, adicione um ponto de interrupção na chamada `#0x2400`:

```
444a:  b012 0024      call	#0x2400 <-- here
```

Em seguida, podemos ver que a função `enc` escreveu na memória entre os endereços `0x2400` e `0x2570`. Se copiarmos a memória nesse intervalo e acessarmos a página [(DIS)ASSEMBLER](https://microcorruption.com/assembler), podemos inserir os valores da memória no disassembler para nos mostrar o código em assembly, que podemos ler e entender o que ele faz.

### Disassembly do Bloco de Memória

O disassembly exibirá muitas linhas de código, o que pode parecer intimidante, mas podemos analisá-las passo a passo. Ainda podemos usar a ação "step" no programa e observar a "Current Instruction" (no canto superior direito da página), que mostra a instrução atual em assembly.

Se analisarmos o código do programa procurando pelo comando `cmp`, encontraremos apenas duas ocorrências. Isso significa que apenas dois locais comparam algo para verificar se a senha está correta ou incorreta. Os dois locais são:

```
b490 45ea dcff cmp	#0xea45, -0x24(r4)
fa9b 6674      cmp.b	@r11+, 0x7466(r10)
```

As comparações são:
- #0xea45, -0x24(r4)
	- Valor literal `0xea45` comparando com o endereço `-0x24(r4)`
- @r11+, 0x7466(r10)
	- valor `@r11+` comparando com o endereço `0x7466(r10)`

Primeiro, vamos tentar inserir o valor literal da primeira comparação. Então, vamos inserir os valores `0x45ea` (invertido de `0xea45` em formato little endian). 

```
45ea45ea45ea45ea45ea45ea45ea45ea45ea45ea45ea
```

Após inserir o código, a porta trancada se abre. Encontramos a solução.

Mas talvez possamos encurtar um pouco? Vamos tentar com apenas `45ea`:

```
45ea
```

Isso também abre a porta trancada, fica mais limpo e bonito.
