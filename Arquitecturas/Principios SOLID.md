---
title: SOLID
layout: home
parent: Arquitecturas
nav_order: 4
---

## Principios SOLID

| Letra | Nombre                              | Propósito                                                                              |
| ----- | ----------------------------------- | -------------------------------------------------------------------------------------- |
| **S** | **Single Responsibility Principle** | Cada clase debe tener una única responsabilidad.                                       |
| **O** | **Open/Closed Principle**           | Abierto a extensión, cerrado a modificación.                                           |
| **L** | **Liskov Substitution Principle**   | Las clases hijas deben poder reemplazar a la clase base sin alterar el funcionamiento. |
| **I** | **Interface Segregation Principle** | No obligar a las clases a implementar métodos que no usan.                             |
| **D** | **Dependency Inversion Principle**  | Depender de abstracciones, no de implementaciones concretas.                           |


### S — Single Responsibility Principle (Responsabilidad Única)

**EJEMPLO MALO:***

Problema:

> La clase Reporte tiene múltiples responsabilidades, Generar el reporte, Guardarlo en archivo, Enviarlo por correo.

```

// Si cambia una forma de envío o guardado, hay que modificar esta clase (difícil de mantener y probar).
internal class SingleResponSabilityMAL
{
    public string GenerarReporte()
    {
        return "Contenido del reporte";
    }

    public void GuardarArchivo(string contenido)
    {
        // Guardar en archivo
        File.WriteAllText("reporte.txt", contenido);
    }

    public void EnviarPorCorreo(string contenido)
    {
        // Lógica de envío de correo
        Console.WriteLine("Correo enviado con reporte");
    }
}

```

**BIEN (una clase = una responsabilidad)**

```
public class Reporte
{
    public string GenerarReporte()
    {
        return "Contenido del reporte";
    }
}

public class GuardarArchivo
{
    public void Guardar(string contenido)
    {
        File.WriteAllText("reporte.txt", contenido);
    }
}

public class EnviarCorreo
{
    public void Enviar(string contenido)
    {
        Console.WriteLine("Correo enviado con reporte");
    }
}
```

### O — Open/Closed Principle (Abierto/Cerrado)

Las clases deben estar abiertas a extensión, pero cerradas a modificación.

**MAL ejemplo (cada vez modificamos la clase)**

Cada vez que aparece un nuevo tipo de cliente, hay que modificar la clase, violando el principio.

```
public class CalculadoraDescuento
{
    public double CalcularDescuento(string tipoCliente, double monto)
    {
        if (tipoCliente == "Normal")
            return monto * 0.05;
        else if (tipoCliente == "VIP")
            return monto * 0.10;
        else if (tipoCliente == "Premium")
            return monto * 0.15;
        else
            return 0;
    }
}
```

**BIEN (extensible sin modificar)**

Puedes agregar DescuentoGold, DescuentoEstudiante, etc. sin modificar la clase principal.
Código más flexible y mantenible.

```
public abstract class Descuento
{
    public abstract double Calcular(double monto);
}

public class DescuentoNormal : Descuento
{
    public override double Calcular(double monto) => monto * 0.05;
}

public class DescuentoVIP : Descuento
{
    public override double Calcular(double monto) => monto * 0.10;
}

public class DescuentoPremium : Descuento
{
    public override double Calcular(double monto) => monto * 0.15;
}

// Contexto abierto a extensión
public class CalculadoraDescuento
{
    public double ObtenerDescuento(Descuento tipo, double monto)
    {
        return tipo.Calcular(monto);
    }
}

```

### L — Liskov Substitution Principle (Sustitución de Liskov)

Las clases hijas deben poder reemplazar a la clase base sin romper la lógica del programa.

**MAL ejemplo**

Pinguino hereda de Ave, pero rompe el contrato: no puede volar, lo que genera errores si se trata como una Ave.

```
public class Ave
{
    public virtual void Volar()
    {
        Console.WriteLine("Estoy volando");
    }
}

public class Pinguino : Ave
{
    public override void Volar()
    {
        throw new NotSupportedException("Los pingüinos no vuelan");
    }
}

```

**BIEN**

Separar las responsabilidades con clases e interfaces adecuadas:

Ahora el código no asume que todas las aves vuelan.Pinguino cumple su rol sin romper el principio.

```
public abstract class Ave
{
    public abstract void Comer();
}

public interface IVolador
{
    void Volar();
}

public class Aguila : Ave, IVolador
{
    public override void Comer() => Console.WriteLine("El águila come carne");
    public void Volar() => Console.WriteLine("El águila vuela alto");
}

public class Pinguino : Ave
{
    public override void Comer() => Console.WriteLine("El pingüino come peces");
}
```

### I — Interface Segregation Principle (Segregación de Interfaces)

Las clases no deben estar forzadas a implementar métodos que no usan.

**MAL ejemplo**

```
Cuadrado está forzado a implementar un método que no aplica a su tipo.

public interface IFigura
{
    double CalcularArea();
    double CalcularVolumen();
}

public class Cuadrado : IFigura
{
    public double CalcularArea() => 4 * 4;
    public double CalcularVolumen()
    {
        throw new NotImplementedException(); // ❌ Cuadrado no tiene volumen
    }
}
```

**BIEN**

Dividir la interfaz en partes específicas
Cada clase implementa solo lo que necesita.
Las interfaces son más específicas y limpias.

```
public interface IFigura2D
{
    double CalcularArea();
}

public interface IFigura3D
{
    double CalcularVolumen();
}

public class Cuadrado : IFigura2D
{
    public double CalcularArea() => 4 * 4;
}

public class Cubo : IFigura3D
{
    public double CalcularVolumen() => 4 * 4 * 4;
}
```


### D — Dependency Inversion Principle (Inversión de Dependencias)

Los módulos de alto nivel no deben depender de módulos de bajo nivel, ambos deben depender de abstracciones (interfaces o clases abstractas).

**MAL ejemplo (dependencia directa)**

Auto depende directamente de una clase concreta Motor.
Si mañana hay un MotorElectrico, tendrías que modificar Auto.

```
public interface IMotor
{
    void Encender();
}

public class MotorGasolina : IMotor
{
    public void Encender() => Console.WriteLine("Motor de gasolina encendido");
}

public class MotorElectrico : IMotor
{
    public void Encender() => Console.WriteLine("Motor eléctrico encendido");
}

public class Auto
{
    private readonly IMotor motor;

    // Inyección de dependencia
    public Auto(IMotor motor)
    {
        this.motor = motor;
    }

    public void Iniciar()
    {
        motor.Encender();
    }
}
```

**BIEN**

Auto no conoce los detalles del motor.
Fácil de extender (nuevo tipo de motor → no se toca la clase Auto).
Facilita pruebas unitarias (puedes inyectar motores falsos).

```
public interface IMotor
{
    void Encender();
}

public class MotorGasolina : IMotor
{
    public void Encender() => Console.WriteLine("Motor de gasolina encendido");
}

public class MotorElectrico : IMotor
{
    public void Encender() => Console.WriteLine("Motor eléctrico encendido");
}

public class Auto
{
    private readonly IMotor motor;

    // Inyección de dependencia
    public Auto(IMotor motor)
    {
        this.motor = motor;
    }

    public void Iniciar()
    {
        motor.Encender();
    }
}
```

Uso:

```
var auto1 = new Auto(new MotorGasolina());
auto1.Iniciar(); // Motor de gasolina encendido

var auto2 = new Auto(new MotorElectrico());
auto2.Iniciar(); // Motor eléctrico encendido
```
