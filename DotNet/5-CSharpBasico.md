---
title: C Sharp Basico
layout: home
parent: DotNet
nav_order: 5
---

# C Sharp
{: .no_toc }

Como todas las pruebas de desarrollo se basan en pruebas de logica con caracteres lo cual te dice para nada la forma de desarrollar de alguien, decidi hacer esta pagina para no olvidar el uso de distintas funciones de caracteres.
{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


# Funciones básicas de string en C# para manejo de caracteres

* **IndexOf()** Devuelve la posición donde aparece un carácter o texto. Ej: Si no aparece, devuelve -1.

  ```csharp
  int pos = "Hola mundo".IndexOf("mundo");
  Console.WriteLine(pos); // 5
  ```

* **LastIndexOf()** Devuelve la última posición donde aparece el texto.

  ```csharp
  int pos = "abc abc abc".LastIndexOf("abc");
  Console.WriteLine(pos); // 8
  ```
* **Substring()** Extrae parte de una cadena.

  ```csharp
  string s = "Hola mundo".Substring(5); 
  Console.WriteLine(s); // mundo

  string s2 = "Hola mundo".Substring(0, 4);
  Console.WriteLine(s2); // Hola
  ```

* **Contains()** Verifica si un texto está contenido en la cadena.

  ```csharp
  bool tiene = "Hola mundo".Contains("mun");
  Console.WriteLine(tiene); // True
  ```

* **StartsWith() / EndsWith()** Verifica si la cadena empieza o termina con cierto texto.

  ```csharp
  "Programar".StartsWith("Pro"); // True
  "Programar".EndsWith("mar");  // True
  ```
  
* **Split()** Divide un string usando uno o varios caracteres.

  ```csharp
  string[] palabras = "uno,dos,tres".Split(',');

  foreach (var p in palabras)
      Console.WriteLine(p);
  ```
* **Replace()** Reemplaza texto simple sin usar regex.

  ```csharp
  string r = "hola mundo".Replace("o", "0");
  Console.WriteLine(r); // h0la mund0
  ```

# Funciones Regex

* **Regex.IsMatch()** Verifica si una cadena cumple o contiene un patrón.

  ```csharp
  bool resultado = Regex.IsMatch("Hola123", @"\d+");
  Console.WriteLine(resultado); // True
  ```

* **Regex.Match()** Devuelve la primera coincidencia con el patrón.

  ```csharp
  Match m = Regex.Match("ABC123XYZ", @"\d+");
  Console.WriteLine(m.Value); // 123
  ```

* **Regex.Matches()** Devuelve todas las coincidencias encontradas en la cadena.

  ```csharp
  MatchCollection mc = Regex.Matches("A1 B22 C333", @"\d+");
  foreach (Match m in mc)
      Console.WriteLine(m.Value);
  // 1
  // 22
  // 333
  ```

* **Regex.Replace()** Reemplaza el texto que coincide con el patrón por otro texto.

  ```csharp
  string r = Regex.Replace("hola 123", @"\d", "#");
  Console.WriteLine(r); // hola ###
  ```

* **Regex.Split()** Divide una cadena usando un patrón como delimitador.

  ```csharp
  string[] partes = Regex.Split("uno,dos;tres cuatro", @"[,; ]");

  foreach (var p in partes)
      Console.WriteLine(p);
  // uno
  // dos
  // tres
  // cuatro
  ```

# Expresiones Regulares

Ejemplo basico: buscar si un texto contiene una palabra:

```csharp
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

```csharp
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

```csharp
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

```csharp
Regex.IsMatch("2025", @"\d\d\d\d"); // True
```

> En C# muchas veces se usa el formato verbatim string con @ para evitar escapes dobles.

## Anclas (inicio y fin)

| Expresión | Significado     |
| --------- | --------------- |
| `^`       | inicio de línea |
| `$`       | fin de línea    |


Ejemplo: validar que el texto es solo números

```csharp
Regex.IsMatch("12345", @"^\d+$"); // True
Regex.IsMatch("12a45", @"^\d+$"); // False
```

## Grupos y captura

**grupo simple**

devuelve abc123

```csharp
Regex.Match("abc123", @"([a-z]+)(\d+)");
```

**Extraer valores***

```csharp
var m = Regex.Match("abc123", @"([a-z]+)(\d+)");
Console.WriteLine(m.Groups[1].Value); // abc
Console.WriteLine(m.Groups[2].Value); // 123
```

## Remplazos con Regex

```csharp
string resultado = Regex.Replace("hola 123", @"\d", "#");
Console.WriteLine(resultado); // hola ###
```

## Ejemplos prácticos comunes

* **validar Email**
  ```csharp
  Regex.IsMatch("correo@dom.com",
                @"^[\w\.-]+@[\w\.-]+\.\w{2,}$");
  ```

* **Validar numero de telefono simple**
  ```csharp
  Regex.IsMatch("311-555-1234",
                @"^\d{3}-\d{3}-\d{4}$");
  ```
* **Extraer números de una cadena**

  ```csharp
  var nums = Regex.Matches("A1 B22 C333", @"\d+");
  foreach (Match n in nums)
      Console.WriteLine(n.Value);

  // 1
  // 22
  // 333
  ```