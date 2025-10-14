---
title: "Intro to Debuggers"
weight: 6
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Intro to Debuggers

Essa sessão da [Playlist PicoCTF](https://play.picoctf.org/playlists/2) se chama "Intro to Debuggers", os desafios vão se aprofundar em como usar um Debugger, especificamente o GDB, um debugger e disassembler bem conhecido. Depois de algumas explicações de como usar o GDB, nós podemos começar os desafios.

Eu também estarei usando uma Máquina Virtual com Ubuntu pra resolver esses desafios.

---

## GDB baby step 1

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this (file).

Em uma tradução livre seria algo como:

> Consegue descobrir o que estará no registo EAX no final da função main? Coloque a sua resposta no formato da flag: picoCTF{n}, onde n é o conteúdo do registo EAX em decimal. Se a resposta for 0x11, a sua flag seria picoCTF{17}. Faça disassemble desse (arquivo).

Então o que precisamos fazer é:

1. Baixar o arquivo (debugger0_a) pra fazer disassemble
2. Fazer disassemble da função main com o GDB
3. Descobrir o valor de EAX ao final da função main

Em resumo, nós precisamos adquirir o dump em assembly do arquivo, assim como nos desafios anteriores.

Então depois de baixar o arquivo, nós podemos fazer um disassemble da função principal com esses comandos no shell:

```shell
chmod +x ./debugger0_a
gdb ./debugger0_a
```

e dentro do gdb:

```shell
(gdb) disassemble main
```

Fazendo esses comandos, gdb retorna com o seguinte resultado:

```python
0x0000000000001129 <+0>:	endbr64
0x000000000000112d <+4>:	push   %rbp
0x000000000000112e <+5>:	mov    %rsp,%rbp
0x0000000000001131 <+8>:	mov    %edi,-0x4(%rbp)
0x0000000000001134 <+11>:	mov    %rsi,-0x10(%rbp)
0x0000000000001138 <+15>:	mov    $0x86342,%eax
0x000000000000113d <+20>:	pop    %rbp
0x000000000000113e <+21>:	ret
```

Eu acho meio difícil de ler nesse formato, talvez se a gente mudar para um que estamos acostumados, a sintaxe intel, pra isso faremos:

```shell
(gdb) set disassembly-flavor intel
(gdb) disassemble main
```

E então nos retornará o seguinte resultado:

```python
0x0000000000001129 <+0>:	endbr64
0x000000000000112d <+4>:	push   rbp
0x000000000000112e <+5>:	mov    rbp,rsp
0x0000000000001131 <+8>:	mov    DWORD PTR [rbp-0x4],edi
0x0000000000001134 <+11>:	mov    QWORD PTR [rbp-0x10],rsi
0x0000000000001138 <+15>:	mov    eax,0x86342
0x000000000000113d <+20>:	pop    rbp
0x000000000000113e <+21>:	ret
```

Muito melhor, agora, nós precisamos achar o resultado final do registro EAX. A parte mais engraçada é que só tem um comando que afeta EAX:

```python
0x0000000000001138 <+15>:	mov    eax,0x86342
```

Então o valor de EAX é `0x86342`.

Mas o que realmente precisamos é o valor em decimal, então vamos colocar esse valor no nosso script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py) pra ter a nossa resposta:

```python
python3 HexToDec.py 0x86342
```

Formatamos a resposta como picoCTF{resposta} e teremos a flag:

{{% details title="Answer: Flag 1" open=false %}}
```
picoCTF{549698}
```
{{% /details %}}

---

## GDB baby step 2

A descrição desse problema é o mesmo que o anterior, só que dessa vez nós estaremos brincando com breakpoints e loops no programa. O exercício vai começar a olhar sobre análise dinâmica.

Análise estática é o que já fizemos antes, olhando pelo código e entender o que ele faz. Análise Dinâmica estuda o programa e código pela sua execução e debugging (breakpoints).

Fazendo o mesmo que os exercícios anteriores, gerando um dump de assembly, nós temos esse resultado:

