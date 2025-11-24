---
title: "Level 08"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 08

## Objetivo
> The password for the next level is stored in the file data.txt next to the word millionth

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, ao lado da palavra millionth.

## Comandos que você pode precisar para resolver esse level
> man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Passos

Este desafio tem como objetivo nos ensinar o comando `grep`. O comando `grep` mostra linhas que correspondem a um padrão, neste exemplo, uma string.

Imagine que você está procurando em um arquivo grande e precisa de uma palavra específica; o comando grep ignorará as outras linhas e mostrará apenas aquela que você precisa.

Por que isso é necessário, você pergunta? Em primeiro lugar, é prático. Em segundo lugar, você pode automatizar algumas buscas a partir da linha de comando, mas em terceiro lugar, e o mais importante para mim, é para encontrar padrões em arquivos enormes.

Alguns arquivos, como listas e dicionários, são apenas arquivos de texto (.txt) com gigabytes de tamanho. Se você tentar abri-los com o Bloco de Notas, não vai funcionar. Então, para brincar com ele, vamos voltar ao Objetivo.

Queremos encontrar a palavra "millionth", então, usando o comando `grep`, ficaria algo como:

```shell
cat data.txt | grep 'millionth'
```

Onde:
- `cat`: abre o arquivo
- `data.txt`: o arquivo para ler os dados
- `|` : caractere especial de pipe, que envia a saída de um comando para a entrada de outro comando.
- `grep`: comando para filtrar a saída de texto de `cat data.txt`
- "millionth": a string que queremos filtrar

Com isso, a senha para o próximo level aparecerá ao lado da palavra "millionth".

