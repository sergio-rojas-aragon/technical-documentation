---
title: React JS
layout: home
nav_order: 19
---

# React
{: .no_toc }

Apuntes que tomo cuando desarrollo en React para no olvidar los comandos aplicados.
{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Store

* auth → es el nombre del trozo de estado
* authReducer → describe cómo cambia ese estado

```Typescript
export const store = configureStore({
    reducer: {
        auth: authReducer
    }
});
```
El store tiene dos capacidades principales:

* dispatch() = Envia acciones para cambiar estado.
* getState() = Devuelve el estado completo.

## Slice

Un slice es una “sección” del estado + sus reglas.
* una parte del estado (ej: auth)
* sus acciones sincrónicas (ej: setToken)
* su reducer (cómo responder cuando se dispara una acción)

Ejemplo:

```Typescript
interface AuthState {
  token: string | null;
}
```

```Typescript
reducers: {
  setToken: (state, action) => {
    state.token = action.payload;
  }
}
```

Es código de negocio SIN asincronía. No llama APIs.
Solo cambia el estado inmediatamente


## ACTIONS 

Eventos que describen cambios en el estado Una acción es un objeto que dice QUÉ pasó.
Ejemplo de lo que dispara setToken:

```Typescript
{
  type: "auth/setToken",
  payload: "abc123"
}
```

El reducer decide cómo cambia el estado con esa acción.



## Flujo Redux Toolkit

```
           ┌──────────────────────────────┐
           │        COMPONENTE UI         │
           │  onClick / onSubmit / etc    │
           └───────────────┬──────────────┘
                           │ dispatch(thunk)
                           ▼
           ┌──────────────────────────────┐
           │            THUNK             │
           │  lógica async (login, etc)   │
           └───────────────┬──────────────┘
                           │ llama service
                           ▼
           ┌──────────────────────────────┐
           │           SERVICE            │
           │   axios / fetch / backend    │
           └───────────────┬──────────────┘
                           │ responde
                           ▼
           ┌──────────────────────────────┐
           │        THUNK (return)        │
           └───────────────┬──────────────┘
                           │ fulfilled / rejected
                           ▼
           ┌──────────────────────────────┐
           │            SLICE             │
           │ reducers cambian el estado   │
           └───────────────┬──────────────┘
                           │ actualiza store
                           ▼
           ┌──────────────────────────────┐
           │            STORE             │
           │   estado global actualizado  │
           └───────────────┬──────────────┘
                           │ useSelector()
                           ▼
           ┌──────────────────────────────┐
           │        COMPONENTE UI         │
           │   se re-renderiza automáticamente   │
           └──────────────────────────────┘
```

# Snipets y otros

Ctrl + k + F identar automaticamente