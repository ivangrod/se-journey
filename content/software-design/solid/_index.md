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

**Concepto**:
* Si S es un subtipo de T, instancias de T deberían poderse sustituir por instancias de S sin alterar las propiedades del programa
* Es decir, al tener una jerarquía nos supone que estamos estableciendo un contrato en el padre, por lo que, garantizar que se mantiene dicho contrato en el hijo, nos permitirá que podamos sustituir al padre y la aplicación seguirá funcionando perfectamente.

**Cómo**:
* El comportamiento de las subclases debe respetar el contrato establecido en la superclase.

**Finalidad**:
* Mantener correctitud funcional para poder aplicar OCP

```php
final class UserRepositoryMySql extends Repository implements UserRepository
{
    public function save(User $user): void{
        $this->entityManager()->persist($user);
    }

    public function flush(User $user)
    {
        $this->entityManager()->flush($user);
    }

    public function saveAll(Users $users)
    {
        each($this->persister(),$users);
    }
}
```

Nuestra UserRepositoryMySql es una implementación de un repositorio MySql, pero ojo! con la característica de utilizar Doctrine como ORM. Doctrine implementa el *Unit of work pattern*, que nos ofrece algunas características:

* Utiliza una 'caché' en memoria (unit of work) donde almacena inicialmente los datos antes de persistir en BD, haciendo más rápida su recuperación durante ese proceso.
* Cuando editamos estos objetos, compara las diferencias con el estado almacenado para saber él mismo qué atributos deben actualizarse.

### Violación del LSP

Como clientes externos que sólo conocemos la interface publicada, podríamos esperar que cuando se llamase al método save, con nuestra implementación se persistieran la información en BD.

```php
interface UserRepository
{
    public function save(User $user): void;
    
    public function saveAll(Users $users): void;
    
    public function search(UserId $id): ?User;
    
    public function all(): Users;
}
```

Sin embargo nuestra implementación del método **save** internamente llama a **persist**, que simplemente almacenará los datos en la *unit of work* sin forzar la persistencia real en BD. Como vemos, aunque se ha mantenido la firma de los métodos definidos en la interface, estaríamos violando el LSP puesto que no podríamos utilizarla para reemplazar otras implementaciones que no utilizan el *Unit of work pattern* y si estarían persistiendo en su BD al llamar al método **save**.

## ISP - Interface segregation principle

**Concepto**:
* Ningún cliente debería verse forzado a depender de métodos que no usa

