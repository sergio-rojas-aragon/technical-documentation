---
title: Classic Asp
layout: home
nav_order: 1
has_children: True
---

# Apuntes de programacion Asp Clasico

## Funciones Basicas

### Select Case

```
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

