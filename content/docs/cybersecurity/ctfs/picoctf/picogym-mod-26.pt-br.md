---
title: "PicoGym: Mod 26"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoGym: Mod 26

Esse é um desafio do [picoCTF picoGym](https://play.picoctf.org/practice/challenge/144?category=2&page=1), a descrição do desafio descreve:

```
"Cryptography can be easy, do you know what ROT13 is? 
cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_uJdSftmh}"
```

Em uma tradução livre seria algo como "Criptografia pode ser fácil, você sabe o que é ROT13?".

A solução é bem direta, nós podemos usar um site online para decriptar a mensagem em ROT13.

Mas nesse caso, eu usei um script em bash pra fazer isso pra mim. Aqui está o script:

```bash
# Tentando achar a flag usando ROT13
# Onde $1 é: cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_uJdSftmh}

echo "== ROT13 =="
echo ""
output=$(echo "$1" | tr 'N-ZA-Mn-za-m' 'A-Za-z')
echo "${output}"
```

O comando `tr` é um comando de substituição, nesse caso, ele vai converter o A em um N, um B em um O, e assim vai. Esse é o significado de ROT13, ele vai mover cada caractere em 13 casas.

![Image description](/images/picogym-mod-26/1.jpg)

Essa encriptação é bidirecional, então não precisa se preocupar com a ordenação.

Rodando o script, a flag é revelada:

{{% details title="Resposta: Flag" open=false %}}
```
picoCTF{next_time_I'll_try_2_rounds_of_rot13_hWqFsgzu}
```
{{% /details %}}
