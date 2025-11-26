---
title: "Level 24"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 24

## Objetivo 
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
> 
> NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

Em uma tradução livre, seria algo como:

> Um programa está sendo executado automaticamente em intervalos regulares pelo cron, o agendador de tarefas baseado em tempo. Verifique o arquivo /etc/cron.d/ para ver a configuração e qual comando está sendo executado.
> 
> NOTA: Este level exige que você crie seu primeiro script shell. Este é um passo muito importante e você deve se orgulhar de si mesmo ao concluir este level!
> 
> NOTA 2: Lembre-se de que seu script shell é removido após a execução, portanto, talvez seja interessante manter uma cópia...

## Comandos que você pode precisar para resolver esse level
> chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Passos

A forma de encontrar o script dentro da pasta cron.d é a mesma dos dois níveis anteriores (level 22 e level 23). Depois de encontrar o script `cronjob_bandit24.sh`, analise-o. Se não entender alguma parte, verifique o bloco de código comentado a seguir:

```shell 
#!/bin/bash # Executar como um script bash

myname=$(whoami) # salva na variável "myname" o usuário atual

cd /var/spool/$myname/foo # Entra em um diretório chamado "foo"
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*; # Percorre todos os arquivos da pasta.
do
    if [ "$i" != "." -a "$i" != ".." ]; # Se não for um diretório
    then
        echo "Handling $i" # Script atual que está lidando
        owner="$(stat --format "%U" ./$i)" # Obtenha o nome do dono do script.
        if [ "${owner}" = "bandit23" ]; then # se o dono for bandit23
            timeout -s 9 60 ./$i # Executa um script e feche se ultrapassar 1 minuto.
        fi
        rm -f ./$i # Deleta o script
    fi
done
```

Em outras palavras, existe um script que é executado periodicamente, verificando o diretório `/var/spool/bandit24/foo/` em busca de outros scripts e executando-os caso o dono seja bandit23. Após a execução, o script é excluído.

A solução para esse problema é copiar um script shell personalizado para o diretório `/var/spool/bandit24/foo/` que pega a senha para o próximo level. Primeiro, vamos criar um script shell chamado `getpass.sh` em uma pasta temporária usando o comando `mktemp -d`. Você pode editar o texto no seu editor de texto preferido (nano, vim, etc...).

O script que escrevi é o seguinte:

```shell 
#!/bin/bash

result=$(cat /etc/bandit_pass/bandit24)
echo "res" > /tmp/tmp.tempfolderhash/$result
```

Onde:
- `#!/bin/bash` : instrui o executor a usar o bash
- `result=$(cat /etc/bandit_pass/bandit24)` : salva a senha do bandit24 em uma variável chamada "result"
- `echo "res" > /tmp/tmp.tempfolderhash/$result`: salva um arquivo na pasta temporária com a senha como nome do arquivo, assim como no script de level anterior. ("res" é um argumento de descarte para o echo. Você poderia usar `touch`)

Agora, altere as permissões para que outros usuários possam executar o script usando `chmod a+x getpass.sh`, e copie o script para `/var/spool/bandit24/foo/` usando o comando `cp`:

```shell
cp ./getpass.sh /var/spool/bandit24/foo/getpass.sh
```

Agora, aguarde um pouco e a senha para o próximo level aparecerá como um arquivo no diretório temporário que você criou.


