---
title: "Level 18"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 18

## Objetivo 
> There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
> 
> NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

Em uma tradução livre, seria algo como:

> Existem dois arquivos no diretório inicial: passwords.old e passwords.new. A senha para o próximo level está em passwords.new e é a única linha que foi alterada entre passwords.old e passwords.new.
> 
> NOTA: se você concluiu este level e ver um "Byebye!" ao tentar entrar no bandit18, isso está relacionado ao próximo level, bandit19.

## Comandos que você pode precisar para resolver esse level
> cat, grep, ls, diff

## Passos


Este desafio tem como objetivo nos ensinar sobre o comando `diff`. O `diff` compara dois arquivos e mostra a diferença entre eles, sendo muito usado com o `git` e outros programas de controle de versão. A ideia é que, dado um arquivo antigo e um novo, o comando mostrará o que mudou entre os dois.

O caractere "<" representa uma diferença que saiu do arquivo, e o caractere ">" representa uma diferença que entrou no arquivo. Então, vamos comparar os arquivos:

```shell
diff passwords.old passwords.new
```

A senha para o próximo level deve estar ao lado do caractere ">".
