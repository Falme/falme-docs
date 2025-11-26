---
title: "Level 27"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 27

## Objetivo
> Good job getting a shell! Now hurry and grab the password for bandit27!

Em uma tradução livre, seria algo como:

> Ótimo trabalho por ter conseguido entrar no shell! Agora se apresse e pegue a senha do bandit27!

## Comandos que você pode precisar para resolver esse level
> ls

## Passos

Ainda temos problemas não resolvidos para o bandit26, então precisamos repetir os mesmos passos do level anterior (level 26) até que o modo visual do Vim seja ativado (após pressionar `v` para editar).

Nesse momento, precisamos executar um programa chamado `bandit-27-do`, mas não conseguimos acessá-lo com o terminal atual. Portanto, vamos abrir um novo terminal dentro do editor vim com o comando:

```
:term bash
```

Com este comando, vamos abrir um shell bash separado. Agora podemos executar `bandit27-do`, que é semelhante a `bandit20-do`, simulando que somos o bandit27 e executando um comando. Então, abrimos o shell com a senha do bandit27 usando:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Agora, a senha para o próximo level foi revelada.


