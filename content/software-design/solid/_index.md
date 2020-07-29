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

## LSP - Liskov substitution principle

## ISP - Interface segregation principle

## DIP - Dependency inversion principle

