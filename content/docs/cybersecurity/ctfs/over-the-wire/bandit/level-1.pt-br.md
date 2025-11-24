---
title: "Level 01"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 01

Daqui pra frente, os desafios irão realmente começar. Normalmente eu uso a tag 
de spoiler para o resultado da flag, mas no caso do OverTheWire, a senha muda 
ocasionalmente, então eu só vou fazer um passo-a-passo do problema sem uma 
resposta final.

## Objetivo
> The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

Em uma tradução livre seria algo como:

> A senha para o próximo level está guardada num arquivo chamado readme 
localizado na pasta home. Use essa senha para logar no usuário bandit1 usando 
SSH. Quando você encontrar uma senha para um level, use o SSH (na porta 2220) 
para logar naquele level e continuar o jogo.

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find


## Passos

Esse level tem a intenção de ensinar os basicos de como usar um terminal unix. 
No meu caso eu estou usando um Linux numa Máquina Virtual.

No último level que estivemos (level 0) nós ganhamos acesso a um usuário 
chamado bandit0. Nós podemos ver isso na identificação do shell para o usuário 
que está atualmente logado:

```shell
bandit0@bandit:~$
```

Lendo o objetivo desse desafio, nossa senha está num arquivo chamado `readme`. 
Então primeiro vamos listar todos os arquivos da pasta atual usando o comando 
`ls`. E só nos mostrou um arquivo chamado `readme`. Esse é o arquivo que contém 
a senha pro próximo level.

Para ler o conteúdo do arquivo nós iremos usar um comando chamado `cat`, ele 
vai nos mostrar o conteúdo do arquivo.

Com isso, nós temos um texto com a senha para o próximo level.
