---
title: "Level 10"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 10

## Objetivo
> The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several '=' characters.

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt em uma das poucas sequências legíveis para humanos, precedida por vários caracteres '='.

## Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Passos

A maior parte do arquivo `data.txt` está preenchida com código de máquina, o que significa que não é legível por humanos. Poderíamos usar o comando `xxd` para ler o arquivo e procurar a senha manualmente, mas há muito conteúdo e isso levaria muito tempo.

Então, vamos pensar da seguinte forma: precisamos apenas da parte legível do arquivo, ou seja, apenas as strings contidas nele. Portanto, usaremos o comando `strings`, que exibe apenas as strings dentro de um arquivo, nada mais. O resultado será algo como:

```shell
strings data.txt
```

Só serão exibidas as strings do arquivo. Mas ainda é muito conteúdo. No texto do objetivo, diz que a senha é precedida por vários caracteres '=', então vamos usar o comando `grep` para filtrar esses caracteres. Mas quantos '=' são considerados "vários"? Vamos tentar 2, parece suficiente para a busca. O resultado será algo como:

```shell
strings data.txt | grep '=='
```

E com isso, a senha para o próximo level é revelada.
