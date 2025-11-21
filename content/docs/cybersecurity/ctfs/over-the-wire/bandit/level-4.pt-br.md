---
title: "Level 4"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 4

## Objetivo
> The password for the next level is stored in a hidden file in the `inhere` directory.

Numa tradução livre, seria algo como:

> A senha para o próximo level está guardado em um arquivo escondido no diretório `inhere`.

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

## Passos

Esse desafio nos ensina sobre arquivos e diretórios escondidos (hidden). No 
Linux, arquivos escondidos são marcados com um ponto (`.`) antes do nome do 
arquivo. Isso faz com que ele seja marcado como escondido e não irá aparecer 
em um comando `ls` comum.

Então se nós usarmos `ls` no diretório home, nós podemos encontrar o diretório 
`inhere`. Então nós entramos no `inhere` usando o comando `cd`:

```shell
cd inhere
```

Agora, dentro do diretório `inhere`, se listarmos os arquivos usando `ls`, 
nenhum arquivos vai ser encontrado. Para mostrar arquivos escondidos em um 
diretório nós vamos usar a opção `-a` para mostrar todos os arquivos, 
incluindo os arquivos escondidos. Será algo como:

```shell
ls -a
```

Com isso, o arquivo `...Hiding-From-You` é revelado, lemos o arquivo com `cat`: 

```shell
cat < ...Hiding-From-You
```

E a senha é revelada para o próximo level.
