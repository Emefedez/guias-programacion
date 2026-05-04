# TEMA 7. Aspectos funcionales

# 0. Como entender un lenguaje concreto a nivel funcional:

1. El **Objetivo** de la programación funcional es que las funciones sean "ciudadanos de primera clase", es decir, sean un tipo más:
    1. Pueden ser asignadas a variables.
    2. Pueden ser recibidas como parámetros.
    3. Pueden ser devueltas en otras funciones.

2. La **Expresión lambda** expresa un valor de tipo función (sin nombre, solo cabecera y cuerpo). El nombre lo tendrá la variable o parámetro al que se asigna y/o retorna.

3. Los cierres o **Closures** son muy importantes en la programación funcional, ya que permiten a las funciones lambda acceder a variables del contexto donde fueron definidas.

4. En lenguajes con comprobación estática de tipos, como Java, cada función lambda tiene un tipo (que es importante conocer), que se conoce como **interfaz funcional** que siempre tiene un único método abstracto.


## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
```C
int sumar(int a, int b) {
    return a + b;
}
int restar(int a, int b) {
    return a - b;
}
int main() {
    int (*operacion)(int, int); //(*nombre_de_variable)(parametros)

    operacion = sumar; //Asignamos la función sumar al puntero
    printf("Suma: %d\n", operacion(5, 3)); 

    operacion = restar; //Asignamos la función restar al puntero
    printf("Resta: %d\n", operacion(5, 3));

    return 0;
}
```
* Los punteros a función en `C`, un lenguaje monoparadigma sin OOP, carece de `expresiones lambda` y `Closures`.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

* Veamos la expresiones lambda (también conocidas muchas veces como "arrow functions"):

```java
void toUpperCase(String s) {
    s -> s.toUpperCase(); //de usar múltiples parámetros se haría así: (p1,p2,p3) -> {cuerpo}
}
```

* La idea de la programación funcional no es crear nada nuevo, si no que permite expresar cosas de manera más legible y potencialmente compacta.

```java
/*
String toUpperCase(String s) {
    Function<String, String> miFuncion = s -> s.toUpperCase(); //String es el tipo de entrada (1) y String el tipo de salida (2) [Function<(1),(2)>]
    return miFuncion.apply(s);
    //en C, C# y etc..., la sintaxis para aplicar la función a un parámetro sería: miFuncion(s);
}
*/

static String transformar(String s, Function<String, String> transform) {
    return transform.apply(s);
}

public static void main(String[] args) {
    //Llamando a transformar con una función lambda directamente en la llamada
    Function<String, String> miFuncion = s -> s.toUpperCase(); //contexto fundamental
    String resultado = transformar("hola mundo", miFuncion); //transformar recibe la función toUpperCase como parámetro
    System.out.println(resultado); //HOLA MUNDO
    //podríamos haber transformado la cadena de manera distinta con un mismo método pero cambiando el parámetro de la función lambda
}
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?


### Respuesta

* Seamos más concretos con Java. El compilador transforma el código funcional en Java "normal" (como vimos hasta ahora) para la JVM. El compilador convierte las expresiones lambda en expresiones usuales de Java antiguo.

* Tienen **Interfaces funcionales** a las que hay que darles cuerpo:
    1. Son interfaces.
    2. Tienen un sólo método abstracto.
    3. Valen como tipo para funciones lambda. (Es decir, el tipo de las funciones lambda en Java siempre será el de interfaz funcional).

### Ejemplo: 
```java
@FunctionalInterface // Si queremos que nos prohiba agregar más métodos, aseguramos que mantiene las características de interfaz funcional con esta etiqueta.
public interface Transformador {
    public String transformar(String);
}

public main() {
    Transformador miTransform = s -> s.toUpperCase();
}
```

* **¡Eureka!** `Function` es una interfaz funcional.

* El transformador que hicimos es muy parecido a como se haría con otro tipo, ¡empleemos genéricos!

```java
public interface Transformador<E,S> {
    public S transformar(E entrada);
}

public main() { //ahora podemos aprovechar la genericidad, para crear un transformador de cualquier tipo a cualquier tipo.
     Transformador<String, String> miTransform = s -> s.toUpperCase();
     Transformador<String, Integer> longitud = s -> s.length(); 
     Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
     
    //Si queremos aplicar cualquiera...
        String resultado1 = miTransform.transformar("hola mundo");
        Integer resultado2 = longitud.transformar("hola mundo");
        Integer resultado3 = redondear.transformar(3.14);
}
```

* Java tiene muchas interfaces funcionales predefinidas, como `Function`, `Consumer`, `Supplier`, `Predicate`, `BiFunction`, etc... que cubren la mayoría de casos comunes.

Por ejemplo, `Function<T,R>` es una interfaz funcional genérica que representa una función que toma un argumento de tipo T y devuelve un resultado de tipo R.

**No todas** las interfaces preestablecidas **emplean `apply()`**, por ejemplo: 
- `Consumer<T>` tiene un método `accept(T t)` que no devuelve nada, sino que consume el argumento. 
- `Supplier<T>` tiene un método `get()` que no recibe argumentos pero devuelve un valor de tipo T. 
- `Predicate<T>` tiene un método `test(T t)` que devuelve un booleano...

* Veamos un ejemplo de `Predicate`:
    (**OJO**, `.getSalario()`, se aplica sobre una interfaz funcional y no utiliza `apply`):

```java
Predicate<Empleado> filtro = e-> e.getSalario() > 1000; //Ejemplo de un predicado que filtra empleados con salario mayor a 1000.
````

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

La `closure` es independiente de Java, para ser plenamente funcional, una expresión lambda debe capturar las variables del contexto **donde fue declarada** (variables en el texto "próximo" a donde se expresa la función) **[OJO, CONTEXTO DONDE SE DECLARA != CONTEXTO DONDE SE LLAMA]**. Por ejemplo, imaginemos que se declara en el main y empleamos una variable local `saludo` que se concatena a la cadena de entrada; a pesar de que haya una variable `saludo` también donde es llamada para asistir, mantiene preferencia la del contexto inicial.

```java
public static Function<Double, Double> crearDescuento(int descuento) {
    return cantidad -> (cantidad * (1 - descuento/100)); 
    // Esta es la expresión lambda, que debe ser compatible con el tipo de retorno. 
    // Tenemos un único parámetro de entrada, será interpretado como Double. Hago alusión a cantidad (dentro de la expresión lambda) con una variable de fuera (el descuento).
    // Aunque hayamos pasado una variable por parámetros, es importante recordar que cantidad primero observa el contexto del interior de la función.
}

public static void main() {
    Function<Double, Double> descuento25 = crearDescuento(25);
    Function<Double, Double> descuento30 = crearDescuento(30);
    // ¡Esto es una "factoría" de funciones!
    double reducido1 = descuento25.apply(500);
    double reducido2 = descuento30.apply(800);

     // Aunque pasemos los descuentos como parámetros, las variables locales de las funciones lambda deben sobrevivir en lugar de lo que usualmente pasa (se borran). Java hace un nuevo Objeto secreto en el heap, una instancia que contiene el atributo requerido.
     // Esto no es muy "bueno".  Las variables capturadas en Java no permiten modificar el valor (son efectivily final). Otros lenguajes sí te dejan. Aun así, hay un "truco" o "arreglo" que nos permite gestionarlo como queremos. Aún así, no es particularmente legible y es muy verbose.
}
```

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
