---
title: "Test Types"
date: 2020-04-13T18:58:03+02:00
weight: 2
---

* Claros, concisos y certeros: Arrange - Act - Assert / Given - When - Then. Datos mínimos relevantes para entender y distinguir cada escenario.
* Feedback rápido e informativo
* Simplicidad: Dependencias - librerías, frameworks - ¿cuánto cuesta entender como funcionan todas estas piezas?
* Robustez: Cuando un test se **rompe debería hacerlo por un solo motivo**. Debería ser muy expresivo, señalando el motivo del fallo como para que no haga falta depurar el sistema.
* Flexibilidad: Refactoring
* Isolation: El resultado de un test no debería estar sujeto a la ejecución de otro test, deberían ser independientes a la hora de ejecutarse.

## Unit

## Integration

## Acceptance

Acceptance testing is the process of verifying that stories were developed such that each works exactly the way the customer team expected it to work. They represent the **expectations** of a project's users.

Having basic criteria that tell us when something is done is the best way to avoid putting too much, or too little, time and effort into it. 

> The acceptance tests need to be specified by the customer ->  Minimally the customer needs to specify the tests that will be used to know when a story has been correctly developed.

#### Write early

More of the customer team’s assumptions and expectations are communicated earlier to the developers.

Tests should be written as early in an iteration as possible. Tests are generally written at the following times:

* Whenever the customer and developers talk about the story and want to capture explicit details
* As part of a dedicated effort at the start of an iteration but before programming begins
* Whenever new tests are discovered during or after the programming of the story
  
#### Questions from customer

* What else do the programmers need to know about this story?
* What am I assuming about how this story will be implemented?
* Are there circumstances when this story may behave differently?
* What can go wrong during the story?

#### Example

User story "A user can pay for the items in her shopping cart with a credit card":

* Test with Visa, MasterCard and American Express (pass).
* Test with Diner’s Club (fail).
* Test with a Visa debit card (pass).
* Test with good, bad and missing card ID numbers from the back of the card.
* Test with expired cards.
* Test with different purchase amounts (including one over the card’s limit).

## End to end

* Comparación con unit test
  * `+` granularidad
  * `-` velocidad
  * `+` complejidad
  * `+` fragilidad
  * `-` acoplamiento a la implementación del sistema
  * `+` requerimiento de infraestructura para poder ser inocuo
  * `+` dependencia del entorno de ejecución del sistema 

## Mutation

Consiste en introducir pequeños cambios en el código de producción a modo de defectos, por ejemplo, invertir una condición lógica, y forzar así un fallo en los tests.

## Exploratory

Explorar la aplicación para descubrir cómo romperla

* Pair testing: Explorar el software a pares. Si una de las personas tiene más habilidad explorando y la otra escribiendo test automáticos, pueden aprovechar para ir automatizando las pruebas que están sin cubrir.
* Mob testing: Explorar el software en grupo. 

## Property based

Ejecutar miles de casos que ponen a prueba el código de forma que un humano escribiendo test automáticos no haría.

// TODO Karumi -> MaxibonKata -> Java y JUnit-QuickCheck

## Snapshot

# Testing pyramid

> Few e2e tests. A lot unit tests

