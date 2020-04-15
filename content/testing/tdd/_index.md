---
title: "Test-Driven Development"
date: 2020-04-13T18:55:59+02:00
weight: 1
---

## Steps

1. Quickly add a test.
2. Run all tests and see the new one fail.
3. Make a little change.
4. Run all tests and see them all succeed.
5. Refactor to remove duplication.
   
   The problem is the dependency between the code and the test—you can't change one without changing the other. Our goal is to be able to write another test that “makes sense” to us, without having to change the code.

   If **dependency** is the problem, **duplication** is the symptom.

   _By eliminating duplication before we go on to the next test, we maximize our chance of being able to get the next test running with one and only one change._

### Points of interest

* How each test can cover a small increment of functionality
* How small and ugly the changes can be to make the new tests run
* How often the tests are run
* How many teensy-weensy steps make up the refactorings

## Beginning

* We'll make a to-do list to remind us what we need to do, to keep us focused and to tell us when we are finished.

> We don't start with objects, we start with tests.

* Told a story with a snippet of code about how we wanted to view one operation

* Ignored the details of JUnit for the moment

* Made the test compile with stubs

* Made the test run by committing horrible sins

* Gradually generalized the working code, replacing constants with variables

* Added items to our to-do list rather than addressing them all at once

# TDD Patterns

## Three Laws of TDD

1. **Law 1**: You can’t write any production code until you have first written a failing spec.
   
2. **Law 2**: You can’t write more of a unit test than is sufficient to fail, and not compiling is failing.
   
3. **Law 3**: You can’t write more production code than is sufficient to pass the currently failing unit test.

## Rule of Three

**Rule of three** ("Three strikes and you refactor") is a code refactoring rule of thumb to decide when similar pieces of code should be refactored to avoid duplication. It states that two instances of similar code don't require refactoring, but when similar code is used three times, it should be extracted into a new procedure.

## Test-Driven Development Patterns

### TEST (NOUN)

Positive feedback loop. The more stress you feel, the less testing you will do. The less testing you do, the more errors you will make. The more errors you make, the more stress you feel.

### ISOLATED TEST

> Make the tests so fast to run that you can run them myself, and run them often. That way you can catch errors before anyone else sees them
> 
> A huge stack of errors didn't usually mean a huge list of problems

One convenient implication of isolated tests is that the **tests are order independent**.

A second implication of isolated tests is that you have to work, sometimes work hard, to **break your problem** into little orthogonal dimensions

### TEST LIST

### TEST FIRST

### ASSERT FIRST

> "What is the right answer?" || "How am I going to check?"

### TEST DATA

> Use data that makes the tests easy to read and follow.
>
> Don't have a list of ten items as the input data if a list of three items will lead you to the same design and implementation decisions.

### EVIDENT DATA

Include expected and actual results in the test itself, and try to make their relationship apparent.

## Red Bar Patterns

### ONE STEP TEST

Pick a test from the list that will teach you something and that you are **confident you can implement**. Each test should represent one step toward your overall goal.

### STARTER TEST

Start by testing a variant of an operation that doesn't do anything.

> The first question you have to ask with a new operation is, "Where does it belong?" Until you've answered this question, you won't know what to type for the test.

~~If you write a realistic test first, then you will find yourself solving a bunch of problems at once.~~ Beginning with a realistic test will leave you too long without feedback.

### EXPLANATION TEST

Ask for and give explanations in terms of tests.

A companion technique is to start giving explanations in terms of tests: "Here's how it works now. When I have a Foo like this and a Bar like that, then the answer is 76. If I have a Foo like that and a Bar like this, though, I would like the answer to be 67."

### LEARNING TEST

When do you write tests for externally produced software? Before the first time you are going to use a new facility in the package.

Ex.: A project in which Learning Tests were routinely written. When new releases of the package arrived, first the tests were run (and fixed, if necessary). If the tests didn't run, then there was no sense running the application because it certainly wouldn't run.

### ANOTHER TEST

How do you keep a technical discussion from straying off topic? When a tangential idea arises, **add a test to the list** and go back to the topic.

### REGRESSION TEST

When a defect is reported write the smallest possible test that fails and that, once run, will be repaired.

Regression tests for the application give your users a chance to speak concretely to you about what is wrong and what they expect.

### BREAK

> If you know what to type, type the Obvious Implementation. If you don't know what to type, then Fake It. If the right design still isn't clear, then Triangulate. If you still don't know what to type, then you can take that shower.

## Testing Patterns

### CHILD TEST

Write a smaller test case that represents the broken part of the bigger test case. Get the smaller test case running. Reintroduce the larger test case.

### MOCK OBJECT

When you have an object that **relies on an expensive or complicated resource**, you must create a fake version of the resource that answers constants.

### SELF SHUNT

For testing that one object communicates correctly with another, the best way is having the object under test communicate with the test case instead of with the object it expects.

// TODO Review it

### LOG STRING

Test that the sequence in which messages are called is correct -> Keep a log in a string, and append to the string when a message is called.

