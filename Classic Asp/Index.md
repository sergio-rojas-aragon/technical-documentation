---
title: Classic Asp
layout: home
nav_order: 3
has_children: True
---

# Apuntes de programacion Asp Clasico

## Funciones Basicas

### Select Case

```vbnet
Select Case VARIABLE
    Case 1
    formapago = "01"
    banco = Lpad(trim(rst2("banco")),"0",3)
    Case 2
    formapago = "02"
    banco = Lpad(trim(rst2("banco")),"0",3)
    Case 3
    formapago = "01"
    banco = Lpad(trim(rst2("banco")),"0",3)
    Case 4
    formapago = "30"	
    banco = "012"
    Case Else
    formapago = "22"
    banco = "012"
End Select
```
### for

```
for x=0 to cantidadmaxima
 
next 
```

otra forma:

```
For i = 0 To 5
  response.write("The number is " & i & "<br />")
Next
```

### round

```
response.write(Round(24.75122))
```

25 

### mayusculas

```
UCase(cadena)
```

### minusculas 

```
LCase(cadena)
```

### ubound 

cuenta el arreglo:

```
ubound(arreglo)
```

indica la cantidad de elementos que estan dentro de una matriz o array.

```
for x=0 to ubound(prestamos,2)
```
### valida elemento null

```vbnet
isNull(elemento)
```

### redimencionar array

```vbnet
ReDim cadenas(10)
```