```python
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   rbp
0x000000000040110b <+5>:	mov    rbp,rsp
0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x14],edi
0x0000000000401111 <+11>:	mov    QWORD PTR [rbp-0x20],rsi
0x0000000000401115 <+15>:	mov    DWORD PTR [rbp-0x4],0x1e0da
0x000000000040111c <+22>:	mov    DWORD PTR [rbp-0xc],0x25f
0x0000000000401123 <+29>:	mov    DWORD PTR [rbp-0x8],0x0
0x000000000040112a <+36>:	jmp    0x401136 <main+48>
0x000000000040112c <+38>:	mov    eax,DWORD PTR [rbp-0x8]
0x000000000040112f <+41>:	add    DWORD PTR [rbp-0x4],eax
0x0000000000401132 <+44>:	add    DWORD PTR [rbp-0x8],0x1
0x0000000000401136 <+48>:	mov    eax,DWORD PTR [rbp-0x8]
0x0000000000401139 <+51>:	cmp    eax,DWORD PTR [rbp-0xc]
0x000000000040113c <+54>:	jl     0x40112c <main+38>
0x000000000040113e <+56>:	mov    eax,DWORD PTR [rbp-0x4]
0x0000000000401141 <+59>:	pop    rbp
0x0000000000401142 <+60>:	ret
```

Nós podemos ver 2 problemas que não vimos antes.

1. O registro EAX está sendo chamado em múltiplos lugares
2. Nós temos pulos (JUMPs) na instrução, indicando um possível loop

O loop pode ser encontrado pelo seguinte padrão:

Na linha main+51 nós temos uma comparação:

```python
0x0000000000401139 <+51>:	cmp    eax,DWORD PTR [rbp-0xc]
```

Se essa comparação for menor que o valor, pula para outra linha: JL (Jump if Less / Pula se for menor)
Então nesse caso, estamos checando se `[rpb-0xc]` é menor que `EAX` ([rpb-0xc] < EAX)

Se sim, nós pulamos para a linha main+38, então voltamos para uma das linhas acima e comparamos de novo. Dessa forma encontramos um padrão de loop.

```python
0x000000000040113c <+54>:	jl     0x40112c <main+38>
```

Então, com essa informação, nós podemos assegurar que não iremos contar cada vez que o valor em EAX é modificado no loop.

Na verdade, nós iremos até o fim do programa e ler o valor final de EAX, como uma boa análise dinâmica espera que façamos.

Primeiro, adicionamos um breakpoint no final da função main logo antes do return. A linha `main+59` é um bom lugar para fazer isso. Então vamos ao gdb e fazer o seguinte:

```shell
chmod +x ./debugger0_b
gdb ./debugger0_b
```

e dentro do gdb nós vamos definir um breakpoint, onde ele vai pausar a execução do programa:

```shell
(gdb) break *main+59
```

E rodar o programa normalmente pelo gdb debugger:

```shell
(gdb) run
```

Depois de um momento, o programa irá parar com a seguinte mensagem:

```python
Breakpoint 1, 0x0000000000401141 in main ()
```

Agora o programa está pausado na linha `main+59` como esperado, você pode checar o endereço, é o mesmo número. Agora nós podemos pedir pelo valor em EAX, que vai retornar com o valor da flag em hexadecimal e decimal.

```shell
(gdb) info registers eax
```

Nós formatamos o valor no picoCTF{resposta} e teremos a flag:

{{% details title="Resposta: Flag 2" open=false %}}
```
picoCTF{307019}
```
{{% /details %}}

---

## GDB baby step 3

> Now for something a little different. 0x2262c96b is loaded into memory in the main function. Examine byte-wise the memory that the constant is loaded in by using the GDB command x/4xb addr. The flag is the four bytes as they are stored in memory. If you find the bytes 0x11 0x22 0x33 0x44 in the memory location, your flag would be: picoCTF{0x11223344}. Debug this (file).

Em uma tradução livre seria algo como:

> Agora, algo um pouco diferente. 0x2262c96b é carregado na memória na função principal. Examine byte a byte a memória na qual a constante é carregada usando o comando GDB x/4xb addr. A flag são os quatro bytes armazenados na memória. Se encontrar os bytes 0x11 0x22 0x33 0x44 na localização da memória, o seu sinalizador será: picoCTF{0x11223344}. Depure isto (arquivo).

