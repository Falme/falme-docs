---
title: "Level 17"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 17

## Objetivo 
> The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

Em uma tradução livre, seria algo como:

> As credenciais para o próximo level podem ser obtidas enviando a senha do level atual para uma porta no localhost no intervalo de 31000 a 32000. Primeiro, descubra quais dessas portas têm um servidor escutando. Em seguida, descubra quais deles usam SSL/TLS e quais não usam. Há apenas um servidor que fornecerá as próximas credenciais; os outros simplesmente retornarão o que você enviar para eles.
> 
> Nota útil: Está recebendo os comandos “DONE”, “RENEGOTIATING” ou “KEYUPDATE”? Leia a seção “CONNECTED COMMANDS” na página do manual.

## Comandos que você pode precisar para resolver esse level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Passos

Neste desafio, a forma de resolvê-lo é a mesma do level anterior (level 16), mas a porta à qual precisamos nos conectar é desconhecida em um intervalo de 1000 portas (de 31000 a 32000). Precisamos encontrar a porta correta e nos conectar a ela usando o comando `ncat --ssl`.

Para encontrar portas abertas em um intervalo, usaremos uma ferramenta chamada `nmap`. Essa ferramenta é um scanner de várias coisas, incluindo portas. Usaremos o `nmap` para escanear essas 1000 portas e verificar quais estão abertas e quais estão fechadas. O comando será algo como:

```shell
nmap localhost -p 31000-32000
```

Onde:
- `nmap`: é a ferramenta de varredura
- `localhost`: é o endereço do servidor
- `-p`: é a opção para procurar em portas específicas.
- `31000-32000`: é o intervalo de portas a serem verificadas, o hífen significa "entre"

Com isso, temos o resultado de 5 portas abertas, mas sem nenhuma informação. Sabemos que a porta correta aceita apenas conexões SSL, então usaremos o mesmo comando que usamos antes com o `ncat`:

```shell
ncat --ssl localhost {port}
```

Normalmente, uma porta incorreta resultará em um erro, e a porta correta não retornará nada. Basta inserir a senha do level atual e o sistema retornará as credenciais RSA do próximo level.

Salve as credenciais em um novo arquivo e use o comando `ssh` para se conectar sem precisar de uma senha:

```shell
ssh -i ./key-saved.private bandit.labs.overthewire.org -p 2220 -l bandit17
```

Se tiver alguma dúvida, confira o desafio do level 14.
