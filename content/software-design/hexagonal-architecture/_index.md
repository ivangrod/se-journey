+++
title = "Arquitectura Hexagonal"
date = 2020-08-09T17:49:11+02:00
weight = 1
+++

## Layers

![hexagonal_layers](/se-journey/img/hexagonal_arch.png)

### 1.Domain

* Conceptos que están en nuestro contexto: Usuario, Producto, Carrito,...
* Reglas de negocio que vienen determinadas en exclusiva por nosotros - **Servicios de dominio**

### 2.Application

* Casos de uso de nuestra aplicación: registrar usuario, publicar producto, añadir producto al carrito,...

### 3.Infrastructure

* Código que cambia en función de decisiones externas.
* En esta capa vivirán las implementaciones de las interfaces que definiremos a nivel de dominio. Es decir, nos apoyaremos en el **DIP** de SOLID para poder desacoplarnos de las dependencias externas.

### Regla de dependencia

El código que viva en cada una de nuestras capas sólo deberá tener conocimiento de la capa inmediatamente siguiente. Orden de fuera hacia dentro: Infraestructura -> Aplicación -> Dominio.

Variaciones en elementos de las capas externas no afectan a las internas.

![hexagonal_testing](/se-journey/img/hexagonal_arch_testing.png)

### Ports and Adapters

* **Puertos** Interfaces definidas en la capa de dominio para desacoplarnos de nuestra infraestructura: *UserRepository*
* **Adaptadores** Implementaciones posibles de esos puertos: *MySqlUserRepository*