---
title: "Montevideo"
weight: 7
# bookFlatSection: false
# bookToc: true
# bookHidden: true
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Microcorruption - Montevideo

Microcorruption nos dá um depurador e uma senha para desbloquear um dispositivo. Nossa tarefa é encontrar essa senha lendo o código assembly e usando algumas técnicas de engenharia reversa. Cada desafio leva o nome de uma cidade, e cada um é mais difícil que o anterior.

Se você ainda não resolveu o desafio sozinho, recomendo que pare de ler, resolva o desafio e confira minha solução depois. Não estrague o desafio e divirta-se!

## Como resolver

A solução para este problema é *quase* idêntica à do [Whitehorse](../whitehorse/), então verifique aquela antes de verificar esta.

Então, a resposta é a mesma, mas existe uma diferença crucial. O dado que inserimos está sendo copiado de um local da memória para outro.

Nossa entrada está sendo registrada entre `0x2400` e `0x242f`.
A cópia da nossa entrada está sendo copiada entre `0x43ee` e `0x441d`.

O problema é que a função que copia nossa entrada (`<strcpy>`) é interrompida se um byte zero for encontrado. Portanto, na entrada de Whitehorse, o valor era:

```
666666666666666666666666666666664c450000ff00
```

Mas não podemos usar os zeros aqui, ou o valor `ff00` não será copiado. Vamos substituir os zeros por mais 6s.

```
666666666666666666666666666666664c456666ff00
```

Com isso, a porta será destrancada.

