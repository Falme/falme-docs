---
title: "Level 30"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 30

## Objetivo 
> There is a git repository at ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo através da porta 2220. A senha para o usuário bandit29-git é a mesma do usuário bandit29.
> 
> Clone o repositório e encontre a senha para o próximo level.

## Comandos que você pode precisar para resolver esse level
> git

## Passos

Clone o repositório da mesma forma que no level anterior e vamos encontrar a senha para este desafio.

Desta vez, o desafio nos ensina sobre branches. No `git`, branches são diferentes linhas do tempo de um projeto. Digamos que você queira testar uma funcionalidade, então, você cria uma nova branch para testá-la de forma não destrutiva. Primeiro, vamos verificar quantas branches estão disponíveis. Podemos fazer isso com:

```shell 
git branch --all
```

Isso mostrará todas as branches disponíveis no projeto. Vamos verificar a branch de desenvolvimento chamada `origin/dev`. Para mudar para essa branch, usaremos o comando `git checkout`:

```shell 
git checkout origin/dev 
```

Agora, mudamos da branch master para origin/dev, e ao abrir o arquivo README.md com o comando `cat`, a senha para o próximo level é exibida.
