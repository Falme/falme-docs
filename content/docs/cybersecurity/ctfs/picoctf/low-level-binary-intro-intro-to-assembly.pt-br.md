---
title: "Low Level Binary Intro: Intro to Assembly"
weight: 4
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Intro to Assembly

Essa sessão da [Playlist do PicoCTF](https://play.picoctf.org/playlists/2) se chama "Intro to Assembly", os desafios vão se aprofundar nos comandos assembly, na qual eu já brinquei algumas vezes em códigos de baixo nível usando [IDA](https://hex-rays.com/ida-free/#download) e [Microcorruption](https://microcorruption.com/), mas mesmo assim, talvez seja divertido revisitar alguns conceitos basicos.

---

## Bit-O-Asm-1

O primeiro desafio é bem simples, nós temos um dump file em assembly, e precisamos saber qual é o valor final de EAX.

Então, o que é um assembly dump?

Um arquivo executável pode ser lido em um código de baixo nível, fazendo com o que o código de máquina (machine code) possa ser interpretado em assembly. Isso faz com que seja possível nós humanos possamos ler e entender sua lógica (human-readable code), que é muito usado em engenharia reversa, pegando esses comandos e transformando eles em C++ por exemplo, para que o usuário possa entender e re-compilar o código original.

Com isso em mente, o computador também tem alguns registros, que serve para fazer calculos computacionais, um desses registros de chama EAX, e nós precisamos do valor guardado nele.

Aqui está o dump file:

```python
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
```

Os valores e registros em assembly funciona que nem uma variavel numa linguagem de programação, então na linha `<+15>` nós temos 

`MOV` (move um valor para) 
`EAX, 0x30` (de `0x30` para `EAX`, sendo assim `EAX = 0x30`)

Mas na verdade nós precisamos do valor em decimal, então nós colocaremos o valor em nosso script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py), dessa forma:

```python
python3 HexToDec.py 0x30
```

E pegamos o resultado, colocando no formato `picoCTF{value}` e então teremos nossa flag:


{{% details title="Resposta: Flag 1" open=false %}}
```
picoCTF{48}
```
{{% /details %}}

---

## Bit-O-Asm-2

Esse tem a mesma solução e os passos que o problema anterior, na verdade, todos os desafios desse documento fazem os mesmo passos, só que com algo a mais.

Dessa vez ele vai ensinar que valores podem ser apontados para outros endereços, nesse caso nós queremos o valor em `EAX`, mas o valor final está na linha acima, isso é porque assembly pode ter endereços relativos. O endereço é `RBP`(registro) `-0x4`(menos 4 a partir do endereço do registro).

(só foque nas linhas `<+15>` e `<+22>`)

```python
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
<+25>:    pop    rbp
<+26>:    ret
```

A solução é o valor 0x9fe1a em decimal, então é só colocar esse valor no nosso script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py) e a resposta será revelada:

```python
python3 HexToDec.py 0x9fe1a
```


{{% details title="Resposta: Flag 2" open=false %}}
```
picoCTF{654874}
```
{{% /details %}}

---

## Bit-O-Asm-3

Esse desafio nos ensina operações matemáticas, como multiplicação e adição em assembly.

```python
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0xc],0x9fe1a
<+22>:    mov    DWORD PTR [rbp-0x8],0x4
<+29>:    mov    eax,DWORD PTR [rbp-0xc]
<+32>:    imul   eax,DWORD PTR [rbp-0x8]
<+36>:    add    eax,0x1f5
<+41>:    mov    DWORD PTR [rbp-0x4],eax
<+44>:    mov    eax,DWORD PTR [rbp-0x4]
<+47>:    pop    rbp
<+48>:    ret
```

Nesse caso, nós precisamos acompanhar onde `EAX` está/estava, e assim ter o seu resultado final (porque ele vai se modificar no decorrer do dump file)

Para multiplicação e adição dos valores hexadecimal, nós utilizaremos python no terminal e fazer os calculos lá. Só escrever `python` no terminal/cmd e estamos prontos:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9pbt8uciy3j0uzk1ipkg.png)

1. Em `<+29>` nós temos um valor sendo definido à `EAX`, um valor no endereço `RBP-0xC`
2. Em `<+15>` o valor de `RBP-0xC` é definido para `0x9fe1a`, sendo assim `EAX=0x9fe1a`
3. Em `<+32>` EAX é multiplicado pelo valor do endereço `RBP-0x8`
4. Em `<+22>` o valor em `RBP-0x8` é definido para `0x4`, sendo assim `EAX=0x9fe1a * 0x4 = 0x27F868`
5. Em `<+36>` nós temos o valor `0x1f5` adicionado à `EAX`, sendo assim `EAX=0x27F868 + 0x1f5 = 0x27FA5D`

A solução é o valor 0x27FA5D em decimal, então, colocamos o valor no nosso script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py) pra termos a resposta:

```python
python3 HexToDec.py 0x27FA5D
```

{{% details title="Resposta: Flag 3" open=false %}}
```
picoCTF{2619997}
```
{{% /details %}}

---

## Bit-O-Asm-4

Esse desafio nos ensina sobre comparações e código divergente (também conhecido como if, elseif, else) e jumps.

```python
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710
<+29>:    jle    0x55555555514e <main+37>
<+31>:    sub    DWORD PTR [rbp-0x4],0x65
<+35>:    jmp    0x555555555152 <main+41>
<+37>:    add    DWORD PTR [rbp-0x4],0x65
<+41>:    mov    eax,DWORD PTR [rbp-0x4]
<+44>:    pop    rbp
<+45>:    ret
```

- Na linha `<+22>` se compara(CMP) os valores no endereço `RBP-0x4` e o valor `0x2710` e o resultado fica guardado.
- Na linha `<+29>` checa o valor guardado na operação de comparação(CMP) e se o resultado for menor ou igual ao último valor [outro jeito de se ver seria `(RBP-0x4) <= 0x2710`]. Se for verdadeiro (se `RBP-0x4` é menor ou igual à `0x2710`) pula para a linha `<+37>`, se não, então continua para a próxima linha `<+31>`

Isso é meio difícil de explicar, mas o J em `jmp` ou `jle` significa jump (pulo), que é parecido com um GOTO (ir para) outra linha. E o `cmp` é uma comparação, algo como um comando IF.

Indo passo a passo:

1. Linha `<+22>` compara o endereço `RBP-0x4` e o valor `0x2710`
2. Linha `<+15>` define o endereço `RBP-0x4` para o valor`0x9fe1a`, fazendo a comparação de `0x2710` e `0x9fe1a`
3. Linha `<+29>` compara se `0x9fe1a` é menor ou igual à `0x2710`, não é, a condição é false, então nenhum pulo é feito
4. Linha `<+31>` subtrai (`SUB`) o valor no endereço `RBP-0x4` por `0x65`, sendo assim `RBP-0x4=0x9fe1a-0x65 = 0x9FDB5`
5. Linha `<+35>` pula (`jmp`) para a linha `<+41>`
6. Linha `<+41>` define `EAX` pra ser o valor do endereço `RBP-0x4`. Então `EAX=0x9FDB5`

A solução é o valor 0x9FDB5 em decimal, então nós colocamos esse valor no nosso script [HexToDec.py](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py) pra revelar a resposta:

```python
python3 HexToDec.py 0x9FDB5
```

{{% details title="Resposta: Flag 4" open=false %}}
```
picoCTF{654773}
```
{{% /details %}}
