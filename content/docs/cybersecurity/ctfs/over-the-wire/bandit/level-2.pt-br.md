---
title: "Level 02"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 02

## Objetivo
> The password for the next level is stored in a file called - located in the home directory

Em uma tradução livre, seria algo como:

> A senha para o próximo level está guardada num arquivo chamado - localizado no diretório home

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

## Passos

Nesse caso o arquivo se chama literalmente `-`, você pode listar o arquivo no 
diretório usando o comando `ls` e pode averiguar que o arquivo está mesmo lá.

Mas não é possível só usar o comando `cat -` pra ler o arquivo. Entenda que, o 
caractere traço é um caractere especial, ele é usado para definir opções em um 
programa. Vamos ver outro exemplo:

```shell
ls -l 
```

Nesse caso, o comando `ls` está usando a opção `-l`, na qual lista os arquivos 
em um formato de lista longa, com mais detalhes.

Então não podemos usar `cat -`, porque cat está selecionando uma opção a ser 
executada. Pra fazer isso funcionar nós precisamos de uma alternativa (e assim 
a razão pra esse desafio), porque cat está esperando um input de um arquivo, 
nós vamos usar um pouco de redirecionamento Input/Output (I/O redirection).

Primeiro vamos explicar o que é Input/Output redirection. Se nós fizermos algo 
como `echo "test text" > file.txt`, isso significa que nós iremos escrever no 
console usando o comando `echo` o texto `"test text"`, e então redirecionar o 
Output para um arquivo chamado `file.txt` usando o sinal `>`.

Ok, então se `>` significa output (saída), o `<` significa input (entrada), 
certo?

Então vamos colocar o arquivo `-` dentro do comando `cat`, será algo como:

```shell
cat < -
```

E com isso, nós temos a senha para o próximo level.
