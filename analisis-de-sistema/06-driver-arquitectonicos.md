# Drivers arquitectónicos

| ID | Driver arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
|---|---|---|---|
| DA01 | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales. | AC03 - Escalabilidad | Influye en la estrategia de escalamiento y despliegue. |
| DA02 | El sistema debe mantener tiempos de respuesta adecuados cuando haya muchos usuarios conectados simultáneamente. | AC01 - Rendimiento | Influye en la comunicación entre componentes, el procesamiento y el almacenamiento. |
| DA03 | El sistema debe proteger los datos de los usuarios y las operaciones de compra. | AC04 - Seguridad | Influye en la autenticación, la autorización y la protección de datos. |
| DA04 | El sistema debe integrarse con una pasarela de pago externa mediante una API. | RC04 - Pasarela de pago | Condiciona la comunicación e integración con servicios externos. |
| DA05 | El sistema debe utilizar una API REST para la comunicación entre la aplicación web y el backend. | RC03 - API REST | Limita las alternativas de comunicación entre las partes del sistema. |