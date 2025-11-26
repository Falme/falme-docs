---
title: "Level 28"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 28

## Objetivo 
> There is a git repository at ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo através da porta 2220. A senha para o usuário bandit27-git é a mesma do usuário bandit27.
> 
> Clone o repositório e encontre a senha para o próximo level.

## Comandos que você pode precisar para resolver esse level
> git

## Passos

A partir de agora, vamos brincar com o `git`. O Git é um programa de controle de versão que usa repositórios para rastrear as alterações em um projeto, criando uma linha do tempo das mudanças de forma eficaz e com ótimo desempenho.

Este é o básico para lidar com o `git`. Vamos clonar um repositório para uma pasta, o que significa fazer uma cópia do servidor para a nossa máquina. O comando será algo como:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Lembre-se de que estamos lidando com uma porta diferente (2220), a porta SSH comum é 22, então precisamos adicioná-la ao endereço SSH.

Após clonar o repositório, encontre o arquivo README, ele contém a senha para o próximo level.


