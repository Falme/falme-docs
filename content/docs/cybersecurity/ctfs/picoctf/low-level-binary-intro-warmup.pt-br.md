---
title: "Low Level Binary Intro: Warmup"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoCTF: Low Level Binary Intro - Warmup

Estou começando a enfrentar os desafios do PicoCTF, um site para resolver problemas de Capture the Flag (CTFs), aprender os fundamentos de cibersegurança e talvez torná-los úteis para desenvolvimento de jogos, de sites e pra vida em geral.

Os primeiros são bem básicos, mas a ideia é criar uma rotina de aprendizagem passo a passo e, para isso, vou começar pela página 'Playlists'.

![Image description](/images/low-level-binary-intro-warmup/1.jpg)

Como diz na página: "Playlists são coleções de desafios, por vezes com textos ou jogos, que são escolhidos para ajudar os alunos a aprender um tópico específico."

Então, vou começar com <a href='https://play.picoctf.org/playlists/2' target='_blank'>"Low Level Binary Intro"</a>, que pode ser útil para o meu trabalho atual (Desenvolvimento de Jogos).

A primeira parte de Low Level Binary Intro chama-se "Warmup", na qual vamos abordar neste documento.

## Flag 1: Obedient Cat

![Image description](/images/low-level-binary-intro-warmup/2.jpg)


Esse aqui é apenas um teste de sanidade para os outros desafios.

O teste de sanidade é um teste simples para verificar se tudo está acontecendo como esperado no nível básico. Neste caso, só preciso baixar a flag em um arquivo e ler o conteúdo dentro dele.

{{% details title="Resposta: Flag 1" open=false %}}
```
picoCTF{s4n1ty_v3r1f13d_b5aeb3dd}
```
{{% /details %}}

---

## Flag 2: Warmed Up

Após a verificação de sanidade, chegamos ao primeiro desafio deste post, chamado "Warmed Up". 

![Image description](/images/low-level-binary-intro-warmup/3.jpg)

Este desafio é bem fácil, basta procurar no DuckDuckGo como "0x3D in decimal" para termos a resposta. Mas sei que vou precisar usar python nesta playlist. Então eu vou além e criarei um conversor de hexadecimal para decimal em python.

Atualmente, tenho a versão `Python 3.10.8`. Então, vamos começar.

A ideia é criar uma string hexadecimal e multiplicar cada caractere por 16.

Por exemplo: 0xA9D = A\*16\*16 + 9\*16 + D
Isso é o mesmo que: 10\*(16^2) + 9\*(16^1) + 13\*(16^0) = 2717

Então, primeiro, vou criar um método para converter o caractere hexadecimal em um número decimal:

```python
#Lista/Dicionario de números acima de 9
#Pra ser chamado em um loop
associationList = [
            ['A', 10],
            ['B', 11],
            ['C', 12],
            ['D', 13],
            ['E', 14],
            ['F', 15],
        ]

def ToDecimal(hexChar):
    
    #Primeiro tenta achar o valor na lista acima
    #Se não encontrar no dicionário, retorna o mesmo valor
    for item in associationList:
        if item[0] == hexChar.upper():
            return item[1]

    #Tenta retornar um número válido, se não, retorna -1
    try:
        return int(hexChar)
    except:
        return 0
```

O método ToDecimal(char) converterá o valor ‘F’ para 15, mas também converterá o meu valor ‘6’ (como uma string) para 6 (como um inteiro).

Agora preciso criar um método que pegue o valor hexadecimal inserido pelo usuário, converter em valores decimais e somá-los.

```python
import sys

def HexToDec():
    
    #Soma total depois dos calculos
    total = 0
 
    #Tenta parsear o input do usuário, senão, mostra um erro
    try:
        #Passa por todos os caracteres de trás pra frente
        for x,char in enumerate(reversed(sys.argv[1])):
            
            #Multiplica a respectiva posição do hexadecimal
            #Para o valor decimal
            multiplier = ToDecimal(char) * pow(16,x)
            
            #Soma/Adiciona ao resultado final
            total += multiplier

        print(total)
    except:
        print("Not valid arguments or missing argument")

HexToDec()
```

