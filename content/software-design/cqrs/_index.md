+++
title = "Command Query Responsibility Segregation"
date = 2020-08-20T17:49:11+02:00
weight = 1
+++

## Command

En CQRS, un **Command** representa la intención de realizar una operación en nuestro sistema que acabe modificando el estado de tal.

> Command -> DTO | Inmutable | Void | Side-effects

## Query

En CQRS, una **Query** representa la intención de pedir unos datos a nuestro sistema sin que ello acabe alterando el estado de tal.

> Query -> DTO | Inmutable | <T> | No side-effects (+ caché)

## Command/Query Bus

![CQRS - Axon diagram](/se-journey/img/cqrs_axon.jpg)

* Controlador desacoplado de la lógica de negocio.
* El Handler es el componente encargado de mapear los valores de entrada a objetos de dominio (Value Objects) para que el caso de uso los reciba.
* (+) **Rendimiento**. Podemos introducir asincronía aprovechándonos del nivel de indirección introducido entre el entrypoint y la lógica a ejecutar.
* Decorator -> Handlers / Use cases. Utilizar middlewares para ejecutar lógica transversal.

## CommandHandler vs QueryHandler

> QueryHandler ahora sí debe tener un retorno

QueryHandler será el encargado de llamar al Converter que mapea una entidad de dominio a una instancia de tipo Response (**Projection**).

Esta proyección solo contiene tipos primitivos -> Bus sólo debería admitir datos en plano.

### Outside > Identificadores de recursos 

Posibles responsables para generar el identificador:

* MySqlVideoRepository
  * (X) Delegamos la responsabilidad de gestionar los identificadores a un componente de infraestructura.
  * (X) Representaciones de recursos inconsistentes en cliente
  * (X) Complejidad adicional en testing. Generación de identificadores no es determinista.

* VideoCreator
  * (\/) Podemos cambiar la infraestructura manteniendo la lógica de gestión de identificadores.
  * (X) Representaciones de recursos inconsistentes en cliente
  * (X) Complejidad adicional en testing. Generación de identificadores no es determinista.

* Cliente (app, JS del frontend, etc.)
  * (\/) Mantenimiento y cambiabilidad: Podemos cambiar cualquier componente de infraestructura
  * (\/) Testing
  * (\/) Integridad de representación de recursos en cliente
  
> OJO: No utilizar desde cliente identificadores secuenciales -> Podríamos generar **condiciones de carrera**  

[UUID or GUID as Primary Keys](https://tomharrisonjr.com/uuid-or-guid-as-primary-keys-be-careful-7b2aa3dcb439)

