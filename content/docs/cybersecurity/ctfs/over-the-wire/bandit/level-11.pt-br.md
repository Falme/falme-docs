---
title: "Level 11"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 11

## Objetivo
> The password for the next level is stored in the file data.txt, which contains base64 encoded data

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, que contém dados codificados em base64.

## Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Passos

Este exercício é o primeiro que aborda (e nos ensina) sobre criptografia. Não que precisamos entender criptografia, mas precisamos descriptografar um arquivo. O arquivo `data.txt` está criptografado em base64, e nós possuímos o comando `base64`. No manual do `base64`, temos a opção `-d`:

```
-d, --decode
              decodificar dados
```

Então, usamos essa opção para decodificar o arquivo `data.txt`, ficaria mais ou menos assim:

```shell
base64 -d data.txt
```

E com isso, a senha para o próximo level é revelada.


