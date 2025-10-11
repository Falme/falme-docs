---
title: "PicoGym: GET aHEAD"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoGym: GET aHEAD

Navegando pelos desafios de web do [picoCTF](https://play.picoctf.org/practice/challenge/), o desafio escolhido se chama "GET aHEAD", o que deve significar algo sobre requisições de páginas web, talvez uma requisição GET, ou algo envolvendo um parâmetro ou resposta do cabeçalho (Head).

Primeiro, vamos olhar como é a página:

![Image description](/images/picogym-get-ahead/1.png)

É uma página simples que muda a cor de fundo.  
Se o usuário clicar no botão "Choose Red", a cor de fundo muda pra vermelho.  
Se o usuário clicar no botão "Choose Blue", a cor de fundo muda pra azul.  

Primeiro, vamos checar as requisições no [Postman](postman.com/), talvez eu consiga achar algo de interessante lá.

![Image description](/images/picogym-get-ahead/2.png)

Primeiro eu fiz uma requisição GET, mas nada de útil veio no header da resposta. Brincando um pouco mais com as opções, Eu encontrei a opção de requisição "HEAD", selecionei ele e tentei mandar uma requisição padrão:

![Image description](/images/picogym-get-ahead/3.jpg)

Só com essa mudança, a resposta da requisição tem a flag no header:

{{% details title="Resposta: Flag" open=false %}}
![Image description](/images/picogym-get-ahead/4.png)
```
picoCTF{r3j3ct_th3_du4l1ty_70bc61c4}
```
{{% /details %}}

Essa flag foi simples, a intenção é fazer o usuário aprender que não existe somente requisições GET e POST.