Com este código, posso passar o valor hexadecimal através dos parâmetros chamando algo como: `python HexToDec.py 2e87` e ele irá retornar 11911.

Mas depois de fazer muitas coisas que não resolvem o problema original, vamos voltar e resolvê-lo:

Preciso saber o valor de 0x3D em decimal e, colocando no código python, ele retorna...

` > 61 `

Então eu só preciso adicionar o 61 no formato da Flag e está resolvido:

![Image description](/images/low-level-binary-intro-warmup/4.jpg)

E é isso, uma solução super complicada para um problema simples.

Aqui está o [Script em Python](https://github.com/Falme-SideProjects/python-hex-to-dec/blob/main/HexToDec.py)

{{% details title="Resposta: Flag 2" open=false %}}
```
picoCTF{61}
```
{{% /details %}}

---

## Flag 3: ASCII Numbers

Pra esse desafio, vou precisar coletar uma coleção de hexadecimais e converter em letras ASCII pra revelar a flag. Eu poderia converter na mão, mas eu quero fazer um script em python pra isso.

Primeiro, nós precisamos recortar os valores hexadecimal do tipo "0x30 0x40" pra ser uma lista de array só com os valores hexadecimal simples como [30, 40]. Aqui está o código demonstrando:

```python

import sys

# Pega um texto como "0x30 0x40 0x50" e converte em array
def TrimHexString(originalString):

    #Quebra os valores em um array
    hexValues = originalString.split()

    # Checa se precisa remover o 0x do texto hexadecimal
    for index,val in enumerate(hexValues):
        # Se tiver um 0x, remover
        if val[:2] == "0x":
            hexValues[index] = val[2:]

    #Retorna o valor final da lista em array
    return hexValues

TrimHexString("0x10 0x20 0x30")

```

E, em seguida, pegar essa lista e concatená-la em uma só string, por exemplo converter de [30,40,50] para “304050”. Tornando possível chamar bytes.fromhex().decode(“utf-8”) para que possamos converter o hexadecimal para ASCII facilmente:

```python

# Junta todos os hexadecimais do array em uma só string
def ConcatenateHexList(hexList):
    return "".join(hexList)

```

E então chamar tudo no primeiro argumento da função pela linha de comando vai mostrar o resultado da conversão usando decode("utf-8"):

```python

# Pega o primeiro valor da linha de comando pra recortar e juntar os valores
hexConcatenated = ConcatenateHexList(TrimHexString(sys.argv[1]))

# Decodifica o valor hexadecimal e mostra os resultados em ASCII
result = bytes.fromhex(hexConcatenated).decode("utf-8")
print(result)

```

E ao chamar o comando:

```bash
python3 HexToASCII.py "0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d"
```

Nós teremos a flag:

{{% details title="Resposta: Flag 3" open=false %}}
```
picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
```
{{% /details %}}

---

## Flag 4: Picker I

Dessa vez temos um serviço que gera números aleatórios, esse tem uma chamada NetCat para os servidores do PicoCTF, que também providencia pra gente o código-fonte (script em python)

Acessando o netcat, ele me providencia a possibilidade de chamar o getRandomNumber() pra retornar um número aleatório, mas sempre retorna 4. A função é realmente aleatória? [Nós nunca saberemos.](https://xkcd.com/221/)

![Image description](/images/low-level-binary-intro-warmup/5.jpg)

Ao examinar o código-fonte, podemos ver que a função getRandomNumber() é inútil, mas que também existe um método chamado "win()" que chama um arquivo chamado "flag.txt". Espera, nós queremos isso.

![Image description](/images/low-level-binary-intro-warmup/6.png)

Chamando essa função ao invés do getRandomNumber, nós recebemos um monte de valores hexadecimal:

![Image description](/images/low-level-binary-intro-warmup/7.jpg)

Sorte nossa, temos um script especificamente para isso, então chamando o comando:

```bash
python3 HexToASCII.py "0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x36 0x65 0x30 0x34 0x34 0x34 0x30 0x64 0x7d"
```

Temos um resultado, e a resposta é a nossa flag:

{{% details title="Resposta: Flag 4" open=false %}}
```
picoCTF{4_d14m0nd_1n_7h3_r0ugh_6e04440d}
```
{{% /details %}}
