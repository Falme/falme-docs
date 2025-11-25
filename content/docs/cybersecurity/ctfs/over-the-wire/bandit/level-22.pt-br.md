---
title: "Level 22"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 22

## Objetivo
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

Em uma tradução livre, seria algo como:

> Um programa está sendo executado automaticamente em intervalos regulares pelo cron, o agendador de tarefas baseado em tempo. Dê uma olhada em /etc/cron.d/ para achar a configuração e qual comando está sendo executado.

## Comandos que você pode precisar para resolver esse level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Passos

Desta vez, vamos analisar os arquivos de configuração do `cron`. O `cron` é um agendador de tarefas que roda em segundo plano para executar tarefas periodicamente. As tarefas podem ser encontradas no caminho /etc/cron.d/, então vamos dar uma olhada lá.

Encontramos alguns arquivos, mas no momento o arquivo `cronjob_bandit22` é o mais adequado ao nosso objetivo. Usando o comando `cat` para ler o arquivo, podemos encontrar o seguinte caminho:

```
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Sabemos que /dev/null é um arquivo de descarte especial, mas pode haver algo no script /usr/bin/cronjob_bandit22.sh que nos ajude. Portanto, usamos `cat` novamente para verificar o que ele faz.

O programa grava periodicamente a senha de /etc/bandit_pass/bandit22 em um arquivo na pasta /tmp/. Ao abrir esse arquivo na pasta /tmp/, a senha para o próximo nível é revelada.



