---
title: "Level 31"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 31

## Objetivo
> There is a git repository at ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo através da porta 2220. A senha para o usuário bandit30-git é a mesma do usuário bandit30.
> 
> Clone o repositório e encontre a senha para o próximo level.

## Comandos que você pode precisar para resolver esse level
> git

## Passos

Primeiramente, clone o repositório como nos níveis anteriores. Depois disso, podemos continuar a jornada para encontrar a senha do próximo level.

Desta vez, o `git log` e o `git show` mostram apenas um arquivo, sem alterações. Parece que chegamos a um beco sem saída. Talvez tenha um branch diferente, mas nenhum branch nos leva à senha. 

Na verdade, a resposta está no sistema de arquivos dentro da pasta oculta `.git`, que mostra alguns arquivos compactados. Então, vamos verificar as referências dos pacotes no repositório. Abra os arquivos `.git/packed-refs` com o comando `cat` para ver uma referência curiosa:

```
843...351 /refs/tags/secret
```

O primeiro hash provavelmente é o hash de um commit de uma tag chamada "secret". Portanto, usaremos um comando chamado `git cherry-pick` para obter o commit específico da árvore.

```
git cherry-pick 843...351
```

Então, o `git` nos retorna a mensagem: "Não é possível selecionar um blob com cherry-pick". Ok, isso significa que não estamos lidando com um commit, mas com um blob de algum tipo. Consultando o manual do `git`, encontramos um comando que resolve isso, chamado `unpack-file`, que diz:

```
git-unpack-file(1)
           Cria um arquivo temporário com o conteúdo de um blob.
```

Então, vamos executar o seguinte comando:

```shell 
git unpack-file 843...351
```

Depois disso, um arquivo chamado `.merge_file_uEpLe8` será criado na mesma pasta. Em seguida, use o comando `cat` para ler o arquivo descompactado e revelar a senha para o próximo level.


