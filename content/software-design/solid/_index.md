+++
title = "SOLID Principles"
date = 2020-07-28T17:49:11+02:00
weight = 1
+++

## SRP - Single responsibility principle

**Concepto**:
* Una clase = Un concepto y responsabilidad
* Una clase debería tener sólo 1 razón para cambiar

**Cómo conseguirlo**:
* Clases pequeñas con objetivos acotados
  * [Code Smell] En clases de servicio, si presentan más de un método público, se interpreta que tienen más de una responsabilidad (hacen más de una *cosa*)
  * [Code Smell] Nombres de clases abstractos, ej. EmailSender (1) vs EmailService (1..n). La terminología más específica no abre la puerta a añadir más funcionalidades

**Finalidad**:
* Alta cohesión y robustez
* Permitir composición de clases (inyectar colaboradores)
* Evitar duplicidad de código

Modelo de dominio *Book*:

```java
final class Book
{
    public String getTitle()
    {
        return "A great book";
    }
    public String getAuthor()
    {
        return "John Doe";
    }
    public void printCurrentPage()
    { 
        System.out.println("current page content");
    }
}
```

Servicio cliente del modelo de dominio:

```java
final class Client
{
    public Client() {
        Book book = new Book(…);
        book.printCurrentPage();
    }
}
```

**Motivo del por qué no respetamos SRP**: Book está acoplada al canal estándar de salida al imprimir la página actual. Sabe cómo modelar los datos y cómo imprimirlos.

```java
final class Book
{
    public String getTitle()
    {
        return "A great book";
    }
    public String getAuthor()
    {
        return "John Doe";
    }
    public String getCurrentPage()
    {
        return "current page content";
    }
}
```

```java
final class StandardOutputPrinter
{
    public void printPage(String page)
    {
        System.out.println(page);
    }
}
```

```java
final class Client
{
    public Client() {
        Book book = new Book(…);
        String currentPage = book.getCurrentPage();
        StandardOutputPrinter printer = new StandardOutputPrinter();
        printer.printPage(currentPage);
    }
}
```

Aplicando **modularidad**

```java
interface Printer
{
    public void printPage(String page);
}
```

```java
final class StandardOutputPrinter implements Printer
{
    public void printPage(String page)
    {
        System.out.println(page);
    }
}
```

```java
final class StandardOutputHtmlPrinter implements Printer
{
    public void printPage(String page)
    {
        System.out.println("<div>" + page + "</div>");
    }
}
```

> Modelo de dominio anémico <-> DTOs


## OCP - Open/closed principle

**Concepto**:
* El software debería estar abierto a extensión y cerrado a modificación.
* Ésto aplica tanto a nuestras clases internas, servicios, microservicios, casos de usos, etc.

**Cómo conseguirlo**:
* Evitando depender de implementaciones específicas, haciendo uso de clases abstractas o interfaces.

**Finalidad**:
* Facilidad para añadir nuevos Casos de uso en nuestra aplicación.

### Violación OCP

```java
final class Song {
  private Double totalLength;
  private Double sentLength;

  public Double getSentLengthPercentage() {
    return sentLength * 100 / totalLength;
  }
}
```

```java
final class File {
  private Double totalLength;
  private Double sentLength;

  public Double getSentLengthPercentage() {
    return sentLength * 100 / totalLength;
  }
}
```

### OCP - Interface

```java
interface Measurable {
  public Double getTotalLength();
  public Double getSentLength();
}
```

```java
final class Song implements Measurable {
    private Double totalLength;
    private Double sentLength;
    
    @Override
    public Double getTotalLength() {
        return totalLength;
    }
    
    @Override
    public Double getSentLength() {
        return sentLength;
    }
}
```
Clase *Progress* acoplada únicamente a la interface

```java
final class Progress {
    public Double getSentLengthPercentage(Measurable measurable) {
        return measurable.getSentLength() * 100 / measurable.getTotalLength();
    }
}
```

### OCP - Abstract class 

```java
abstract class Measurable {
    abstract Double getTotalLength();
    abstract Double getSentLength();
    
    public Double getSentLengthPercentage() {
        return getSentLength() * 100 / getTotalLength();
    }
}
```

```java
final class Song extends Measurable {
    @Override
    public Double getTotalLength() {
        // ...
    }

    @Override
    public Double getSentLength() {
        // ...
    } 
}
```

```java
final class Progress {
    public Double getSentLengthPercentage(Measurable measurable) {
        // Nos llevamos lógica a nuestro modelo de dominio
        return measurable.getSentLengthPercentage();
    }
}
```

### Interface - Abstract class

* Beneficios de Interface - Usar para **desacoplar entre capas**:
  * No modifica el árbol de jerarquía
  * Permite implementar N Interfaces

* Beneficios de Abstract Class - Determinados casos para **Modelos de dominios**:
  * Permite desarrollar el patrón [Template Method](https://github.com/iluwatar/java-design-patterns/tree/master/template-method) empujando la lógica al modelo
    * Problema: Dificultad de trazar
  * Getters privados (Tell don’t ask)

## LSP - Liskov substitution principle

## ISP - Interface segregation principle

## DIP - Dependency inversion principle

