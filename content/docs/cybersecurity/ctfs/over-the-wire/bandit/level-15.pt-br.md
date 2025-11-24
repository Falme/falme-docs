---
title: "Level 15"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 15

## Objetivo
> The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

Em uma tradução livre, seria algo como:

> A senha para o próximo level pode ser obtida enviando a senha do level atual para a porta 30000 no localhost.

## Comandos que você pode precisar para resolver esse level
> ssh, telnet, nc, openssl, s_client, nmap

## Passos

Entendendo o texto do objetivo, a porta 30000 está em escuta (listening), aguardando a senha do level atual. Para estabelecer uma conexão TCP/UDP através de uma porta em escuta, utilizaremos o comando `nc`. O comando `nc` (ou netcat) foi criado para esse tipo de tarefa, portanto, vamos estabelecer a conexão com:

```shell
nc localhost 30000
```

Nenhuma resposta será exibida no console, mas ele está aguardando a entrada. Basta digitar a senha do level atual, pressionar Enter e a senha para o próximo level será exibida.

