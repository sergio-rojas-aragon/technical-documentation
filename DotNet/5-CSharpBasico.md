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

# Funciones segun aplicacion

* **Buscar elemento dentro de una lista**

```csharp
List<int> numeros = new List<int> { 10, 20, 30, 40, 50 };
int numeroABuscar = 30;

// true or false
numeros.Contains(numeroABuscar)
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

# Estructura de datos comunes

## List (Lista) – List<T>

Una colección dinámica, crece y se reduce de forma automática. Permite acceso por índice.
Cuando necesitas una lista flexible de elementos con acceso rápido.

* Colección dinámica de elementos.
* Acceso por índice (como un arreglo mejorado).
* Permite agregar, eliminar y buscar datos.

```csharp
List<int> numeros = new List<int>();
numeros.Add(10);
numeros.Add(20);
numeros.Add(30);

Console.WriteLine(numeros[1]); // 20
```
 ## Queue (Cola) – Queue<T>

Estructura FIFO (First In, First Out).
El primero en entrar es el primero en salir.
Para procesos en orden de llegada (impresoras, colas de tareas).

```csharp
Queue<string> cola = new Queue<string>();
cola.Enqueue("A");
cola.Enqueue("B");
cola.Enqueue("C");

Console.WriteLine(cola.Dequeue()); // A
```

## Stack (Pila) – Stack<T>

Estructura LIFO (Last In, First Out).
El último en entrar es el primero en salir.
Deshacer operaciones, navegación hacia atrás, parsing.

```csharp
Stack<int> pila = new Stack<int>();
pila.Push(1);
pila.Push(2);
pila.Push(3);

Console.WriteLine(pila.Pop()); // 3
```

## Dictionary (Tabla Hash / Hash Table) – Dictionary<TKey, TValue>

Estructura basada en hashing.
Acceso por clave, no por índice.
Búsqueda muy rápida (O(1) promedio).
Cuando quieres buscar valores por una clave única.

```csharp
Dictionary<string, int> edades = new Dictionary<string, int>();
edades["Juan"] = 25;
edades["Ana"] = 30;

Console.WriteLine(edades["Ana"]); // 30
```

## LinkedList (Lista enlazada) – LinkedList<T>

Cada elemento apunta al siguiente (y al anterior si es doblemente enlazada).
Inserciones y eliminaciones rápidas en cualquier parte (O(1) si tienes el nodo).

```csharp
LinkedList<int> lista = new LinkedList<int>();
lista.AddLast(10);
lista.AddLast(20);
lista.AddFirst(5);
```

## HashSet (Conjunto) – HashSet<T>

No permite duplicados.
Uso típico: comprobar existencia rápida.

```csharp
HashSet<int> hs = new HashSet<int>();
hs.Add(1);
hs.Add(1); // No se agrega
hs.Add(2);

Console.WriteLine(hs.Count); // 2
```

## SortedList – `SortedList<TKey,TValue>`

Como un Dictionary pero mantiene las claves ordenadas.

```csharp
SortedList<int, string> sl = new SortedList<int, string>();
sl.Add(20, "B");
sl.Add(10, "A");

Console.WriteLine(sl[10]); // A
```

## SortedDictionary – `SortedDictionary<TKey,TValue>`

Similar a SortedList, pero más eficiente en inserciones grandes (usa árbol).

## SortedSet – SortedSet<T>

Como HashSet pero ordenado.

# Algorintmos de ordenamiento y busqueda

## Búsqueda Lineal (Linear Search)

Revisa elemento por elemento.

```csharp
int Buscar(int[] arr, int x) {
    for (int i = 0; i < arr.Length; i++)
        if (arr[i] == x) return i;
    return -1;
}
```
## Búsqueda Binaria (Binary Search)

```csharp
int BinarySearch(int[] arr, int x) {
    int i = 0, j = arr.Length - 1;

    while (i <= j) {
        int mid = (i + j) / 2;

        if (arr[mid] == x) return mid;
        else if (arr[mid] < x) i = mid + 1;
        else j = mid - 1;
    }
    return -1;
}
```

## Bubble Sort

```csharp
void BubbleSort(int[] arr) {
    for (int i = 0; i < arr.Length - 1; i++)
        for (int j = 0; j < arr.Length - 1 - i; j++)
            if (arr[j] > arr[j + 1])
                (arr[j], arr[j + 1]) = (arr[j + 1], arr[j]);
}
```

## Selection Sort
Busca el elemento menor y lo pone al inicio repetidamente.

```csharp
void SelectionSort(int[] arr) {
    for (int i = 0; i < arr.Length; i++) {
        int min = i;
        for (int j = i + 1; j < arr.Length; j++)
            if (arr[j] < arr[min]) min = j;

        (arr[i], arr[min]) = (arr[min], arr[i]);
    }
}
```

## Insertion Sort
Inserta cada elemento en la parte ya ordenada.

```csharp
void InsertionSort(int[] arr) {
    for (int i = 1; i < arr.Length; i++) {
        int key = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

## Merge Sort

Divide y combina (divide y vencerás).
Muy eficiente y estable.

```csharp
int[] MergeSort(int[] arr) {
    if (arr.Length <= 1) return arr;

    int mid = arr.Length / 2;
    var left = MergeSort(arr[..mid]);
    var right = MergeSort(arr[mid..]);

    return Merge(left, right);
}

int[] Merge(int[] a, int[] b) {
    List<int> res = new List<int>();
    int i = 0, j = 0;

    while (i < a.Length && j < b.Length)
        res.Add(a[i] < b[j] ? a[i++] : b[j++]);

    res.AddRange(a[i..]);
    res.AddRange(b[j..]);
    return res.ToArray();
}
```

## QuickSort

Divide usando un pivote.
Muy rápido en promedio.

```csharp
void QuickSort(int[] arr, int left, int right) {
    if (left >= right) return;

    int pivot = arr[(left + right) / 2];
    int i = left, j = right;

    while (i <= j) {
        while (arr[i] < pivot) i++;
        while (arr[j] > pivot) j--;

        if (i <= j) {
            (arr[i], arr[j]) = (arr[j], arr[i]);
            i++; j--;
        }
    }

    QuickSort(arr, left, j);
    QuickSort(arr, i, right);
}
```