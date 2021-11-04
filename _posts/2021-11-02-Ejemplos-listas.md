---
typora-copy-images-to: ../assets/images
typora-root-url: ../
title: Listas en python
---

Ejemplos de listas:

```python
def listas()
	# Definir lista
	lista1 = ['python','javascript','html']
    # Recorrer elementos de lista
    for x in lista:
        print(x)
    lista2 = ['Firefox','Edge','Chrome']
    # Concatenar listas
    print(lista1+lista2)
    # Coger un elemento de la lista
    print(lista1[2])
    # Coger elementos de una lista. Ojo que empieza en 0 y el último
    # no cuenta, como en range
    print(lista2[0:3])
    # Recorrer elementos de lista teniendo en cuenta su posición
    for pos, val in enumerate(lista1):
        print(str(val) + ' está en la posición ' + str(pos))
```

