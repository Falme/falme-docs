---
title: "Level 5"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 5

## Objetivo
> The password for the next level is stored in the only human-readable file in the `inhere` directory. Tip: if your terminal is messed up, try the “reset” command.

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada no único arquivo legível para humanos no diretório `inhere`. Dica: se o seu terminal estiver com problemas, tente o comando "reset".

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

## Passos

(Eu sei o que acontece no próximo level, então vamos jogar de forma mais simples 
nesse aqui)

O objetivo é encontrar um arquivo human-readable (legível para humanos) no diretório `inhere` contendo a senha para o próximo nível. Isso significa que o arquivo não está em código de máquina. 

O código de máquina é ilegível quando você tenta lê-lo por texto, então vamos tentar encontrar um arquivo de texto simples para leitura. Primeiro, entramos na pasta `inhere` com `cd`:

```shell
cd inhere
```

Listando os arquivos da pasta com o comando `ls`, temos a seguinte lista de arquivos:

```
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

Desta vez, usaremos o comando `file`, que exibe os detalhes e informações do arquivo. O problema é que `file < -file00` não funciona, precisamos usar outra forma para exibir seu conteúdo no console, sem chamar uma opção. Então usaremos `--`.

```shell
file -- -file00
```

Assim, usando o comando `file` em cada um dos arquivos, temos as seguintes informações:

```
-file00: data
-file01: OpenPGP Public Key
-file02: OpenPGP Public Key
-file03: data
-file04: data
-file05: data
-file06: data
-file07: ASCII text
-file08: data
-file09: data
```

As informações que reunimos são as seguintes:

- data : código de máquina, não legível para humanos
- OpenPGP Public Key : chave para criptografia, não legível para humanos
- ASCII text : Padrão de codificação de caracteres, legível para humanos

Com isso, temos o arquivo que queremos, `-file07`, e lemos seu conteúdo com o comando `cat`:

```shell
cat < -file07
```

Resultando na senha para o próximo level.
