---
title: "Safe Opener"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoGym: Safe Opener

Hoje vamos resolver uma playlist no PicoGym chamada "Safe Opener", que consiste em 2 desafios escritos em Java. Então, vamos lá!


## Safe Opener 1

A descrição do desafio segue abaixo:

> Can you open this safe? 
> I forgot the key to my safe but this program is supposed to help me with retrieving the lost key. 
> Can you help me unlock my safe? 
> Put the password you recover into the picoCTF flag format like: 
> picoCTF{password}


Numa tradução livre, seria algo como:

> Você consegue abrir este cofre? 
> Eu esqueci a chave do meu cofre, mas esse programa deve me ajudar a recuperá-la. 
> Você pode me ajudar a abrir meu cofre? 
> Insira a senha recuperada no formato da flag picoCTF, como: 
> picoCTF{password}

Após baixar o arquivo chamado `SafeOpener.java`, podemos lê-lo usando o comando `cat` ou em qualquer editor de texto (como o Bloco de Notas ou gedit):


```java
import java.io.*;
import java.util.*;  
public class SafeOpener {
    public static void main(String args[]) throws IOException {
        BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
        Base64.Encoder encoder = Base64.getEncoder();
        String encodedkey = "";
        String key = "";
        int i = 0;
        boolean isOpen;

        while (i < 3) {
            System.out.print("Enter password for the safe: ");
            key = keyboard.readLine();

            encodedkey = encoder.encodeToString(key.getBytes());
            System.out.println(encodedkey);

            isOpen = openSafe(encodedkey);
            if (!isOpen) {
                System.out.println("You have  " + (2 - i) + " attempt(s) left");
                i++;
                continue;
            }
            break;
        }
    }

    public static boolean openSafe(String password) {
        String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";

        if (password.equals(encodedkey)) {
            System.out.println("Sesame open");
            return true;
        }
        else {
            System.out.println("Password is incorrect\n");
            return false;
        }
    }
}
```

Na verdade, não precisamos executar esse código. Podemos verificar o que ele faz e encontrar a flag.

Primeiro temos esta linha:

```java
Base64.Encoder encoder = Base64.getEncoder();
```

Então, talvez a senha codificada esteja em Base64, e nesse caso podemos encontrar essa linha dentro da função `openSafe`:

```java
String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";
```

Podemos estabelecer a conexão de que a `encodedkey` é a nossa flag e está codificada em base64.

Usarei o `radare2` para decodificar essa string, mas você pode usar qualquer decodificador, como o CyberChef.

Usar `rax2` nos dá a seguinte flag:

```bash
rax2 -D cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz

```

E o resultado:

```
pl3as3_l3t_m3_1nt0_th3_saf3
```

E formatando na estrutura da flag:

```
picoCTF{pl3as3_l3t_m3_1nt0_th3_saf3}
```



## Safe Opener 2

A descrição do desafio segue abaixo:

> What can you do with this file? 
> I forgot the key to my safe but this file is supposed to help me with retrieving the lost key. 
> Can you help me unlock my safe?


Numa tradução livre, seria algo como:

> O que você pode fazer com este arquivo? 
> Eu esqueci a chave do meu cofre, mas esse arquivo deve me ajudar a recuperá-la. 
> Você pode me ajudar a abrir meu cofre?


Após baixar o arquivo chamado `SafeOpener.class`, podemos lê-lo usando o comando `cat` ou em qualquer editor de texto (como o Bloco de Notas ou gedit):

```bash
cat SafeOpener.java
```

Embora o conteúdo esteja bastante bagunçado, podemos encontrar a flag facilmente lendo o arquivo.

```
picoCTF{SAf3_0p3n3rr_y0u_solv3d_it_b427942b}
```

