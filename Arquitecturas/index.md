---
title: Arquitecturas
layout: home
nav_order: 23
---

# Clean Architecture


## Dominio 

Es donde vive la lógica de negocio pura, libre de frameworks, bases de datos o UI.

```
/Domain
│
├── Entities/
│   ├── CuentaBancaria.cs
│   ├── Cliente.cs
│   └── Transaccion.cs
│
├── ValueObjects/
│   ├── Dinero.cs
│   └── Email.cs
│
├── Events/
│   ├── TransaccionRealizadaEvent.cs
│
├── Services/
│   ├── ServicioTransferencia.cs
│
├── Interfaces/
│   ├── ICuentaBancariaRepository.cs
│
└── Exceptions/
    ├── SaldoInsuficienteException.cs
```

