# Estilo arquitectónico

## Arquitectura general del sistema

El sistema sigue un **estilo arquitectónico de monolito modular**, organizado en capas:

- **Capa de presentación**
- **Capa de lógica de negocio**
- **Capa de datos**

Además, se integra con servicios externos como pasarela de pagos y servicio de envíos.

## Diagrama de arquitectura

```mermaid
flowchart TD

    %% Actores
    C[Cliente]
    S[Seller]
    A[Administrador]

    C --> WEB[Cliente Web\nHTML / CSS / JavaScript]
    S --> WEB
    A --> WEB

    WEB --> API[HTTPS / JSON\n/api/v1/*]

    %% Backend
    subgraph MONO[Marketplace Backend - Monolito Node.js 20 LTS + Express]
        direction TB

        MW[Middlewares Express\ncors - express.json() - auth JWT - validación - manejo de errores - logger]

        API --> MW

        %% Presentación
        subgraph PRESENTACION[Capa de Presentación]
            direction LR

            subgraph MODU[Modulo Usuarios]
                U1[usuarios.routes.js]
                U2[usuarios.controller.js]
                U1 --> U2
            end

            subgraph MODS[Modulo Sellers]
                S1[sellers.routes.js]
                S2[sellers.controller.js]
                S1 --> S2
            end

            subgraph MODC[Modulo Catálogo]
                C1[catalogo.routes.js]
                C2[catalogo.controller.js]
                C1 --> C2
            end

            subgraph MODCA[Modulo Carrito]
                CA1[carrito.routes.js]
                CA2[carrito.controller.js]
                CA1 --> CA2
            end

            subgraph MODP[Modulo Pedidos]
                P1[pedidos.routes.js]
                P2[pedidos.controller.js]
                P1 --> P2
            end
        end

        MW --> U1
        MW --> S1
        MW --> C1
        MW --> CA1
        MW --> P1

        %% Lógica de negocio
        subgraph NEGOCIO[Capa de Lógica de Negocio]
            direction LR
            US[usuarios.service.js\nregistro, login, roles]
            SS[sellers.service.js\nalta de tiendas, validación]
            CS[catalogo.service.js\nproductos, categorías, stock]
            CAS[carrito.service.js\nitems, totales]
            PS[pedidos.service.js\ncheckout, estados, seguimiento]
        end

        U2 --> US
        S2 --> SS
        C2 --> CS
        CA2 --> CAS
        P2 --> PS

        %% Comunicación entre servicios
        US -.-> PS
        SS -.-> PS
        CS -.-> CAS
        CAS -.-> PS

        %% Datos
        subgraph DATOS[Capa de Datos]
            direction LR
            UR[usuarios.repository.js]
            SR[sellers.repository.js]
            CR[catalogo.repository.js]
            CAR[carrito.repository.js]
            PR[pedidos.repository.js]
        end

        US --> UR
        SS --> SR
        CS --> CR
        CAS --> CAR
        PS --> PR

        DBA[Acceso a datos compartido\nSequelize ORM - modelos - pool de conexiones]

        UR --> DBA
        SR --> DBA
        CR --> DBA
        CAR --> DBA
        PR --> DBA
    end

    %% Base de datos
    DBA --> DB[(PostgreSQL\nmarketplace_db)]

    %% Sistemas externos
    PS --> PAY[Pasarela de pagos\nCulqi / Nubiz]
    PS --> SHIP[Servicio de envíos\nAPI del courier]
