
# Planificación de Hilos

## Introducción

La planificación de hilos es un aspecto esencial en la programación concurrente. Permite gestionar qué hilo se ejecuta y cuándo, optimizando el uso de los recursos del sistema y garantizando que cada hilo tenga la oportunidad de ejecutarse.

En Java, la planificación de hilos es realizada por el planificador del sistema operativo, que decide qué proceso o hilo se ejecuta en un momento dado. Esto depende del número de núcleos disponibles y del algoritmo de planificación utilizado.

---

## Algoritmos de Planificación

### Planificación por Prioridades

Java utiliza un sistema de prioridades para los hilos. La prioridad de un hilo determina su "importancia" relativa en la ejecución. Los hilos con mayor prioridad tienen preferencia sobre los de menor prioridad, especialmente cuando no hay suficientes recursos para ejecutarlos todos simultáneamente.

- **Prioridades en Java**: Las prioridades se establecen usando el método `setPriority()` de la clase `Thread`.
  - Rango: `Thread.MIN_PRIORITY` (1) a `Thread.MAX_PRIORITY` (10).
  - Por defecto: `Thread.NORM_PRIORITY` (5).

#### Ejemplo:
```java
Thread hilo1 = new Thread(() -> {
    System.out.println("Hilo 1 ejecutándose");
});
Thread hilo2 = new Thread(() -> {
    System.out.println("Hilo 2 ejecutándose");
});

hilo1.setPriority(Thread.MAX_PRIORITY);
hilo2.setPriority(Thread.MIN_PRIORITY);

hilo1.start();
hilo2.start();
```
> **Nota**: Aunque se establece la prioridad, el sistema operativo puede no respetarla estrictamente.

### Planificación Cooperativa

En este modelo, un hilo de mayor prioridad puede ejecutarse completamente hasta que cambie de estado (por ejemplo, a `Bloqueado` o `Terminado`), antes de que se ejecute un hilo de menor prioridad.

### Planificación Apropiativa

Java utiliza este modelo por defecto. Si un hilo con mayor prioridad se vuelve `Runnable`, el sistema lo seleccionará inmediatamente para su ejecución.

---

## Interrupción de Hilos

No es posible detener un hilo de manera directa desde fuera. En lugar de ello, se recomienda solicitar la interrupción del hilo mediante el método `interrupt()`. Esto implica que:

1. El hilo debe comprobar periódicamente si ha sido interrumpido.
2. Los métodos de la biblioteca estándar, como `Thread.sleep()`, lanzan una excepción si son interrumpidos.

#### Métodos Relacionados:

- **Interrumpir un hilo**:
  ```java
  hilo.interrupt();
  ```
- **Comprobar interrupción del hilo actual**:
  ```java
  if (Thread.interrupted()) {
      System.out.println("Hilo actual interrumpido");
  }
  ```
- **Verificar si otro hilo ha sido interrumpido**:
  ```java
  if (hilo.isInterrupted()) {
      System.out.println("Hilo interrumpido");
  }
  ```

#### Ejemplo:
```java
Thread hilo = new Thread(() -> {
    while (!Thread.currentThread().isInterrupted()) {
        System.out.println("Hilo trabajando...");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            System.out.println("Hilo interrumpido durante sleep.");
            break;
        }
    }
    System.out.println("Hilo detenido.");
});

hilo.start();
Thread.sleep(3000);
hilo.interrupt();
```

---

## Consideraciones de Uso

- **Herencia de Prioridades**: Los hilos heredan la prioridad del hilo que los crea, pero esta puede modificarse posteriormente.
- **Compatibilidad con el Sistema Operativo**: La planificación depende del sistema operativo subyacente y puede no ser completamente controlada por Java.
- **Uso Responsable de Interrupciones**: Las interrupciones deben manejarse cuidadosamente para evitar estados inconsistentes en los hilos.

---

## Ejercicio Propuesto

**Crea un programa en Java que:**
1. Cree tres hilos con diferentes prioridades.
2. Cada hilo debe imprimir un mensaje en bucle durante un tiempo determinado.
3. Permite interrumpir uno de los hilos después de un periodo.

#### Pista:
Usa los métodos `setPriority()`, `interrupt()` y `sleep()` para gestionar las prioridades y la interrupción de los hilos.

---

## Referencias

- [Documentación de la clase `Thread`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html)
- [Gestión de Hilos en Java](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)
