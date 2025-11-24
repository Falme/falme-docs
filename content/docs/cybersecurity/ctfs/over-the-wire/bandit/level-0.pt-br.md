---
title: "Level 00"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 00

Olá, bem vindes a minha jornada no CTF do Over The Wire, um dos meus CTFs 
favoritos. 

Eu já completei ele anos atrás, mas retornei para revisitar e dessa 
vez documentar como que eu resolvo esses desafios.

Lembrando que essa documentação é para mim, e que se você não resolveu esse ctf 
por conta própria, não leia essas postagens, vá e resolva os desafios por conta 
própria, não estrague essa experiência.

## Level 00

> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

Traduzindo de uma forma livre, ficaria algo como:

> O objetivo desse level é que você entre no sistema do jogo usando SSH. O host que você precisa conectar é o bandit.labs.overthewire.org, na porta 2220. O usuário é bandit0 e a senha é bandit0. Uma vez logado, vá para a página Level 1 para descobrir como vencer o Level 1.

Esse level é a introdução ao sistema que iremos nos aventurar: Bandit.

Então a solução está no problema, ele te diz qual o login e senha para entrar 
no jogo. O único problema aqui é como usar o comando SSH.

Nesse caso ficaria algo como:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```

Onde:
- `ssh` : É a chamada do comando SSH
- `bandit.labs.overthewire.org` : é o host a se conectar
- `-p 2220` : é a porta a se conectar, no caso do bandit é sempre 2220
- `-l bandit0` : é o nome de usuário que está conectando, nesse caso, bandit0.

Sendo assim, terminamos o level0. Agora podemos ir para o Level1.
