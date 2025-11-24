---
title: "Level 14"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 14

## Objetivo
> The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada em /etc/bandit_pass/bandit14 e só pode ser lida pelo usuário bandit14. Para este level, você não recebe a próxima senha, mas sim uma chave SSH privada que pode ser usada para acessar o próximo level. Analise os comandos que permitiram o acesso aos níveis anteriores do bandit e descubra como usar a chave para este level.

## Comandos que você pode precisar para resolver esse level
> ssh, scp, umask, chmod, cat, nc, install

## Passos

Normalmente, acessamos um level usando o comando `ssh` com uma solicitação de senha, mas desta vez vamos usar uma chave privada. Uma conexão segura usando chaves pública e privada está além do escopo deste guia, mas posso garantir que a chave privada não deve ser acessível a outros usuários. Essa é a sacada deste desafio: a chave privada é acessível e vamos usá-la para acessar o próximo level.

Na home, podemos encontrar o arquivo `sshkey.private`. Essa é uma chave privada RSA e podemos ler seu conteúdo com o comando `cat`, mostrando todo seu conteúdo criptografado. Vamos copiar essa chave privada e salvá-la em nossa máquina em um novo arquivo chamado `id_rsa`.

Agora, fora do usuário bandit, vamos nos conectar ao próximo level apenas com a chave RSA. O primeiro lugar que o `ssh` verifica em busca de chaves é no caminho `~/.ssh`, então vamos colocar nossa chave privada roubada lá.

Depois disso, basta conectar-se via `ssh` para o próximo level, sem necessidade de senha.

### WARNING: UNPROTECTED PRIVATE KEY FILE!

Se você vir esta mensagem, o problema é que a chave privada tem permissões em excesso. Por ser um arquivo privado, ter permissões demais representa um problema de segurança e a chave privada será ignorada. Para resolver isso, altere as permissões do arquivo da chave privada usando o comando `chmod`.

```shell
chmod 600 id_rsa
```

Onde:
- `chmod`: comando para alterar permissões
- `600`: tipo de permissão de arquivo, normalmente separada em Usuário, Grupo e Outros
	- `6` (Usuário): Ler e Escrever
	- `0` (Grupo): Sem permissão
	- `0` (Outros): Sem permissão
- `id_rsa`: arquivo da chave privada

Agora, dentro do próximo level, você tem acesso à senha em /etc/bandit_pass/bandit14.