Depois de uma breve explicação de como ler a memória no GDB, o exercício é apresentado.

Nós precisamos achar os bytes no endereço de memória do arquivo `debugger0_c`. A dica é o valor `0x2262c96b` na função main.

Então primeiro de tudo, nós precisamos fazer disassemble da função main, mesma coisa de antes, resultando no seguinte código em assembly:

```python
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   rbp
0x000000000040110b <+5>:	mov    rbp,rsp
0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x14],edi
0x0000000000401111 <+11>:	mov    QWORD PTR [rbp-0x20],rsi
0x0000000000401115 <+15>:	mov    DWORD PTR [rbp-0x4],0x2262c96b
0x000000000040111c <+22>:	mov    eax,DWORD PTR [rbp-0x4]
0x000000000040111f <+25>:	pop    rbp
0x0000000000401120 <+26>:	ret
```

seguindo os comandos, o valor `0x2262c96b` é movido (MOV em <+15>) para o endereço de memória $RBP-0x4.

Primeiro, algumas explicações. O registro RBP se chama Register Base Pointer, ele aponta para a base do Stack Frame, e a adição de seus valores seguem a numeração negativa.

Então, normalmente, o RBP segue adicionando valores como RBP-0x4 ou RPB-0x8, ele está adicionando valors do RBP (Register Base Pointer) para o RSP (Register Stack Pointer).

Então nós estamos movendo o valor `0x2262c96b` para a posição do Register Base Pointer menos 4.

Adicione um breakpoint em main+25 e rode o programa:

```shell
(gdb) break *main+25
(gdb) run
```

E chame os valores dentro de RBP-0x4. onde `x/` é o comando pra chamar a leitura da memória, nós queremos 4 bytes (4) em hexadecimal (x) cada um no tamanho de byte (b) resultando no comando `4xb` .

```shell
(gdb) x/4xb $rbp-0x4
```

Resultando em:

```python
0x7fffffffddbc:	0x6b	0xc9	0x62	0x22
```

Bem, na verdade, o valor retornado está invertido de acordo com o valor original `0x2262c96b`. Isso acontece porque estamos lidando com Little endian.

Nós temos Big Endian e Little Endian. Irei adicionar duas imagens de um video (não está mais disponível) da C3rb3ru5d3d53c explicando isso visualmente:

### Big Endian

Normalmente o jeito "correto" de como lemos valores hexadecimal.

Onde

 - M : é o Byte Mais Significante (Most Significant Byte)
 - L : é o Byte Menos Significante (Least Significant Byte)

![Big Endian](/images/intro-to-debuggers/1.jpg)

### Little Endian

Normalmente o jeito "invertido" de como lemos valores hexadecimal.

![Little Endian](/images/intro-to-debuggers/2.jpg)

Então o exercício pede para nós escrevermos a flag do jeito que estamos lendo ela, então, como Little Endian, do jeito que está na tela.

É simplesmente um exercício pra ler um valor na memória e pra entender Little endian.

Nós pegamos o resultado e colocamos no picoCTF{resposta} e assim teremos a flag:

{{% details title="Resposta: Flag 3" open=false %}}
```
picoCTF{0x6bc96222}
```
{{% /details %}}

---

## GDB baby step 4

> main calls a function that multiplies eax by a constant. The flag for this challenge is that constant in decimal base. If the constant you find is 0x1000, the flag will be picoCTF{4096}. Debug this (file).

Em uma tradução livre seria algo como:

> A main chama uma função que multiplica eax por uma constante. A flag para este desafio é essa constante na base decimal. Se a constante que encontrar for 0x1000, a bandeira será picoCTF{4096}. Depure isto (arquivo).

Este exercício é muito simples, é para mostrar que podemos chamar e fazer disassemble de muitas funções, não apenas a main. Precisamos encontrar um número constante que multiplique por EAX.

Então, fazemos o mesmo que nos outros exercícios, disassemble a função main, retornando o seguinte:

