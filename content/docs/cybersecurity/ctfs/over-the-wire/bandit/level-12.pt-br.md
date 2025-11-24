---
title: "Level 12"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 12

## Objetivo
> The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, onde todas as letras minúsculas (a-z) e maiúsculas (A-Z) foram rotacionadas em 13 posições.

## Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Passos

Este desafio é semelhante ao anterior (level 11), mas desta vez não estamos lidando com a criptografia base64, e sim com a criptografia ROT13. 

A criptografia ROT13 é bastante simples: ela desloca um caractere 13 letras da posição inicial, como em uma rotação. Se o caractere for 'A', ele será traduzido para 'N'; se for 'B', será traduzido para 'O', e assim por diante. Para uma demonstração, veja a imagem abaixo:

![Image description](/images/picogym-mod-26/1.jpg)

Para o nosso desafio, usaremos um comando chamado `tr`, que traduz caracteres de um caractere para outro. Com alguns exemplos, ficará muito fácil de entender. Vamos traduzir um caractere da palavra `test`:

```shell
echo "test" | tr "s" "N"
```

No exemplo acima, estamos usando `tr` para converter o caractere 's' em 'N'. O resultado será `teNt`, com N em maiúsculo, exatamente como escrevemos. Mas não é apenas um único caractere que pode ser traduzido, mas também uma sequência de caracteres:

```shell
echo "test" | tr "r-u" "N"
```

No caso acima, estamos traduzindo os caracteres de 'r' a 'u' (inclusivo) para o caractere 'N'. O resultado será `NeNN` porque 't' e 's' estão entre 'r' e 'u' no alfabeto.

Você deve estar se perguntando se ambos possuem um argumento de intervalo. Vamos analisar:

```shell
echo "test" | tr "r-u" "N-Q"
```

No caso acima, estamos traduzindo os caracteres [r,s,t,u] para os caracteres [N,O,P,Q]. O resultado será `PeOP`, porque tentarão encontrar a correspondência entre suas posições em ambos os arrays para obter o resultado. O 't' está na mesma posição que 'P' e 's' está na mesma posição que 'O'.

Agora podemos fazer algumas alterações para concluir nosso desafio, executando o seguinte comando:

```shell
cat data.txt | tr "N-ZA-Mn-za-m" "A-Za-z"
```

Where:
- `cat data.txt`: irá ler o arquivo ROT13 codificado
- `|` : irá direcionar o resultado de `data.txt` para o comando `tr`
- `tr`: irá traduzir os caracteres codificados para caracteres decodificados.
- `"N-ZA-Mn-za-m"` : são os caracteres originais que queremos alterar.
	- O array de caracteres terá uma aparência semelhante a esta:
	- [N, O, P, Q, ..., Y, Z, A, B, C, ..., L, M, n, o, p, q, ..., y, z, a, b, c, ..., l, m]
	- Em outras palavras, um alfabeto deslocado em maiúsculas e minúsculas.
- `"A-Za-z"`: é a transformação dos caracteres, então ficará mais ou menos assim:
	- [A, B, C, ..., Y, Z, a, b, c, ..., y, z]

O primeiro array será substituído pelo segundo array, realizando uma conversão ROT13 bem sucedida. Agora você tem a senha para o próximo level.

