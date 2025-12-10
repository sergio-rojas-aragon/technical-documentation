---
title: C Sharp Basico
layout: home
parent: DotNet
nav_order: 5
---

# Csharp
{: .no_toc }

Como todas las pruebas de desarrollo se basan en pruebas de logica con caracteres lo cual te dice para nada la forma de desarrollar de alguien, decidi hacer esta pagina para no olvidar el uso de distintas funciones de caracteres.
{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Expresiones Regulares

Ejemplo basico: buscar si un texto contiene una palabra:

```CSharp
string texto = "Hola mundo";
bool encontrado = Regex.IsMatch(texto, "mundo");
Console.WriteLine(encontrado);  // True
```

## Clases de caracteres

Las clases de caracteres permiten definir qué caracteres son válidos.

| Expresión | Significado                         |
| --------- | ----------------------------------- |
| `[abc]`   | a, b o c                            |
| `[^abc]`  | cualquier carácter excepto a, b o c |
| `[a-z]`   | letras minúsculas                   |
| `[A-Z]`   | letras mayúsculas                   |
| `[0-9]`   | dígitos                             |

Ejemplo:

```CSharp
Regex.IsMatch("hola", "[a-z]");   // True
```

# Cuantificadores

Sirven para indicar cuántas veces debe aparecer un carácter o patrón.

| Cuantificador | Significado   |
| ------------- | ------------- |
| `*`           | 0 o más       |
| `+`           | 1 o más       |
| `?`           | 0 o 1         |
| `{n}`         | exactamente n |
| `{n,}`        | al menos n    |
| `{n,m}`       | entre n y m   |


Ejemplo, validar que hay 3 números consecutivos

```CSharp
Regex.IsMatch("abc123def", "[0-9]{3}");  // True
```

## Escapes y caracteres especiales

Algunos caracteres deben “escaparse” usando `\`.

| Carácter | Significado                  |
| -------- | ---------------------------- |
| `\d`     | dígito (0-9)                 |
| `\w`     | palabra (letras, números, _) |
| `\s`     | espacio en blanco            |
| `.`      | cualquier carácter           |

Ejemplo:

```CSharp
Regex.IsMatch("2025", @"\d\d\d\d"); // True
```

> En C# muchas veces se usa el formato verbatim string con @ para evitar escapes dobles.

## Anclas (inicio y fin)

| Expresión | Significado     |
| --------- | --------------- |
| `^`       | inicio de línea |
| `$`       | fin de línea    |


Ejemplo: validar que el texto es solo números

```CSharp
Regex.IsMatch("12345", @"^\d+$"); // True
Regex.IsMatch("12a45", @"^\d+$"); // False
```

## Grupos y captura

**grupo simple**

devuelve abc123

```CSharp
Regex.Match("abc123", @"([a-z]+)(\d+)");
```

**Extraer valores***

```CSharp
var m = Regex.Match("abc123", @"([a-z]+)(\d+)");
Console.WriteLine(m.Groups[1].Value); // abc
Console.WriteLine(m.Groups[2].Value); // 123
```

## Remplazos con Regex

```CSharp
string resultado = Regex.Replace("hola 123", @"\d", "#");
Console.WriteLine(resultado); // hola ###
```

## Ejemplos prácticos comunes

* **validar Email**
  ```CSharp
  Regex.IsMatch("correo@dom.com",
                @"^[\w\.-]+@[\w\.-]+\.\w{2,}$");
  ```

* **Validar numero de telefono simple**
  ```CSharp
  Regex.IsMatch("311-555-1234",
                @"^\d{3}-\d{3}-\d{4}$");
  ```
* **Extraer números de una cadena**

  ```CSharp
  var nums = Regex.Matches("A1 B22 C333", @"\d+");
  foreach (Match n in nums)
      Console.WriteLine(n.Value);

  // 1
  // 22
  // 333
  ```