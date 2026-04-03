---
title: "Addis Ababa"
weight: 11
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Addis Ababa

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

Primeiramente, vamos verificar a função `main`:

```
4438 <main>
4438:  3150 eaff      add	#0xffea, sp
443c:  8143 0000      clr	0x0(sp)
4440:  3012 e644      push	#0x44e6
4444:  b012 c845      call	#0x45c8 <printf>
4448:  b140 1b45 0000 mov	#0x451b ">> ", 0x0(sp)
444e:  b012 c845      call	#0x45c8 <printf>
4452:  2153           incd	sp
4454:  3e40 1300      mov	#0x13, r14
4458:  3f40 0024      mov	#0x2400, r15
445c:  b012 8c45      call	#0x458c <getsn>
4460:  0b41           mov	sp, r11
4462:  2b53           incd	r11
4464:  3e40 0024      mov	#0x2400, r14
4468:  0f4b           mov	r11, r15
446a:  b012 de46      call	#0x46de <strcpy>
446e:  3f40 0024      mov	#0x2400, r15
4472:  b012 b044      call	#0x44b0 <test_password_valid>
4476:  814f 0000      mov	r15, 0x0(sp)
447a:  0b12           push	r11
447c:  b012 c845      call	#0x45c8 <printf>
4480:  2153           incd	sp
4482:  3f40 0a00      mov	#0xa, r15
4486:  b012 5045      call	#0x4550 <putchar>
448a:  8193 0000      tst	0x0(sp)
448e:  0324           jz	$+0x8 <main+0x5e>
4490:  b012 da44      call	#0x44da <unlock_door>
4494:  053c           jmp	$+0xc <main+0x68>
4496:  3012 1f45      push	#0x451f
449a:  b012 c845      call	#0x45c8 <printf>
449e:  2153           incd	sp
44a0:  0f43           clr	r15
44a2:  3150 1600      add	#0x16, sp
```

Então, vamos analisar o que ele faz:
- Exibe na tela a mensagem solicitando nome de usuário e senha com `<printf>`
- Define o limite de caracteres de input para `0x13`, e define a posição pra ser escrita na memória em `0x2400`.
- Abre a Janela solicitando nome de usuário e senha com `<getsn>`
- Copia a string do input de `0x2400` para `0x3c6e` usando `<strcpy>`
- Testa se a senha é válida com `<test_password_valid>`
- Reescreve nosso input na tela usando `<printf>`
- Adiciona alguns `0xa` ao stack frame com `<putchar>`
- Se os bytes em `0x3c6c` forem zero, imprime que a senha está incorreta.
- Se os bytes em `0x3c6c` não forem zero, destranca a porta.

Bem, com essa informação, vamos verificar as possibilidades:


### Até onde nosso Input vai?

Como já foi dito, estamos limitados a `0x13` caracteres de entrada (ou 19 caracteres em decimal).

Talvez possamos usar o retorno em `<main>` e o redirecionar para a função `<unlock_door>`. Isso criaria uma solução comum, mas talvez funcione.

Mas há um problema. Se verificarmos a função `<main>`, nenhuma chamada de retorno (`return`) é feita, ela simplesmente "vaza" para a próxima função `<__stop_progExec__>`, fazendo com que o programa pare. Portanto, isso não é uma possibilidade.


### O estranho na casa: printf

Essa é uma função incomum. Na maioria dos desafios anteriores, era usado `<puts>` para imprimir as strings e informações, então por que mudar?

Isso me leva a crer que a solução está nesta função. Esta é uma função comum/famosa na biblioteca C, então estamos lidando com uma compilação em C? Vamos verificar a função `printf` completa:

