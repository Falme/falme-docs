---
title: "Level 23"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 23

## Objetivo 
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

Em uma tradução livre, seria algo como:

> Um programa está sendo executado automaticamente em intervalos regulares pelo cron, o agendador de tarefas baseado em tempo. Verifique o arquivo /etc/cron.d/ para ver a configuração e qual comando está sendo executado.
> 
> NOTA: Analisar scripts de shell escritos por outras pessoas é uma habilidade muito útil. O script para este level foi intencionalmente desenvolvido para ser fácil de ler. Se você estiver com dificuldades para entender o que ele faz, tente executá-lo para ver as informações de depuração que ele imprime.

## Comandos que você pode precisar para resolver esse level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Passos

Os passos para acessar o script que está sendo executado na rotina `cron` são os mesmos do level anterior (level 22):

- Abra a pasta /etc/cron.d/ com o comando `cd`.
- Leia o conteúdo do arquivo `cronjob_bandit23`
- Acesse o caminho do script dentro dele (`cronjob_bandit23.sh`)
- Leia o script 

Este é um script bastante simples: ele obtém o usuário atualmente logado com o comando `whoami`, codifica o resultado com um codificador `md5sum` e remove os caracteres desnecessários. O resultado dessa codificação é o nome do arquivo na pasta /tmp/ que contém a senha para o próximo level.

Para encontrar o resultado, basta repetir corretamente os passos do script até chegar ao caminho do arquivo correto. É algo como:

```shell 
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Where
- `echo I am user bandit23` : Escreve uma string para ser redirecionada para `md5sum` e bandit23 é o usuário simulado.
- `md5sum`: codifica em MD5 a última string
- `cut -d ' ' -f 1`: remove os caracteres extras desnecessários

Ao ler o arquivo no caminho encontrado, a senha do próximo level é revelada.


