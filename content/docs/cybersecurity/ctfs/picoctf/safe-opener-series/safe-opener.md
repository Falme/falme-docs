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

We are going to solve a playlist in PicoGym called "Safe Opener" today, it consists in 2 challenges written in Java. So, let's do it!


## Safe Opener 1

The description of the challenge follows:

> Can you open this safe? 
> I forgot the key to my safe but this program is supposed to help me with retrieving the lost key. 
> Can you help me unlock my safe? 
> Put the password you recover into the picoCTF flag format like: 
> picoCTF{password}

After downloading the file named `SafeOpener.java`, we can read it using the `cat` command or using any text editor (like notepad or gedit):


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

We don't really need to execute this code, we can check what it does and find the flag.

First we have this line:

```java
Base64.Encoder encoder = Base64.getEncoder();
```

So, maybe the encoded password is in Base64, and then we can find this line inside the `openSafe` function:

```java
String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";
```

We can make the connection that the `encodedkey` is our flag, and is encoded in base64.

I will use `radare2` to decode this string, but you can use any decoder such as cyberchef.

Using `rax2` gives us the flag:

```bash
rax2 -D cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz

```

And outputs:

```
pl3as3_l3t_m3_1nt0_th3_saf3
```

And formatting to the flag structure:

```
picoCTF{pl3as3_l3t_m3_1nt0_th3_saf3}
```



## Safe Opener 2

The description of the challenge follows:

> What can you do with this file? 
> I forgot the key to my safe but this file is supposed to help me with retrieving the lost key. 
> Can you help me unlock my safe?

After downloading the file named `SafeOpener.class`, we can read it using the `cat` command or using any text editor (like notepad or gedit):

```bash
cat SafeOpener.java
```

Even though the content is very messy, we can find the flag easily reading through the file.

```
picoCTF{SAf3_0p3n3rr_y0u_solv3d_it_b427942b}
```
