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

#### Servicios de dominio

* Agrupación de lógica de negocio que podremos reutilizar desde múltiples servicios de Aplicación

Ejemplo:

Tenemos dos casos de uso en nuestra aplicación:

1. Obtener un vídeo en base a su identificador
2. Modificar el título de un determinado vídeo

En ambos casos de uso, necesitaremos la lógica de negocio para:

* Ir al repositorio de vídeos a buscar un vídeo determinado en base a su identificador
* Lanzar una excepción de dominio tipo VideoNotFound en caso de no encontrar el vídeo. Importante destacar que quien lanza esta excepción, como comentamos en el vídeo, no es la implementación del repositorio.
* Retornar el vídeo en caso de encontrarlo

Para evitar duplicar esta lógica de negocio en los dos servicios de Aplicación lo que solemos hacer es **extraerla a un Servicio de Dominio** que invocaremos desde ambos casos de uso.

Los servicios de dominio **en ningún caso** **publicarán** los eventos de dominio que se puedan producir ni **gestinonarán transacciones**. 

#### Value Objects

Los Value Objects (VO) u "Objetos de Valor", no son más que clases que se identifican por el valor que representan. Ejemplo: la URL de los vídeos. Si cambia su valor, ya no representará el mismo concepto. Por esto decimos que se identifican por el valor que contienen.

```java
final class VideoUrl extends StringValueObject
{
    public function __construct(string $value)
    {
        $this->guardValidUrl($value); // ℹ️ Validación en el momento de instanciación. No permitimos tener un VideoUrl con un null por ejemplo.

        parent::__construct($value);
    }

    private function guardValidUrl(string $url)
    {
        if (false === filter_var($url, FILTER_VALIDATE_URL)) {
            throw new \InvalidArgumentException(sprintf('The url <%s> is not well formatted', $url));
        }
    }
}
```

* **Semántica de dominio**: Tipos con conceptos de dominio como VideoUrl, UserRange, Rating, etc. nos ayudará a que el código sea más legible y a que nuestro código exprese mejor los conceptos de nuestro dominio.

* **Cohesión**
  * Evitar comprobaciones redundantes
  * Imán de lógica

### 2.Application

* Casos de uso de nuestra aplicación: registrar usuario, publicar producto, añadir producto al carrito,...

#### Servicios de aplicación

* Son los puntos de entrada a nuestra aplicación.
* Representan de forma atómica un caso de uso de nuestro sistema. En caso de modificaciones del estado de nuestra aplicación:
  * Podrán hacer las de **barrera transaccional** con el sistema de persistencia.
  * Publicarán los **eventos de dominio** respectivos.
* Coordinan las llamadas a los distintos elementos de nuestro sistema para ejecutar un determinado caso de uso.
* Les llamaremos indistintamente Servicio de Aplicación como **Caso de Uso**.

> Pro tip: Inyecta el publisher de eventos en el caso de uso

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