---
title: "Level 21"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 21

## Objetivo
> There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
> 
> NOTE: Try connecting to your own network daemon to see if it works as you think

Em uma tradução livre, seria algo como:

> Existe um binário setuid no diretório home que faz o seguinte: ele estabelece uma conexão com o localhost na porta que você especificar no argumento da linha de comando. Em seguida, lê uma linha de texto da conexão e a compara com a senha do level anterior (bandit20). Se a senha estiver correta, ele transmitirá a senha para o próximo level (bandit21).
> 
> NOTA: Tente conectar-se ao seu próprio daemon de rede para ver se funciona como você espera.

## Comandos que você pode precisar para resolver esse level
> ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)

## Passos

Este desafio nos ensina sobre as conexões entre dois locais que o netcat pode fazer (não muito neste caso, porque ambos estarão rodando no localhost, mas enfim...). 

Então, temos um novo executável em nosso diretório home chamado `suconnect`, que se conecta a uma porta e aguarda a chegada de informações. Para facilitar o entendimento, vamos abrir dois consoles, ambos conectados ao usuário "bandit20" (se preferir usar o `tmux` ou similar, sem problemas). Vamos chamá-los de "ConsoleA" e "ConsoleB".

No "ConsoleA", iniciaremos uma conexão de escuta do netcat em uma porta aleatória. Digamos que usamos a porta `4321`, que ficará mais ou menos assim:

```shell 
nc -l localhost 4321
```

Onde:
- `nc`: programa netcat para conexão
- `-l`: opção de escuta, para que possamos aguardar a nova senha.
- `localhost`: servidor para escutar
- `4321`: porta aleatória

Agora o "ConsoleA" está executando o netcat no modo de escuta. Agora, vamos para o "ConsoleB".

No "ConsoleB", vamos executar o programa `suconnect` na mesma porta que o netcat. Será algo como:

```shell
./suconnect 4321
```

Agora, ambos os consoles estarão ativos, só precisamos processar isso. Isso funcionará da seguinte maneira:

1. Inserimos a senha de bandit20 no "ConsoleA", que está executando o netcat.
2. O "ConsoleB", executando o `suconnect`, receberá a string do netcat.
3. O "ConsoleB", executando o `suconnect`, verificará se a senha está correta.
4. Se a senha estiver correta, uma nova senha será enviada de volta para "ConsoleA", que está executando o netcat.

Com isso, a senha para o próximo level é revelada.