```python
0x000000000040111c <+0>:	endbr64
0x0000000000401120 <+4>:	push   rbp
0x0000000000401121 <+5>:	mov    rbp,rsp
0x0000000000401124 <+8>:	sub    rsp,0x20
0x0000000000401128 <+12>:	mov    DWORD PTR [rbp-0x14],edi
0x000000000040112b <+15>:	mov    QWORD PTR [rbp-0x20],rsi
0x000000000040112f <+19>:	mov    DWORD PTR [rbp-0x4],0x28e
0x0000000000401136 <+26>:	mov    DWORD PTR [rbp-0x8],0x0
0x000000000040113d <+33>:	mov    eax,DWORD PTR [rbp-0x4]
0x0000000000401140 <+36>:	mov    edi,eax
0x0000000000401142 <+38>:	call   0x401106 <func1>
0x0000000000401147 <+43>:	mov    DWORD PTR [rbp-0x8],eax
0x000000000040114a <+46>:	mov    eax,DWORD PTR [rbp-0x4]
0x000000000040114d <+49>:	leave
0x000000000040114e <+50>:	ret
```

A linha main+38 chama outra função chamada "func1". Talvez a multiplicação esteja lá. Vamos dar uma olhada

```shell
(gdb) disassemble func1
```

Retorna o seguinte:

```python
0x0000000000401106 <+0>:	endbr64
0x000000000040110a <+4>:	push   rbp
0x000000000040110b <+5>:	mov    rbp,rsp
0x000000000040110e <+8>:	mov    DWORD PTR [rbp-0x4],edi
0x0000000000401111 <+11>:	mov    eax,DWORD PTR [rbp-0x4]
0x0000000000401114 <+14>:	imul   eax,eax,0x3269
0x000000000040111a <+20>:	pop    rbp
0x000000000040111b <+21>:	ret
```

Na linha func1+14 nós temos uma multiplicação com EAX, talvez o valor 0x3269 é o que precisamos.

Mas na verdade preciamos do valor em decimal, então colocamos esse valor no nosso script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py), assim tendo nossa resposta:

```python
python3 HexToDec.py 0x3269
```

E colocamos no formato picoCTF{resposta} e teremos a flag:


{{% details title="Resposta: Flag 4" open=false %}}
```
picoCTF{12905}
```
{{% /details %}}

---

## ASCII FTW

> This program has constructed the flag using hex ascii values. Identify the flag text by disassembling the program. You can download the file from here.

Em uma tradução livre seria algo como:

> Este programa constroi a flag usando valores hexadecimais ASCII. Identifique o texto da flag desmontando o programa. Pode baixar o arquivo aqui.

Este é o desafio do módulo. A missão é encontrar a flag com o GDB em bytes e, em seguida, convertê-la para ASCII/String.

Primeiro, vamos examinar a função main:

```python
0x0000555555555169 <+0>:	endbr64
0x000055555555516d <+4>:	push   rbp
0x000055555555516e <+5>:	mov    rbp,rsp
0x0000555555555171 <+8>:	sub    rsp,0x30
0x0000555555555175 <+12>:	mov    rax,QWORD PTR fs:0x28
0x000055555555517e <+21>:	mov    QWORD PTR [rbp-0x8],rax
0x0000555555555182 <+25>:	xor    eax,eax
0x0000555555555184 <+27>:	mov    BYTE PTR [rbp-0x30],0x70
0x0000555555555188 <+31>:	mov    BYTE PTR [rbp-0x2f],0x69
0x000055555555518c <+35>:	mov    BYTE PTR [rbp-0x2e],0x63
0x0000555555555190 <+39>:	mov    BYTE PTR [rbp-0x2d],0x6f
0x0000555555555194 <+43>:	mov    BYTE PTR [rbp-0x2c],0x43
0x0000555555555198 <+47>:	mov    BYTE PTR [rbp-0x2b],0x54
0x000055555555519c <+51>:	mov    BYTE PTR [rbp-0x2a],0x46
0x00005555555551a0 <+55>:	mov    BYTE PTR [rbp-0x29],0x7b
0x00005555555551a4 <+59>:	mov    BYTE PTR [rbp-0x28],0x41
0x00005555555551a8 <+63>:	mov    BYTE PTR [rbp-0x27],0x53
0x00005555555551ac <+67>:	mov    BYTE PTR [rbp-0x26],0x43
0x00005555555551b0 <+71>:	mov    BYTE PTR [rbp-0x25],0x49
0x00005555555551b4 <+75>:	mov    BYTE PTR [rbp-0x24],0x49
0x00005555555551b8 <+79>:	mov    BYTE PTR [rbp-0x23],0x5f
0x00005555555551bc <+83>:	mov    BYTE PTR [rbp-0x22],0x49
0x00005555555551c0 <+87>:	mov    BYTE PTR [rbp-0x21],0x53
0x00005555555551c4 <+91>:	mov    BYTE PTR [rbp-0x20],0x5f
0x00005555555551c8 <+95>:	mov    BYTE PTR [rbp-0x1f],0x45
0x00005555555551cc <+99>:	mov    BYTE PTR [rbp-0x1e],0x41
0x00005555555551d0 <+103>:	mov    BYTE PTR [rbp-0x1d],0x53
0x00005555555551d4 <+107>:	mov    BYTE PTR [rbp-0x1c],0x59
0x00005555555551d8 <+111>:	mov    BYTE PTR [rbp-0x1b],0x5f
0x00005555555551dc <+115>:	mov    BYTE PTR [rbp-0x1a],0x38
0x00005555555551e0 <+119>:	mov    BYTE PTR [rbp-0x19],0x39
0x00005555555551e4 <+123>:	mov    BYTE PTR [rbp-0x18],0x36
0x00005555555551e8 <+127>:	mov    BYTE PTR [rbp-0x17],0x30
0x00005555555551ec <+131>:	mov    BYTE PTR [rbp-0x16],0x46
0x00005555555551f0 <+135>:	mov    BYTE PTR [rbp-0x15],0x30
0x00005555555551f4 <+139>:	mov    BYTE PTR [rbp-0x14],0x41
0x00005555555551f8 <+143>:	mov    BYTE PTR [rbp-0x13],0x46
0x00005555555551fc <+147>:	mov    BYTE PTR [rbp-0x12],0x7d
0x0000555555555200 <+151>:	movzx  eax,BYTE PTR [rbp-0x30]
0x0000555555555204 <+155>:	movsx  eax,al
0x0000555555555207 <+158>:	mov    esi,eax
0x0000555555555209 <+160>:	lea    rdi,[rip+0xdf4]        # 0x555555556004
0x0000555555555210 <+167>:	mov    eax,0x0
0x0000555555555215 <+172>:	call   0x555555555070 <printf@plt>
0x000055555555521a <+177>:	nop
0x000055555555521b <+178>:	mov    rax,QWORD PTR [rbp-0x8]
0x000055555555521f <+182>:	xor    rax,QWORD PTR fs:0x28
0x0000555555555228 <+191>:	je     0x55555555522f <main+198>
0x000055555555522a <+193>:	call   0x555555555060 <__stack_chk_fail@plt>
0x000055555555522f <+198>:	leave
0x0000555555555230 <+199>:	ret
```

Temos muitas adições de bytes na memória, de main+27 (RBP-0x30) até main+147 (RBP-0x12).

Portanto, podemos adicionar um ponto de interrupção em algum lugar como main+155, logo após os valores terem sido adicionados à memória.

```shell
(gdb) break *main+155
(gdb) run
```

E, em seguida, ler a memória desde RBP-0x30.

Então, a questão é: como lemos e por que começamos em -0x30?

porque nesta posição negativa, estamos incrementando os valores pra ler, fazendo com que a leitura comece a partir do RBP-0x30, ela seguirá para -0x2f, depois -0x2e, etc...

E para ler a string, vamos alterar o tipo de byte (x) para string (s). Da mesma forma que fizemos da última vez, mas alterando isso, teremos

```shell
(gdb) x/1sb $rbp-0x30
```

Outra coisa é que normalmente lemos muitos bytes, mas neste caso fazemos `1sb`, pra ler 1 string. Isto é um pouco estranho em termos de comprimento, mas isso irá produzir cada «String» e não «Caractere» da memória.

Ao ler este valor da memória, recebemos diretamente a flag para colocar na resposta.

{{% details title="Resposta: Flag 5" open=false %}}
```
picoCTF{ASCII_IS_EASY_8960F0AF}
```
{{% /details %}}
