---
title: "TDD Schools"
date: 2020-04-15T18:40:27+02:00
weight: 1
---

## Inside-out (Classic school) - Algorithms

Enfocada en la verificación del estado de los objetos, siendo por ello imprescindible que el contexto de los test siempre deba estar formado por "objetos reales", configurados previamente.

**Patterns**: Mother Objects, Factory

**Approach**:

We generalise the solution one failing test at a time so we end up with a simple, elegant solution that satisfies all of the tests up to that point. Based on [triangulation]({{< ref "/testing/tdd/_index.md#triangulation" >}})

## Outside-in (London school) - Interactions

Enfocada en verificar que el comportamiento de los objetos es el esperado. Verificar las correctas interacciones entre objetos, y no el estado en sí mismo de los objetos.

Focusing on roles, responsibilities and interactions, as opposed to algorithms. It's born out of the furnace of component-based, distributed and service-oriented applications

**Patterns**: [Test Double]({{< ref "/testing/double-patterns.md" >}})

**Approach**:

Identify the roles, responsibilities and key interactions/collaborations between roles in an end-to-end implementation of the solution to satisfy a system-level scenario or acceptance test. Implement the code needed in each collaborator, one at a time, [faking]({{< ref "/testing/tdd/_index.md##fake-it-til-you-make-it" >}}) it's direct collaborators and then work your way down through the "call stack" of interactions until you have a working end-to-end implementation that passes the front-end test.

## Demo Outside-in TDD

https://www.youtube.com/watch?v=XHnuMjah6ps

https://www.youtube.com/watch?v=gs0rqDdz3ko

https://www.youtube.com/watch?v=R9OAt9AOrzI

**Notes**

* Use verify().methodXXX -> Test over **commands** method (void)
* For command methods you need to test the side effects. After you identify the trigger for them.
  
* You can't test what you can't control it

```java
class Clock{

   public String todayAsString(){
      LocalDate today = today();
      return today.format(DateTimeFormatter.ofPattern("dd/MM/yyyy"));
   }

   // Extract the randomness
   protected LocalDate today(){
      return LocalDate.now();
   }
}
```

```java
class ClockShould{

   @Test
   public void return_todays_date_in_dd_MM_yyyy_format(){
      Clock clock = new TesteableClock();

      ...
   }

   private class TesteableClock extends Clock{

      // Override and control the behaviour of the randomness
      @Override
      protected LocalDate today(){
         return LocalDate.of(2020,3,04);
      }
   }
}
```