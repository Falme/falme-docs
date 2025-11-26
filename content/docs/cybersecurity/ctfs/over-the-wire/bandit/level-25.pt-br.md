---
title: "Level 25"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 25

## Objetivo
> A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
> You do not need to create new connections each time

Em uma tradução livre, seria algo como:

> Um daemon está escutando na porta 30002 e fornecerá a senha para bandit25 se você fornecer a senha para bandit24 e um código PIN numérico secreto de 4 dígitos. Não há como recuperar o código PIN, exceto testando todas as 10.000 combinações, um processo conhecido como força bruta.
> Você não precisa criar novas conexões a cada vez.

## Comandos que você pode precisar para resolver esse level
> 

## Passos

Para este desafio, usaremos o `ncat` para a conexão na porta 30002 e um script para gerar um texto. A conexão é bastante simples:

```shell
ncat localhost 30002
```

Isso se conecta a um daemon, que aguarda a combinação senha_bandit24+pincode. Mas tentar uma por uma levaria uma eternidade. Uma vantagem é que o `ncat` pode receber um arquivo de texto com entradas e processá-lo normalmente. Então, imagine que temos o seguinte arquivo `list.txt`:

```
senha_bandit24 0
senha_bandit24 1
senha_bandit24 2
senha_bandit24 3
```

Se inserirmos o arquivo desta forma:

```shell
ncat localhost 30002 < .list.txt
```

Cada linha será processada como uma entrada comum do usuário. Bom, ok, precisamos escrever 10.000 linhas assim. Isso também levaria uma eternidade. Então, vamos criar um script shell para fazer isso por nós. Vamos criar um script chamado `create_list.sh`, torná-lo executável com `chmod +x create_list.sh` e editá-lo com algo como isto:

```shell
for i in {0..10000}
do
echo "senha_bandit24 $i" >> ./list.txt
done
```

Onde:
- `for i in {0..10000}`: é um loop que se repete de 0 a 10000, salvando a posição atual na variável `$i`.
- `echo "password_bandit24 $i"` : imprime a string com a senha + código PIN da variável
- `>> ./list.txt`: adiciona uma nova linha a um arquivo, neste caso no arquivo `list.txt`, a cada interação do loop.

Execute este script com `./create_list.sh` e o arquivo conterá todas as linhas necessárias para encontrar a próxima senha. Agora, para obter a próxima senha, basta executar `ncat` com a lista preenchida.

```shell
ncat localhost 30002 < .list.txt
```