**Cómo**:
* Definir contratos de interfaces basándonos en los clientes que las usan y no en las implementaciones que pudiéramos tener (**Las interfaces pertenecen a los clientes**)
* Evitar [Header Interfaces](https://martinfowler.com/bliki/HeaderInterface.html) promoviendo [Role Interfaces](https://martinfowler.com/bliki/RoleInterface.html)

**Finalidad**:
* Alta cohesión y bajo acoplamiento estructural


En el ejemplo anterior, con el patrón Unit of work necesitábamos dos métodos save y flush para persistir en BD.

```php
interface UserRepository
{
    public function save(User $user): void;
    
    public function flush(User $user): void;

    public function saveAll(Users $users): void;
    
    public function search(UserId $id): ?User;
    
    public function all(): Users;
}
```

De este modo, nuestra interface UserRepository tendría que tener el aspecto que vemos arriba, haciendo **Header Interface** hemos definido la cabecera de los métodos *save* y *flush* en nuestra interface.

```php
public function __invoke(UserId $id)
{
    $user = $this->finder->__invoke($id);
       
    $user->increaseTotalVideosCreated();
       
    $this->repository->save($user);
    $this->repository->flush($user);
}
```

Si quisiéramos implementar ciertas bases de datos como Redis, que no contemplan el patrón *Unit of work*, nos estaría sobrando la llamada al método **flush**.

Nuestro caso de uso no tiene por qué conocer qué hace la implementación por detrás, de modo que simplemente llamaría al método **save**

```php
final class UserRepositoryMySql extends Repository implements UserRepository
{
    public function save(User $user): void
    {
        $this->entityManager()->persist($user);
        $this->entityManager()->flush($user);
    }

    public function saveAll(Users $users)
    {
        each($this->persister(),$users);
    }
}
```

Aunque el código inicial podríamos pensar que estaba desacoplado de la implementación gracias a la interface, si que estaba **acoplado estructuralmente**:

> Desde nuestro caso de uso sabíamos que debíamos de llamar primero al método save y después al método flush

## DIP - Dependency inversion principle

**Concepto**:
* Módulos de alto nivel no deberían depender de los de bajo nivel. Ambos deberían depender de abstracciones

**Cómo**:
* Inyectar dependencias (parámetros recibidos en constructor)
* Depender de las interfaces (contratos) de estas dependencias y no de implementaciones concretas
* LSP como premisa

**Finalidad**:
* Facilitar la modificación y substitución de implementaciones
* Mejor testabilidad de clases

### Violación de DIP

```java
final class UserSearcher {
    private HardcodedInMemoryUsersRepository usersRepository = new HardcodedInMemoryUsersRepository();

    public Optional<User> search(Integer id) {
        return usersRepository.search(id);
    }
}
```

```java
final class HardcodedInMemoryUsersRepository {
    private Map<Integer, User> users = Collections.unmodifiableMap(new HashMap<Integer, User>() {
        {
            put(1, new User(1, "Rafa"));
            put(2, new User(2, "Javi"));
        }
    });

    public Optional<User> search(Integer id) {
        return Optional.ofNullable(users.get(id));
    }
}
```

Desde el propio Test ya se observa este acoplamiento, obligando a saber, en este caso, que el usuario tiene que existir en el HashMap (caso de *find_existing_users*) o que no va a existir un usuario con un id concreto (caso de *not_find_non_existing_users*).

```java
final class UserSearcherShould {
    @Test
    void find_existing_users() {
        UserSearcher userSearcher = new UserSearcher();

        Integer existingUserId = 1;
        Optional<User> expectedUser = Optional.of(new User(1, "Rafa"));

        assertEquals(expectedUser, userSearcher.search(existingUserId));
    }

    @Test
    void not_find_non_existing_users() {
        UserSearcher userSearcher = new UserSearcher();

        // We would be coupled to the actual HardcodedInMemoryUsersRepository implementation.
        // We don't have the option to set test users as we would have to do if we had a real database repository.
        Integer nonExistingUserId = 5;
        Optional<User> expectedEmptyResult = Optional.empty();

        assertEquals(expectedEmptyResult, userSearcher.search(nonExistingUserId));
    }
}
```

### Refactor - Inyección de dependencias

```java
final class UserSearcher {
    private HardcodedInMemoryUsersRepository usersRepository;

    public UserSearcher(HardcodedInMemoryUsersRepository usersRepository) {
        this.usersRepository = usersRepository;
    }

    public Optional<User> search(Integer id) {
        return usersRepository.search(id);
    }
}
```

El punto de nuestro aplicación que instancie a nuestro UserSearcher será el responsable de saber cómo debe hacerlo y que otras dependencias puede haber detrás, es decir, conseguir exponer el acoplamiento de las clases.

```java
final class UserSearcherShould {
    @Test
    void find_existing_users() {
        // Now we're injecting the HardcodedInMemoryUsersRepository instance through the UserSearcher constructor.
        // Win: We've moved away from the UserSearcher the instantiation logic of the HardcodedInMemoryUsersRepository class allowing us to centralize it.
        // Win: We're exposing the couplings of the UserSearcher class.
        HardcodedInMemoryUsersRepository usersRepository = new HardcodedInMemoryUsersRepository();
        UserSearcher userSearcher = new UserSearcher(usersRepository);

        Integer existingUserId = 1;
        Optional<User> expectedUser = Optional.of(new User(1, "Rafa"));

        assertEquals(expectedUser, userSearcher.search(existingUserId));
    }

    @Test
    void not_find_non_existing_users() {
        HardcodedInMemoryUsersRepository usersRepository = new HardcodedInMemoryUsersRepository();
        UserSearcher userSearcher = new UserSearcher(usersRepository);

        Integer nonExistingUserId = 5;
        Optional<User> expectedEmptyResult = Optional.empty();

        assertEquals(expectedEmptyResult, userSearcher.search(nonExistingUserId));
    }
}
```

### Refactor - Inyección de dependencias de parámetros

```java
final class HardcodedInMemoryUsersRepository {
    private Map<Integer, User> users;

    public HardcodedInMemoryUsersRepository(Map<Integer, User> users) {
        this.users = users;
    }

    public Optional<User> search(Integer id) {
        return Optional.ofNullable(users.get(id));
    }
}
```

Conseguimos aislar nuestros Test sin que **dependan de la infraestructura**

```java
final class UserSearcherShould {
    @Test
    void find_existing_users() {
        // Now we're also injecting the constant parameters needed by the HardcodedInMemoryUsersRepository through its constructor.
        // Win: We can send different parameters depending on the environment.
        // That is, in our production environment we would have actual users,
        // while in our tests cases we will set only the needed ones to run our test cases
        int rafaId = 1;
        User rafa = new User(rafaId, "Rafa");

        Map<Integer, User> users = Collections.unmodifiableMap(new HashMap<Integer, User>() {
            {
                put(rafaId, rafa);
            }
        });
        HardcodedInMemoryUsersRepository usersRepository = new HardcodedInMemoryUsersRepository(users);
        UserSearcher userSearcher = new UserSearcher(usersRepository);

        Optional<User> expectedUser = Optional.of(rafa);

        assertEquals(expectedUser, userSearcher.search(rafaId));
    }

    @Test
    void not_find_non_existing_users() {
        Map<Integer, User> users = Collections.emptyMap();
        HardcodedInMemoryUsersRepository usersRepository = new HardcodedInMemoryUsersRepository(users);
        UserSearcher userSearcher = new UserSearcher(usersRepository);

        // Win: Now we don't have to be coupled of the actual HardcodedInMemoryUsersRepository users.
        // We can send a random user ID in order to force an empty result because we've set an empty map as the system users.
        Integer nonExistingUserId = 1;
        Optional<User> expectedEmptyResult = Optional.empty();

        assertEquals(expectedEmptyResult, userSearcher.search(nonExistingUserId));
    }
}
```

### Inversión de dependencias

```java
final class UserSearcher {
    private UsersRepository usersRepository;

    public UserSearcher(UsersRepository usersRepository) {
        this.usersRepository = usersRepository;
    }

    public Optional<User> search(Integer id) {
        return usersRepository.search(id);
    }
}
```

```java
public interface UsersRepository {
    Optional<User> search(Integer id);
}
```

```java
final class UserSearcherShould {
    @Test
    void find_existing_users() {
        // Now we're injecting to the UserSearcher use case different implementation of the new UserRepository interface.
        // Win: We can replace the actual implementation of the UsersRepository used by the UserSearcher.
        // That is, we'll not have to modify a single line of the UserSearcher class despite of changing our whole infrastructure.
        // This is a big win in terms of being more tolerant to changes.
        // Win: It also make it easier for us to test the UserSearcher without using the actual implementation of the repository used in production.
        // This is another big win because this way we can have test such as the following ones which doesn't actually go to the database in order to retrieve the system users.
        // This has a huge impact in terms of the time to wait until all of our test suite is being executed (quicker feedback loop for developers).
        // Win: We can reuse the test environment repository using test doubles. See CodelyTvStaffUsersRepository for its particularities
        UsersRepository codelyTvStaffUsersRepository = new CodelyTvStaffUsersRepository();
        UserSearcher userSearcher = new UserSearcher(codelyTvStaffUsersRepository);

        Optional<User> expectedUser = Optional.of(UserMother.rafa());

        assertEquals(expectedUser, userSearcher.search(UserMother.RAFA_ID));
    }

    @Test
    void not_find_non_existing_users() {
        // Win: Our test are far more readable because they doesn't have to deal with the internal implementation of the UserRepository.
        // The test is 100% focused on orchestrating the Arrange/Act/Assert or Given/When/Then flow.
        UsersRepository emptyUsersRepository = new EmptyUsersRepository();
        UserSearcher userSearcher = new UserSearcher(emptyUsersRepository);

        Integer nonExistingUserId = 1;
        Optional<User> expectedEmptyResult = Optional.empty();

        assertEquals(expectedEmptyResult, userSearcher.search(nonExistingUserId));
    }
}
```

> **Consejo**: Una forma de ver si estamos violando DIP es comprobar nuestras clases del *Servicio de aplicación* si alguna de las dependencias está apuntando fuera de nuestro *Dominio*

> [Specification pattern](https://github.com/iluwatar/java-design-patterns/tree/master/specification) (a.k.a. Criteria)