Log Strings are particularly useful when you are implementing Observer and you expect notifications to come in a certain order.

### CRASH TEST DUMMY

Test error code that is unlikely to be invoked -> Invoke it anyway with a special object that throws an exception instead of doing real work. **Code that isn't tested doesn't work**.

```java
private class FullFile extends File {
   public FullFile(String path) {
	   super(path);
	}

   public boolean createNewFile() throws IOException {
	   throw new IOException();
	}
}

public void testFileSystemError() {
	File f = new FullFile("foo");
   try {
		saveAs(f);
		fail();
	} catch (IOException e) {}
}
```

A Crash Test Dummy is like a Mock Object, except you don't need to mock up the whole object. Java's anonymous inner classes work well for sabotaging just the right method to simulate the error we want to exercise:

```java
public void testFileSystemError() {
   File f= new File("foo") {
      public boolean createNewFile() throws IOException {
         throw new IOException();
      }
   };
   try {
		saveAs(f);
		fail();
	} catch (IOException e) {}
}
```

### BROKEN TEST

Finish a solo session by writing a test case and running it to be sure it doesn't pass. When you come back to the code, you then have an obvious place to start.

You have an obvious, concrete **bookmark** to help you remember what you were thinking.

### CLEAN CHECK-IN

How do you leave a programming session when you're programming in a team? Leave all of the tests running.

You need to start from a place of confidence and certainty.

## Green Bar Patterns

### FAKE IT ('TIL YOU MAKE IT)

Return a constant and gradually replace constants with variables until you have the real code. Remove **duplication** between the test and the production code.

* Having a green bar feels completely different from having a red bar. When the bar is green, you know where you stand. You can refactor from there with confidence.

* Starting with one concrete example and generalizing from there prevents you from prematurely confusing yourself with extraneous concerns.

### TRIANGULATION

We only generalize code (abstract) when we have two examples or more. 

We briefly ignore the duplication between test and model code. When the second example demands a more general solution, then and only then do we generalize.

Once we have the two assertions and we have abstracted the correct implementation for plus, we **can delete** one of the assertions on the grounds that it is completely redundant with the other.

### OBVIOUS IMPLEMENTATION

Type in the real implementation.

