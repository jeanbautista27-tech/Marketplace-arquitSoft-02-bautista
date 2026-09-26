# Arquitectura inicial del sistema

## Ejercicio 09: Diseñar la arquitectura en capas

```mermaid
flowchart TD
    P["PRESENTACIÓN | Web, API e interfaz"]
    L["LÓGICA DE NEGOCIO | Catálogo, carrito, pedidos, sellers y usuarios"]
    D["DATOS | Base de datos"]

    P --> L
    L --> D
```

| Capa | Pregunta que responde |
|---|---|
| Presentación | ¿Cómo interactúa el usuario? |
| Lógica de negocio | ¿Qué hace el sistema? |
| Datos | ¿Dónde se almacena la información? |