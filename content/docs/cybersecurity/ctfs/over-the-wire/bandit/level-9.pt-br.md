---
title: "Level 09"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 09

## Objetivo
> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt e é a única linha de texto que aparece apenas uma vez.

## Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Passos

Precisamos encontrar uma linha única no arquivo data.txt que não se repete, porque esse arquivo está repleto de possíveis senhas. O primeiro instinto é usar o comando `uniq`, pois, em sua descrição:

```
uniq - reportar ou omitir linhas repetidas
```

E é correto usar o comando `uniq`, que mostra apenas as linhas que são únicas e não repetidas. Mas, no final da página do manual do `uniq`, temos:

```
'uniq' não detecta linhas repetidas a menos que sejam adjacentes.
       Você pode querer ordenar a entrada primeiro ou usar 'sort -u' sem
       'uniq'.
```

Então, antes de usarmos o comando `uniq` para filtrar as linhas repetidas, precisamos ordenar as linhas alfabeticamente e, em seguida, usar o comando `uniq`. O comando será algo como:

```shell
sort data.txt | uniq -u
```

Where:
- `sort`: ordena as linhas alfabeticamente.
- `data.txt`: arquivo a ser lido e processado
- `|` : caractere especial de pipe, que envia o conteúdo de `data.txt` para o comando `uniq`
- `uniq`: comando para reportar ou omitir linhas repetidas
- `-u`: opção do `uniq` para mostrar apenas linhas únicas

Com isso, apenas a senha para o próximo level será mostrada.
