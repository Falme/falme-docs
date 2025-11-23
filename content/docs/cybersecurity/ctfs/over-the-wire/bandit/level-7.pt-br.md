---
title: "Level 7"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 7

## Objetivo
> The password for the next level is stored somewhere on the server and has all of the following properties:
>
>    owned by user bandit7  
>    owned by group bandit6  
>    33 bytes in size  

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada em algum lugar no servidor e possui todas as seguintes propriedades:
>
>    pertence ao usuário bandit7  
>    pertence ao grupo bandit6  
>    33 bytes de tamanho  

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

## Passos

Este desafio é uma continuação do level anterior (nível 6), a solução é praticamente a mesma de antes, mas com alguns parâmetros diferentes.

Primeiramente, o objetivo é buscar um arquivo em algum lugar do servidor, o que significa que não estamos limitados ao diretório `inhere` ou diretório home. Portanto, estaremos procurando nos arquivos em `/`, que é o diretório raiz, e a partir daí, podemos encontrar recursivamente todos os possíveis arquivos no sistema.

Então, vamos começar com as opções do comando `find` que já conhecemos, ou seja, a opção `-size`:

```shell
find / -size 33c
```

Este comando irá percorrer todo o sistema tentando encontrar um arquivo com 33 bytes. Os resultados mostram muitos arquivos, então precisamos filtrar melhor para encontrar o arquivo correto. Consultando o manual, podemos encontrar a opção `-user`:

```
-user nome-usuário
    O arquivo pertence ao usuário uname (ID numérico do usuário permitido). 
```

Este comando permite filtrar os arquivos que pertence ao usuário bandit7.

```shell
find / -size 33c -user bandit7
```

Ainda estão sendo exibidos muitos arquivos possíveis, então precisamos filtrar também por grupo. Consultando o manual do comando `find`, encontramos a opção `-group`:

```
-group nome-grupo
    O arquivo pertence ao grupo nome-grupo (ID numérico do grupo permitido).
```

Este comando permite filtrar os arquivos do grupo bandit6.

```shell
find / -size 33c -user bandit7 -group bandit6
```

Com isso, você já pode encontrar o arquivo com a senha para o próximo level. Mas, há muitos arquivos nos resultados aos quais não temos permissão de acesso. Então, vamos fazer uma limpeza para que apenas o arquivo correto seja exibido.

### File Descriptors

Já aprendemos sobre redirecionamento com os caracteres especiais `<` e `>`, agora vamos aprender sobre descritores de arquivo (File Descriptors). Os diferentes tipos de File Descriptors são:

- stdin: Representado pelo número 0, é a entrada.
- stdout: Representado pelo número 1, é a saída output comum.
- stderr: Representado pelo número 2, é a saída output de erro.

No nosso caso, queremos remover os erros da saída do nosso comando `find`. Portanto, vamos usar stderr (2) no exemplo.

Reescrevendo o comando anterior, estamos adicionando o seguinte redirecionamento:

```shell
2>/dev/null
```

Explicando um pouco a estrutura:

- `2`: é o stderr, ou erro, que o comando irá gerar, como uma tag da saída.
`>` : é o redirecionamento, queremos que as mensagens de erro vão para outro lugar, qualquer lugar que não seja o nosso console.
- `/dev/null`: é o caminho para um arquivo especial, usado para descartar informações, neste caso, queremos descartar os erros.

E com isso descartamos as mensagens de erro. No final, o comando final será algo como:

```shell
find / -size 33c -user bandit7 -group bandit6 2>/dev/null
```

Com o resultado, o arquivo correto é revelado. Usando o comando `cat` para ler o arquivo, vai encontrar a senha para o próximo level.
