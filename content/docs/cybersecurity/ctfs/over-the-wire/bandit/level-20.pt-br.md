---
title: "Level 20"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 20

## Objetivo 
> To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

Em uma tradução livre, seria algo como:

> Para acessar o próximo level, você deve usar o binário setuid no diretório home. Execute-o sem argumentos para descobrir como usá-lo. A senha para este level pode ser encontrada no local usual (/etc/bandit_pass), após você ter usado o binário setuid.

## Comandos que você pode precisar para resolver esse level
> 

## Passos

Existe um arquivo executável na pasta inicial chamado `bandit20-do`. Para saber mais sobre esse executável, vamos executá-lo sem argumentos:

```shell 
./bandit20-do
```

Isso nos retorna o seguinte texto:

```
Run a command as another user. (Executar um comando como outro usuário.)
	Example: ./bandit20-do whoami
```

O comando `whoami` mostra o nome do usuário atualmente conectado. Se usarmos apenas o comando `whoami`, ele retornará "bandit19", o usuário ao qual nos conectamos via `ssh`. Mas se executarmos `./bandit20-do whoami`, ele retornará "bandit20".

Portanto, o que concluímos disso é:

- O executável bandit20-do pode receber outros comandos
- bandit20-do age como o usuário bandit20

Com essa informação, usaremos o programa `bandit20-do` para ler a senha do bandit20 em /etc/bandit_pass/bandit20:

```shell
./bandit20-do cat /etc/bandit_pass/bandit20
```

Com isso, temos a senha para o próximo level.


