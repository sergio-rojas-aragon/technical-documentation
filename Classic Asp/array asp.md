---
title: Arrays en ASP
layout: home
parent: Classic Asp
nav_order: 2
---

## Arrays en ASP

### funcion para recorrer arreglo que es resultado de una query

```
dim hay_prestamos 
dim prestamos 
sql = "select * from tabla where concepto >= 123"
set rst = dbs.Execute(sql) 
if not rst.EOF then 
    prestamos = rst.GetRows 
    hay_prestamos= true 
end if
```

```
function valor(a,b) 
    if hay_prestamos = true then 
    for x=0 to ubound(prestamos,2)
        if prestamos(0,x) = a and prestamos(1,x) = b then 
            valor = prestamos(2,x)
            exit function 
        end if 
        next 
    else 
    valor =0 
    end if 
end function