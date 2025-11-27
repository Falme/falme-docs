---
title: "Level 33"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 33

## Objetivo
> After all this git stuff, it’s time for another escape. Good luck!

Em uma tradução livre, seria algo como:

> Depois de toda essa coisa de Git, é hora de outra escapada. Boa sorte!

## Comandos que você pode precisar para resolver esse level
> sh, man

## Passos

Este é o último desafio disponível no momento. Trata-se de mais uma escapada do shell para obter a senha dentro do diretório /etc/bandit_pass/.

O shell em que você está preso converte todos os comandos para letras maiúsculas, o que é um problema para o Linux, pois os comandos diferenciam maiúsculas de minúsculas, e você não consegue digitar nenhum comando sem receber um erro. 

A solução que encontrei foi escrever `$0`, uma das variáveis especiais que executa o shell atual, mas neste caso ela redireciona para outro tipo de shell, que não está mais restrito a letras maiúsculas. Dentro deste novo shell, escreva `bash` para carregar o terminal bash.

Agora, livres do terminal em maiúsculas, podemos ir para `/etc/bandit_pass/bandit33` e ler a senha do próximo level com `cat`.


