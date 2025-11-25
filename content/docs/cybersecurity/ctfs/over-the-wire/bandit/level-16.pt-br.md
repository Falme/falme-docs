---
title: "Level 16"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 16

## Objetivo
> The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

Em uma tradução livre, seria algo como:

> A senha para o próximo level pode ser obtida enviando a senha do level atual para a porta 30001 no localhost usando criptografia SSL/TLS.
> 
> Dica: Está recebendo os comandos “DONE”, “RENEGOTIATING” ou “KEYUPDATE”? Leia a seção “CONNECTED COMMANDS” do manual.

## Comandos que você pode precisar para resolver esse level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Passos

Este desafio é muito semelhante ao anterior (level 15), mas desta vez precisamos estabelecer uma conexão segura usando criptografia SSL/TLS. Para isso, precisamos de um Netcat mais poderoso, então usaremos o comando `ncat` com a opção `--ssl`:

```
--ssl             Conecte-se ou escute com SSL
```

Estabeleceremos uma conexão segura com o localhost na porta 30001, da seguinte forma:

```shell
ncat --ssl localhost 30001
```

Após estabelecer a conexão, inserimos a senha do level atual e pressionamos Enter. Com isso, a senha para o próximo level é revelada.