> **Advice**
> Commonly shift between two modes of implementation. When everything is going smoothly and you know what to type, you put in **Obvious Implementation** after Obvious Implementation (running the tests each time to ensure that what's obvious to yours is still obvious to the computer). As soon as you get an unexpected red bar, you back up, shift to **Faking** implementations, and refactor to the right code. When you confidence returns, you go back to Obvious Implementations.

### ONE TO MANY

An operation that works with collections of objects -> Implement it without the collections first, then make it work with collections.

### TPP - TRANSFORMATION PRIORITY PREMISE

Cada nuevo test que convertimos a verde debe provocar una transformación en el código de producción que lo haga un poco más genérico de lo que era antes de añadir ese test. Secuencia de transformaciones propuesta por Robert C. Martin:

1. {} –> nil: De no haber código a devolver nulo.
2. nil -> constant: De nulo a devolver un valor literal.
3. constant -> constant+: De un valor literal simple a uno más complejo.
4. constant -> scalar: De un valor literal a una variable.
5. statement -> statements: Añadir más líneas de código sin condicionales.
6. unconditional -> if: Introducir un condicional
7. scalar -> array: De variable simple a colección.
8. array -> container: De colección a contenedor.
9. statement -> recursion: Introducir recursión.
10. if -> while: Convertir condicional en bucle.
11. expression -> function: Reemplazar expresión con llamada a función.
12. variable -> assignment: Mutar el valor de una variable.

## xUnit Patterns

### ASSERTION

Write boolean expressions that automate your judgment about whether the code worked.

> Si para validar un único comportamiento necesitamos varias líneas de asserts, a veces es mejor crear un método propio.
> Otra opción es implementar **Custom Matchers**

Optional first parameter -> Add information about the assertion that will be printed if it ever fails. Some teams adopt the convention that all assertions must be accompanied by an informative error message.

### FIXTURE

Cada test debería ser autosuficiente para crear el conjunto de datos que necesita. Estos datos son los **fixtures**.

How do you create common objects needed by several tests -> Convert the local variables in the tests into instance variables. Override setUp() and initialize those variables.

* Test-fixture-creating code with the test -> Tests written with the setup code right there with the assertions are readable from top to bottom.

* Test-fixture-creating code into a method called setUp() -> Set instance variables to the objects that will be used in the test. We would have to remember that the method was called, and remember what the objects looked like, before we could write the rest of the test.

> In general, if I find myself wanting a **slightly different fixture**, then I **start a new subclass of TestCase**. @Before annotation may help us to evaluate the test class context.

[Fixture Setup Patterns](http://xunitpatterns.com/Fixture%20Setup%20Patterns.html)

### EXTERNAL FIXTURE

How do you release external resources in the fixture? Override tearDown() and release the resources.

### TEST METHOD

Los nombres de los test deberían seguir siendo válidos cuando los pequeños detalles de implementación cambien. No deberían cambiar mientras que las reglas de negocio no cambien

Tip: Usar **snake_case** debido a que los nombres de los test tienden a ser muy largos. Al ser métodos que no van a ser invocados desde ninguna otra parte del código productivo, no supone problema que rompan la convención del lenguaje que se esté utilizando.

### EXCEPTION TEST

How do you test for expected exceptions? Catch expected exceptions and ignore them, failing only if the exception isn't thrown.

[Testing Exceptions (GitHub)](https://github.com/ivangrod/testing-pills/tree/master/testing_exceptions)

```java
public void testMissingRate() {
   try {
	   exchange.findRate("USD", "GBP");
	   fail();
   /* Notice that we are careful only to catch the 
   particular exception we expect, so we will also be 
   notified if the wrong kind of exception is thrown */   
	} catch (IllegalArgumentException expected) {}
}
```

### ALL TESTS

How do you run all tests together? Make a suite of all the suites—one for each package, and one aggregating the package tests for the whole application.

# Best Practice

## Git

* Para descartar los cambios locales en el paso green -> refactor basta con un *git reset* para volver al punto en el que los test pasan.

* Commit en pequeños cambios incrementales. Para que nos formen parte del historial de Git, por algún motivo o por política de equipo, pueden unificarse con *git squash*

// TODO ¿Guía TDD con VCS?

## Legacy code

Al no tener garantías de que el sistema se comporte como uno espera que lo haga, puede ser que plantear test partiendo de premisas incorrectas sea una pérdida de tiempo. Proceso para abordar ésto:

1. Guardar cualquier cambio pendiente que tuvieras hasta ese momento (*git commit*)
2. Añadir la nueva funcionalidad
3. Ejecutar la aplicación a mano para validar que funciona como se espera
4. Explorar la aplicación como tester/usuario para asegurar que ninguna otra funcionalidad está rota
5. **Añadir test automáticos** que den cobertura a la nueva funcionalidad
6. Verlos ejecutarse en verde
7. Volver a dejar el código como estaba al principio (volviendo al commit anterior si es necesario)
8. Lanzar de nuevo los test y verlos en rojo, confirmando que el error es el esperado
9. Recuperar la última versión del código para ver los test en verde de nuevo

## Exceptions

Utilizar assert también para excepciones > código simétrico suele ser más fácil de entender :) :

```java
@Test
public void should_fail_if_the_file_is_empty(){
   assertThatExceptionOfType(IllegalFileException.class)
     .isThrownBy(() -> {
        filter.apply(emptyFile);
     })
}
```

Sentencia declarativa: La expresión se evalúa de dentro hacia fuera. La función *isThownBy* nunca llegará a ejecutarse, porque el test se detendría antes con un rojo.

[Testing Exceptions (GitHub)](https://github.com/ivangrod/testing-pills/tree/master/testing_exceptions)

# Pitfalls

* **Infravalorar el nombre de los test**: Mayor entendimiento del problema y de la solución. Simplificar. Documentación viva y expresiva.
* **Testar estructuras y asignaciones**: Test demasiado acoplados a la implementación del código sin necesidad. No ayudan a implementar ninguna funcionalidad.
* **Falta de refactoring en test**: En un primer momento, no importa si el test tiene diez líneas. Cuando esté en verde, podremos mejorar la legilibilidad del test.
* **Mocks return mocks**: Tests que entorpecen, encarecen el mantenimiento. Pueden ser test de usar y tirar.
* **Uso de variables estáticas/compartidas**: Es recomendable lanzar baterías con las diferentes suites de test en paralelo en máquinasde varios núcleos.
* **Ignorar test en rojo**: No dejarlos ignorados más de un/dos días.
* **Más de una regla por test**: El test debe poner de manifiesto sólo una de las reglas de negocio. Cuando falle, tendremos mejor entendimiento de las consecuencias que puede acarrear.
* **Introducir complejidad ciclomática**: Cualquier cambio que añada indirección o cualquier otra posible complejidad en los test debe hacerse en la fase de refactor.
* **Test parametrizados**: Pocas veces es útil, ya que usando triangulación con dos o tres casos podríamos obtener el mismo resultado. Si fuera necesario, utilizar herramientas de test basados en propiedades.
* **Forzar el diseño para poder probar**: La interfaz publica de un módulo o de una clase es un compromiso adquirido con sus consumidores.
* **Esperas aleatorias para resolver asincronía**: Para que los test inspiren confianza tienen que ser deterministas y además rápidos en la ejecución.
* **Dependencias de plataforma y de máquina**.
* **Ausencia de exploratory testing**: Probar funcionalidades que no hemos probado nosotros.
* **Exceso de test de la GUI**: Tests que atacan a la interfaz gráfica son los más frágiles de todos.
* **Cadenas de transiciones entre estados**: Partir el test en varios, de forma que cada uno se limita a verificar una sola transición de estado. Quizás necesitemos alguna vía de configuración de partida.
* **Ausencia de documentación**.

