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

Passing through the [picoCTF](https://play.picoctf.org/practice/challenge/) web challenges, the chosen challenge is called "GET aHEAD", which means that's something about requests of a web page, maybe a GET request, or something in the Header parameters response.

First, let's look at the page:

![Image description](/images/picogym-get-ahead/1.png)

A simple page that changes the background of the page.
If the user selects Red, the page background-color changes to red. 
If the user selects Blue, the page background-color changes to blue.

First let's check the requisition on the [Postman](postman.com/), maybe I can find something useful there.

![Image description](/images/picogym-get-ahead/2.png)

First I did a GET Request, but nothing useful came in the header response. Playing a little with the options, I found the request for HEAD, and then tried to send a standard request:

![Image description](/images/picogym-get-ahead/3.jpg)

With only that change, the response headers have the final CTF flag :

{{% details title="Resposta: Flag" open=false %}}
![Image description](/images/picogym-get-ahead/4.png)
```
picoCTF{r3j3ct_th3_du4l1ty_70bc61c4}
```
{{% /details %}}

That was a simple flag, to make the user learn that not only exists the GET and POST Request Methods.
