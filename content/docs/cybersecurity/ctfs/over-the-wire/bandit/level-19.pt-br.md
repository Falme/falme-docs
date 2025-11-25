---
title: "Level 19"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 19

## Objetivo
> The password for the next level is stored in a file readme in the home directory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada em um arquivo readme no diretório home. Infelizmente, alguém modificou o arquivo .bashrc para te desconectar quando você faz login via SSH.

## Comandos que você pode precisar para resolver esse level
> ssh, ls, cat

## Passos

Este desafio te desconecta logo após a conexão com o usuário bandit18. O arquivo .bashrc inicializa um script que mostra o texto "Byebye!" e desconecta você. 

A solução para isso é injetar um comando via SSH antes que você seja desconectado. Felizmente, o comando `ssh` aceita um comando injetado dentro da sequência de conexão. É algo como:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit18 "cat ~/readme"
```

O comando `"cat ~/readme"` é um comando comum do console e será executado logo após o estabelecimento da conexão. Ele serve para ler o conteúdo do arquivo `readme`.

Com isso, a senha para o próximo level é revelada.
