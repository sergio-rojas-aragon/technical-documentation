
---
title: POO
layout: home
parent: Arquitecturas
nav_order: 2
---
# Conceptos Base en la Programacion Orientada a Objetos Aplicado
{: .no_toc }

Para estos ejercicios se utilizo **.Net8** en una Consola Simple

{: .no_toc .text-delta }

1. TOC
{:toc}


## Abstraccion

La abstracci�n es un principio fundamental de la Programaci�n Orientada a Objetos (POO) que consiste en:

> Modelar solo los aspectos relevantes de un objeto, ocultando los detalles de implementaci�n.

En otras palabras:

* Te enfocas en qu� hace un objeto, no c�mo lo hace.
* Permite simplificar sistemas complejos, exponiendo solo lo necesario a trav�s de interfaces o clases abstractas.

En .NET puedes aplicar abstracci�n de dos maneras principales:

### Con clases abstractas

* Son clases base que no pueden instanciarse.
* Pueden definir m�todos implementados y no implementados.
* Los m�todos abstractos deben implementarse en las clases derivadas.

El ejemplo se puede encontrar en la carpeta `Abstraccion`

### Con interfaces

Se puede aplicar Abstraccion utilizando interfaces siguiendo este orden:

Interfaz => Abstraccion => Clase a implementar

* Una interfaz define un contrato que las clases deben cumplir.
* No tiene l�gica interna ni estado, solo firmas de m�todos o propiedades.
* Una clase puede implementar m�ltiples interfaces (a diferencia de la herencia simple).

**Diferencias entre Abstraccion y Encapsulacion**


| Concepto          | Qu� hace                                                             | C�mo se aplica                          |
| ----------------- | -------------------------------------------------------------------- | --------------------------------------- |
| **Encapsulaci�n** | Oculta los **datos** internos de un objeto                           | Uso de `private`, `public`, propiedades |
| **Abstracci�n**   | Oculta los **detalles de implementaci�n** y muestra solo lo esencial | Clases abstractas, interfaces           |




El ejemplo se puede encontrar en la carpeta `AbstraccionInterfaces`


## Herencia

Crear una nueva clase (derivada o hija) a partir de una clase existente (base o padre), heredando sus propiedades, m�todos y comportamiento.

Esto permite reutilizar c�digo, extender funcionalidades y mantener una estructura jer�rquica en el dise�o.


| T�rmino                   | Significado                                                                                                           |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Clase base (padre)**    | La clase original que contiene atributos y m�todos comunes.                                                           |
| **Clase derivada (hija)** | La clase que hereda de la base y puede agregar o modificar comportamiento.                                            |
| **Herencia simple**       | En C#, una clase **solo puede heredar de una clase base**.                                                            |
| **Palabras clave**        | `:` (dos puntos) para heredar, `base` para acceder al constructor o m�todos del padre, `override` para sobreescribir. |

## Encapsulacion

La encapsulaci�n es el principio de ocultar los datos internos de un objeto y controlar c�mo se accede o modifica esa informaci�n.

Objetivo

* Evitar acceso directo a los datos (por ejemplo, que se modifiquen sin control).
* Asegurar consistencia en el estado del objeto.
* Separar la implementaci�n interna de la interfaz p�blica.

Sin la encapsulacion no hay control ni valdacion sobre los datos. Esto Rompe la integridad del objeto.

## Polimorfismo

Un mismo m�todo o acci�n puede comportarse de distintas maneras seg�n el tipo de objeto que lo ejecute.

El polimorfismo permite tratar diferentes tipos de objetos de una misma familia (por herencia) como si fueran del mismo tipo, pero ejecutando el comportamiento propio de cada uno.

Tipos de polimorfismo en C# / .NET

| Tipo                                    | Cu�ndo ocurre                     | C�mo se implementa                                  |
| --------------------------------------- | --------------------------------- | --------------------------------------------------- |
| **En tiempo de compilaci�n (est�tico)** | Durante la compilaci�n            | *Sobrecarga de m�todos* (`overloading`)             |
| **En tiempo de ejecuci�n (din�mico)**   | Durante la ejecuci�n del programa | *Sobrescritura de m�todos* (`virtual` / `override`) |


Para que un m�todo sea polim�rfico:

* Debe estar declarado como virtual o abstract en la clase base.
* Debe sobrescribirse con override en la clase derivada.

Si no usas virtual/override, C# no hará polimorfismo real,
sino que llamará siempre al método de la clase base (esto se llama ocultamiento y se hace con new, no override).

## Genericos

Los genéricos (en inglés Generics) permiten crear clases, métodos, estructuras o interfaces que funcionan con diferentes tipos de datos, sin perder seguridad de tipos (type safety).

Los genéricos permiten escribir código reutilizable y flexible, donde el tipo de dato se especifica al momento de usarlo, no al momento de declararlo.

Ventajas principales:

| Ventaja                     | Descripción                                                     |
| --------------------------- | --------------------------------------------------------------- |
| **Reutilización de código** | Escribes una sola clase o método que sirve para cualquier tipo. |
| **Seguridad de tipos**      | El compilador detecta errores de tipo en tiempo de compilación. |
| **Rendimiento**             | Evita conversiones entre tipos (no hay *boxing/unboxing*).      |
| **Legibilidad**             | El código es más claro y expresivo sobre qué tipo maneja.       |

Tipos de uso:

* Repositorio genérico: Un ejemplo común en .NET es el patrón repositorio con genéricos, que permite manejar distintos tipos de entidades.

```
public class Repositorio<T>
{
    private List<T> elementos = new List<T>();

    public void Agregar(T item)
    {
        elementos.Add(item);
    }

    public List<T> ObtenerTodos()
    {
        return elementos;
    }
}
```

* Metodos Genericos: También puedes hacer métodos genéricos, sin necesidad de que toda la clase lo sea.

```
public class Utilidades
{
    public static void MostrarTipo<T>(T dato)
    {
        Console.WriteLine($"El tipo de dato es: {typeof(T)} y su valor es: {dato}");
    }
}

```


### Restricciones

Cuando creas una clase o método genérico en C#, normalmente usas un parámetro de tipo genérico (por ejemplo T).
Sin restricciones, T podría representar cualquier tipo, y por tanto el compilador no te dejaría usar sus miembros específicos.

Las restricciones de tipo sirven para decirle al compilador qué tipo de datos puede usar el genérico.
Así puedes acceder con seguridad a métodos, propiedades o constructores del tipo T.

| Restricción            | Descripción                                                                                | Ejemplo                                              |
| ---------------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------- |
| `where T : class`      | Solo permite **tipos de referencia** (clases, interfaces, delegados, arrays).              | `T` puede ser `string`, `Figura`, etc.               |
| `where T : struct`     | Solo permite **tipos de valor** (int, double, etc.).                                       | `T` puede ser `int`, `float`, pero no `string`.      |
| `where T : new()`      | El tipo debe tener un **constructor público sin parámetros** (para poder hacer `new T()`). | Crear instancias genéricas.                          |
| `where T : BaseClass`  | `T` debe **heredar** de una clase base específica.                                         | `where T : Figura`                                   |
| `where T : IInterface` | `T` debe **implementar** una interfaz específica.                                          | `where T : IFigura`                                  |
| `where T : unmanaged`  | Solo tipos **no administrados** (sin referencias).                                         | `int`, `double`, `structs` simples. *(más avanzado)* |

Se puede usar mas de una restriccion a la vez, por ejemplo:

```
where T : Figura, new()
```

Esto significa: “T debe heredar de Figura y tener un constructor sin parámetros”.