```
45c8 <printf>

45c8:  0b12           push	r11
45ca:  0a12           push	r10
45cc:  0912           push	r9
45ce:  0812           push	r8
45d0:  0712           push	r7
45d2:  0412           push	r4
45d4:  0441           mov	sp, r4
45d6:  3450 0c00      add	#0xc, r4
45da:  2183           decd	sp
45dc:  1b44 0200      mov	0x2(r4), r11
45e0:  8441 f2ff      mov	sp, -0xe(r4)
45e4:  0f4b           mov	r11, r15
45e6:  0e43           clr	r14
45e8:  0b3c           jmp	$+0x18 <printf+0x38>
45ea:  1f53           inc	r15
45ec:  7d90 2500      cmp.b	#0x25, r13
45f0:  0720           jnz	$+0x10 <printf+0x38>
45f2:  6d9f           cmp.b	@r15, r13
45f4:  0320           jnz	$+0x8 <printf+0x34>
45f6:  1f53           inc	r15
45f8:  0d43           clr	r13
45fa:  013c           jmp	$+0x4 <printf+0x36>
45fc:  1d43           mov	#0x1, r13
45fe:  0e5d           add	r13, r14
4600:  6d4f           mov.b	@r15, r13
4602:  4d93           tst.b	r13
4604:  f223           jnz	$-0x1a <printf+0x22>
4606:  0f4e           mov	r14, r15
4608:  0f5f           add	r15, r15
460a:  2f53           incd	r15
460c:  018f           sub	r15, sp
460e:  0941           mov	sp, r9
4610:  0c44           mov	r4, r12
4612:  2c52           add	#0x4, r12
4614:  0f41           mov	sp, r15
4616:  0d43           clr	r13
4618:  053c           jmp	$+0xc <printf+0x5c>
461a:  af4c 0000      mov	@r12, 0x0(r15)
461e:  1d53           inc	r13
4620:  2f53           incd	r15
4622:  2c53           incd	r12
4624:  0d9e           cmp	r14, r13
4626:  f93b           jl	$-0xc <printf+0x52>
4628:  0a43           clr	r10
462a:  3740 0900      mov	#0x9, r7
462e:  4a3c           jmp	$+0x96 <printf+0xfc>
4630:  084b           mov	r11, r8
4632:  1853           inc	r8
4634:  7f90 2500      cmp.b	#0x25, r15
4638:  0624           jz	$+0xe <printf+0x7e>
463a:  1a53           inc	r10
463c:  0b48           mov	r8, r11
463e:  8f11           sxt	r15
4640:  b012 5045      call	#0x4550 <putchar>
4644:  3f3c           jmp	$+0x80 <printf+0xfc>
4646:  6e48           mov.b	@r8, r14
4648:  4e9f           cmp.b	r15, r14
464a:  0620           jnz	$+0xe <printf+0x90>
464c:  1a53           inc	r10
464e:  3f40 2500      mov	#0x25, r15
4652:  b012 5045      call	#0x4550 <putchar>
4656:  333c           jmp	$+0x68 <printf+0xf6>
4658:  7e90 7300      cmp.b	#0x73, r14
465c:  0b20           jnz	$+0x18 <printf+0xac>
465e:  2b49           mov	@r9, r11
4660:  053c           jmp	$+0xc <printf+0xa4>
4662:  1a53           inc	r10
4664:  1b53           inc	r11
4666:  8f11           sxt	r15
4668:  b012 5045      call	#0x4550 <putchar>
466c:  6f4b           mov.b	@r11, r15
466e:  4f93           tst.b	r15
4670:  f823           jnz	$-0xe <printf+0x9a>
4672:  253c           jmp	$+0x4c <printf+0xf6>
4674:  7e90 7800      cmp.b	#0x78, r14
4678:  1c20           jnz	$+0x3a <printf+0xea>
467a:  2b49           mov	@r9, r11
467c:  173c           jmp	$+0x30 <printf+0xe4>
467e:  0f4b           mov	r11, r15
4680:  8f10           swpb	r15
4682:  3ff0 ff00      and	#0xff, r15
4686:  12c3           clrc
4688:  0f10           rrc	r15
468a:  0f11           rra	r15
468c:  0f11           rra	r15
468e:  0f11           rra	r15
4690:  1a53           inc	r10
4692:  079f           cmp	r15, r7
4694:  0338           jl	$+0x8 <printf+0xd4>
4696:  3f50 3000      add	#0x30, r15
469a:  023c           jmp	$+0x6 <printf+0xd8>
469c:  3f50 5700      add	#0x57, r15
46a0:  b012 5045      call	#0x4550 <putchar>
46a4:  0b5b           add	r11, r11
46a6:  0b5b           add	r11, r11
46a8:  0b5b           add	r11, r11
46aa:  0b5b           add	r11, r11
46ac:  0b93           tst	r11
46ae:  e723           jnz	$-0x30 <printf+0xb6>
46b0:  063c           jmp	$+0xe <printf+0xf6>
46b2:  7e90 6e00      cmp.b	#0x6e, r14
46b6:  0320           jnz	$+0x8 <printf+0xf6>
46b8:  2f49           mov	@r9, r15
46ba:  8f4a 0000      mov	r10, 0x0(r15)
46be:  2953           incd	r9
46c0:  0b48           mov	r8, r11
46c2:  1b53           inc	r11
46c4:  6f4b           mov.b	@r11, r15
46c6:  4f93           tst.b	r15
46c8:  b323           jnz	$-0x98 <printf+0x68>
46ca:  1144 f2ff      mov	-0xe(r4), sp
46ce:  2153           incd	sp
46d0:  3441           pop	r4
46d2:  3741           pop	r7
46d4:  3841           pop	r8
46d6:  3941           pop	r9
46d8:  3a41           pop	r10
46da:  3b41           pop	r11
46dc:  3041           ret
```

