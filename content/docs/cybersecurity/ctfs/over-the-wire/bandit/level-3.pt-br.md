---
title: "Level 3"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 3

## Objetivo
> The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory

Em uma tradução livre, ficaria algo como:

> A senha para o próximo level está guardada num arquivo chamado `--spaces in this filename--` localizado no diretório home

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

## Passos

Esse aqui é parecido com o level anterior (level 2), mas nesse caso o caractere 
especial é o espaço. Normalmente usar o espaço depois de chamar um programa é 
entendido como separador de argumentos. Então, vamos ver um exemplo usando `cat`:

```shell
cat file.txt
```

Então nesse caso nós temos o programa `cat` e um argumento esperado, separado 
por um espaço, um arquivo com o nome `file.txt`.

Lendo um arquivo que tem espaço no nome vai causar confusão no console, ele não 
consegue separar o que é espaço e o que é um argumento. Pra fazer isso 
funcionar, supreendentemente, temos duas opções pra ler o arquivo.

### Aspas

Nós podemos usar áspas ou áspas duplas em volta do caminho do arquivo, sendo 
reconhecido como espaço entre as aspas e argumentos fora das aspas. Seria algo 
como:

```shell
cat < "--spaces in this filename--"
```

(O `<` é necessário, pois `-` também é um caractere especial)

### Barra Invertida

A Barra Invertida também é um caractere especial, mas é usado para "Escapar um 
caractere", fazendo um espaço ser lido literalmente como um espaço e não um 
separador. Seria algo como:

```shell
cat < --spaces\ in\ this\ filename--
```

Então cada espaço do caminho do arquivo deve ser substituído por Barra 
Invertida+Espaço. Dessa maneira a senha para o próximo level é revelada.
