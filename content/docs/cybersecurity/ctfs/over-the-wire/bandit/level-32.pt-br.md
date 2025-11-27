---
title: "Level 32"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 32

## Objetivo
> There is a git repository at ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo através da porta 2220. A senha para o usuário bandit31-git é a mesma do usuário bandit31.
> 
> Clone o repositório e encontre a senha para o próximo level.

## Comandos que você pode precisar para resolver esse level
> git

## Passos

Primeiramente, clone o repositório como nos níveis anteriores. Depois disso, podemos continuar a jornada para encontrar a senha do próximo level.

Neste desafio, vamos aprender sobre o arquivo `.gitignore`, esse arquivo define o que NÃO deve ser rastreado no projeto. Pode ser um arquivo binário, um arquivo que você não quer enviar para o repositório, etc.

Após clonar o repositório, você pode encontrar o arquivo `.gitignore` e lê-lo com o comando `cat`. Ele mostrará que todos os arquivos "*.txt" estão sendo ignorados, portanto, nenhum arquivo .txt será rastreado.

Depois, há o arquivo README.md, cujo texto diz:

```
This time your task is to push a file to the remote repository.
(Desta vez, sua tarefa é enviar um arquivo para o repositório remoto.)

Details:
	File name: key.txt
	Content: 'May I come in?'
	Branch: master
```

Portanto, precisamos criar um arquivo chamado `key.txt` com o texto "May I come in?" no branch master. Porém, o arquivo `.gitignore` não rastreia arquivos .txt, então não podemos enviar esse arquivo.

Então, vamos excluir o arquivo .gitignore e confirmar nossas alterações. Sem o .gitignore, nada nos impede de enviar o arquivo .txt.

Então, para enviar uma alteração no `git`, geralmente seguimos esses passos:

- `git add ./file`: Adicionamos o arquivo ao stage, preparando-o para o commit.
- `git commit -m "mensagem"`: Confirmamos nossas alterações e arquivos, geralmente também escrevemos uma mensagem para saber o que mudou.
- `git push --all`: Em seguida, enviamos nossas alterações e arquivos para o servidor onde o repositório está localizado.

Com o novo arquivo, ficará mais ou menos assim:

```shell
git add ./key.txt
git commit -m "May I come in?"
git push --all
```

Se você fez tudo certo, o `git` retornará um erro com a senha para o próximo level. 
