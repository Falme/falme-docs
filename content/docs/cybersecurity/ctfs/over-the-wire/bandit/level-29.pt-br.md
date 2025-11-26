---
title: "Level 29"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 29

## Objetivo 
> There is a git repository at ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo através da porta 2220. A senha para o usuário bandit28-git é a mesma do usuário bandit28.
> 
> Clone o repositório e encontre a senha para o próximo level.

## Comandos que você pode precisar para resolver esse level
> git

## Passos

Agora, faça o mesmo que no level anterior (level 28) para clonar o repositório em uma pasta, mas agora para o bandit28-git.

Depois disso, podemos verificar se o arquivo `README` não contém a senha para o próximo level. Lembra que eu disse que o `git` mantém um histórico das alterações no projeto? Bem, chegou a hora de verificar o que foi alterado. Vamos executar o comando:

```bash
git show
```

Este comando mostrará quais foram as alterações no arquivo `README` e também revelará a senha para o próximo level.