Essa é uma função longa e um tanto complexa (chata), porque ela pula de um lugar para outro com algumas verificações e outras sem.

Então, como vamos analisar essa função? Bem, primeiro vamos verificar as ramificações dessa função, em outras palavras, vamos listar as linhas "Compare" (`cmp`).

- `cmp.b 	#0x25, r13` => Compara r13 com o literal `0x25` (%)
- `cmp.b 	@r15, r13` => Compara r13 com @r15
- `cmp 		r14, r13` => Compara r13 com r14
- `cmp.b 	#0x25, r15` => Compara r15 com o literal `0x25` (%)
- `cmp.b 	r15, r14` => Compara r15 com r14
- `cmp.b 	#0x73, r14` => Compara r14 com o literal `0x73` (s)
- `cmp.b 	#0x78, r14` => Compara r14 com o literal `0x78` (x)
- `cmp 		r15, r7` => Compara r15 com r7
- `cmp.b 	#0x6e, r14` => Compara r14 com o literal `0x6e` (n)

Podemos começar verificando os caracteres ASCII literais, que são `%`, `s`, `x`, `n`. Como já mencionamos, esses são termos comuns para desenvolvedores em C.


### Specifier Characters

Você pode conferir os detalhes na documentação [Aqui](https://cplusplus.com/reference/cstdio/printf/), mas estamos tratando de:

- `%s` : Cadeia de caracteres (String)
- `%x` : Número inteiro em hexadecimal sem sinal (Unsigned hexadecimal integer)
- `%n`: Nada é impresso e o número de caracteres escritos até o momento é armazenado no local indicado.

Então, vamos dar um exemplo com `%s`:

```c
printf ("%s text here \n", "A string");
//       |                 |________|
//       +----------------------

// output : "A string text here"
```

Pegamos um argumento e a substituímos na string original. Isso também funciona com `%x`:

```c
printf ("100 decimal in hex : %x", 100);

// output : "100 decimal in hex : 64"
```

Esta função recebe uma informação e a lê como um valor hexadecimal. Por fim, o especificador `%n`:

```c
// cria uma variável int
int val;

// Metade A e metade B, armazene %n na variável val
printf("AA AA A %n BBBBBBBBBBBBB\n", &val);

// Imprime o valor de val
printf("val = %d \n", val);

// output : "val = 8"
```

O resultado é 8 porque `%n` está considerando o comprimento dos caracteres antes da invocação. Ele conta tudo o que vem antes dele ("AA AA A") e armazena em `val`, resultando em 8. Tudo o que vem depois de `%n` é ignorado, portanto, a seção 'B' não é contabilizada.

Sabendo disso, vamos fazer algumas experiências.

Tentei inserir alguns inputs aleatórios, algo como:

```
AAAAAA%s%sAAA
AAA%x%x%x%x%x
%x%s%n%s%x
```

E assim por diante, até que eu tentei:

```
AA%x%n%n%n
```

Isso gerou um erro ("load address unaligned: 4141") e o programa parou no meio do `<printf>`. Isso foi interessante, minhas suposições estavam certas, a solução está relacionado ao `printf`. Eu não sabia muito sobre C, então procurei por vulnerabilidades na função `printf`. Aqui está o que encontrei no site da [OWASP](https://owasp.org/www-community/attacks/Format_string_attack):

```
A seguir, apresentamos alguns parâmetros que podem ser usados e suas consequências:
•"%x" Lê dados da pilha (stack)
•"%s" Lê os caracteres da memória do processo atual.
•"%n" Escreve um int para lugares na memória do processo atual.
```

Isso me fez pensar: e se eu usasse isso para manipular a pilha (stack) e a memória?

### Modificando a memória e %x%n


No início da função `<printf>`, os valores dos registradores `r11`, `r10`, `r9`, `r8`, `r7` e `r4` são colocados na pilha. Esses valores serão usados posteriormente, e talvez algum input possa manipular o resultado. 

O que podemos ver com nosso último input (`AA%x%n%n%n`) é que o valor desalinhado era "4141", que em ASCII é "AA". Então, ele simplesmente tentou modificar o valor em `0x4141` na memória. Ok, talvez com isso possamos modificar algum valor.

Lembre-se: para chamar a função `<unlock_door>`, os bytes em `0x3c6c` não podem ser zero. Talvez possamos modificar esse endereço? Vamos tentar com a seguinte entrada:

```
l<%x%n%n%n
```

O `l<` é uma representação dos bytes `0x6c` e `0x3c` da tabela ASCII, que é o nosso endereço de destino corrigido para endianness.

Com isso, o valor em `0x3c6c` não é mais zero, fazendo com que a porta se abra.

Depois de um tempo eu tentei limpar um pouco o input, então a entrada final que usei para abrir a porta foi:

```
l<%x%n
```


