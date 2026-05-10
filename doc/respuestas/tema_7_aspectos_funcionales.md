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
La sintaxis básica es:

```java
(parámetros) -> expresión
```

o, si el cuerpo tiene varias instrucciones:

```java
(parámetros) -> {
    instrucciones;
    return valor;
}
```

Las reglas prácticas más importantes son:
- Si hay un solo parámetro, los paréntesis pueden omitirse: `s -> s.length()`.
- Si hay cero parámetros, se usan paréntesis vacíos: `() -> 42`.
- Si el cuerpo es una sola expresión, no hace falta `{}` ni `return`.
- Si el cuerpo es un bloque, entonces sí hay que escribir `return` cuando el tipo de retorno no sea `void`.

Ejemplos:

```java
Function<String, String> aMayusculas = s -> s.toUpperCase();
Function<String, Integer> longitud = s -> s.length();
Runnable saludar = () -> System.out.println("Hola");
```

La idea clave es que la lambda no declara un nombre de método propio: el compilador la adapta a la interfaz funcional esperada por el contexto.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
En este caso el método no hace la transformación directamente, sino que delega esa responsabilidad en la función que recibe como parámetro. Eso permite reutilizar el mismo método con comportamientos distintos.

Java:

```java
import java.util.function.Function;

public class Ejemplo {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}
```

JavaScript:

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

const aMayusculas = s => s.toUpperCase();
console.log(transformar("hola mundo", aMayusculas));
```

Lo importante es que `transformar` queda desacoplado de la lógica concreta. Hoy puede convertir a mayúsculas y mañana puede recortar, invertir, limpiar espacios o lo que haga falta, sin cambiar el método.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Esto se llama pasar una lambda inline, es decir, definir el comportamiento en el mismo sitio donde se usa. Es muy útil cuando la función solo se necesita una vez y no merece la pena darle un nombre aparte.

Java:

```java
String invertida = transformar("hola mundo", s -> new StringBuilder(s).reverse().toString());
System.out.println(invertida);
```

JavaScript:

```javascript
console.log(transformar("hola mundo", s => s.split("").reverse().join("")));
```

Aquí la lambda actúa como un valor literal de tipo función. El método `transformar` no sabe ni le importa si la función viene de una variable o está escrita directamente en la llamada.


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
La diferencia principal es que una lambda es un objeto o valor de tipo función con más capacidad semántica que un simple puntero a función.

En C, un puntero a función solo apunta a código ejecutable. Eso significa que:
- No captura estado del entorno donde se creó.
- No tiene cierre o closure.
- No tiene una interfaz de tipo enriquecida asociada.
- Normalmente solo puede invocar una función con una firma concreta.

En cambio, una lambda en lenguajes como Java o JavaScript puede:
- Capturar variables del contexto exterior.
- Comportarse como dato de primera clase.
- Adaptarse a una interfaz funcional o tipo de función concreto.
- En Java, incluso puede convertirse en una instancia de interfaz funcional mediante el compilador.

En resumen: un puntero a función es una dirección de código; una lambda suele ser una abstracción funcional más rica, porque puede llevar comportamiento y contexto a la vez.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Aquí la idea es construir una función que devuelve otra función. La función exterior fija un parámetro, y la función interior lo reutiliza después cuando se aplique a distintas cantidades.

```java
import java.util.function.Function;

public class Descuentos {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje / 100.0);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        System.out.println(descuento10.apply(100.0)); // 90.0
        System.out.println(descuento25.apply(100.0)); // 75.0
    }
}
```

La closure aparece porque la lambda `cantidad -> ...` captura la variable `porcentaje` del contexto donde fue creada. Esa variable no se pierde cuando `crearDescuento` termina; queda asociada a la función devuelta. Por eso cada llamada a `crearDescuento` produce una función distinta, con su propio porcentaje cerrado dentro.

En Java, esa variable capturada debe ser efectivamente final: no puede cambiarse después dentro del flujo local donde se capturó. Eso simplifica mucho la implementación y evita problemas de concurrencia o de vida de las variables.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una interfaz funcional es una interfaz que representa exactamente una operación abstracta. Se usa como tipo de destino para una lambda o referencia a método.

Requisitos principales:
- Debe tener un solo método abstracto.
- Puede tener métodos `default` y `static` sin problema.
- Puede heredar de otras interfaces siempre que, al final, conserve un único método abstracto.
- Suele anotarse con `@FunctionalInterface` para que el compilador verifique que no deje de cumplir la condición.

Ejemplos típicos:
- `Function<T, R>`: recibe un `T` y devuelve un `R`.
- `Consumer<T>`: recibe un `T` y no devuelve nada.
- `Supplier<T>`: no recibe nada y devuelve un `T`.
- `Predicate<T>`: recibe un `T` y devuelve un `boolean`.

La ventaja de una interfaz funcional es que da un tipo concreto a una lambda, lo que permite tipado estático, autocompletado y comprobación de errores en compilación.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Ejemplo de uso:

```java
Transformador aMayusculas = texto -> texto.toUpperCase();
System.out.println(aMayusculas.transformar("hola"));
```

Con `@FunctionalInterface` le pedimos al compilador que valide que la interfaz sigue teniendo un único método abstracto. Si alguien añade otro método abstracto por error, el compilador avisará.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
La versión genérica queda mucho más reutilizable porque el tipo de entrada y el de salida quedan parametrizados.

```java
@FunctionalInterface
public interface Transformador<E, S> {
    S transformar(E entrada);
}
```

Ejemplo de uso:

```java
Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
Integer resultado = redondear.transformar(3.7);
System.out.println(resultado); // 4
```

Este diseño tiene la misma idea que `Function<T, R>` de Java. La diferencia es que aquí estamos creando nuestra propia interfaz para entender cómo funciona el mecanismo por dentro.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Java trae muchas interfaces funcionales ya preparadas en el paquete `java.util.function`. Las más usadas son:

- `Function<T, R>`: transforma un valor en otro.
- `BiFunction<T, U, R>`: transforma dos valores en uno.
- `Consumer<T>`: consume un valor y no devuelve nada.
- `BiConsumer<T, U>`: consume dos valores y no devuelve nada.
- `Supplier<T>`: produce un valor y no recibe parámetros.
- `Predicate<T>`: evalúa una condición y devuelve `boolean`.
- `BiPredicate<T, U>`: condición sobre dos valores.
- `UnaryOperator<T>`: transforma un valor `T` en otro `T`.
- `BinaryOperator<T>`: combina dos valores del mismo tipo `T` y devuelve otro `T`.

Además existen variantes primitivas para evitar boxing innecesario, como:
- `IntUnaryOperator`, `IntPredicate`, `IntConsumer`
- `LongFunction`, `DoubleFunction`
- `ToIntFunction`, `ToDoubleFunction`, `ToLongFunction`

Estas interfaces cubren casi todos los casos habituales del estilo funcional en Java sin necesidad de definir nuevas interfaces cada vez.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
```java
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(-3, 0, 5, 8, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```

`forEach` recibe un `Consumer<T>`, es decir, una función que consume cada elemento de la lista. En este caso la lambda decide qué hacer con cada entero: si es positivo, imprime un mensaje; si no, no hace nada.

Esto es una forma más declarativa de recorrer la colección que un `for` tradicional, aunque internamente sigue iterando elemento por elemento.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
La firma de `forEach` usa `Consumer<? super T>` porque el consumidor no necesita ser exactamente de tipo `T`; basta con que pueda aceptar `T` o algún supertipo suyo. Eso lo hace más flexible.

La regla PECS significa:
- Producer Extends
- Consumer Super

La idea es sencilla:
- Si una estructura produce valores para nosotros, usamos `extends`.
- Si una estructura consume valores que le damos, usamos `super`.

Aplicado a `forEach`, la lista produce elementos de tipo `T`, y el consumidor los recibe. Por eso el parámetro del consumidor se declara como `Consumer<? super T>`: así puedo pasar un consumidor de `Object` para una lista de `String`, porque `Object` también puede consumir `String`.

En el método `transformar`, la mejora dependerá del papel del parámetro función. Si la función recibe un `String` y devuelve otro `String`, `Function<String, String>` ya es suficientemente claro. Pero si quisiéramos generalizar un poco más, podríamos pensar en la idea de que el transformador consuma un subtipo apropiado y produzca un resultado compatible. En la práctica, `Function<? super T, ? extends R>` es una firma muy flexible:

```java
public static <T, R> R transformar(T valor, Function<? super T, ? extends R> transformador) {
    return transformador.apply(valor);
}
```

Con eso, si tengo un valor de tipo `String`, puedo usar una función que acepte `Object` como entrada y devuelva cualquier subtipo de `R` como salida. Esa es la misma lógica de flexibilidad segura que usa la API estándar de Java.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos son una forma compacta de expresar una lambda cuando ya existe un método compatible.

Java:

```java
public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static void main(String[] args) {
        Persona persona = new Persona("Ana");
        Runnable referencia = persona::saludar;
        referencia.run();
    }
}
```

JavaScript:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const persona = new Persona("Ana");
const referencia = persona.saludar.bind(persona);
referencia();
```

En Java la referencia `persona::saludar` funciona directamente porque el lenguaje adapta el método a la interfaz funcional esperada. En JavaScript, como el valor de `this` depende de cómo se invoque el método, normalmente hay que fijarlo con `bind` para conservar el contexto del objeto.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
Java admite varias formas de referencia a método:

1. Referencia a método estático: `Clase::metodoEstatico`
2. Referencia a constructor: `Clase::new`
3. Referencia a método de una instancia concreta: `objeto::metodo`
4. Referencia a método de cualquier instancia de un tipo concreto: `Clase::metodoDeInstancia`

Ejemplo completo:

```java
import java.util.function.Function;
import java.util.function.Supplier;

public class ReferenciasMetodo {
    public static String convertirMayusculas(String s) {
        return s.toUpperCase();
    }

    public static void main(String[] args) {
        Function<String, String> refEstatico = ReferenciasMetodo::convertirMayusculas;

        Supplier<StringBuilder> refConstructor = StringBuilder::new;

        String texto = "hola";
        Supplier<Integer> refInstanciaConcreta = texto::length;

        Function<String, Integer> refCualquierInstancia = String::length;

        System.out.println(refEstatico.apply("hola"));
        System.out.println(refConstructor.get());
        System.out.println(refInstanciaConcreta.get());
        System.out.println(refCualquierInstancia.apply("mundo"));
    }
}
```

La diferencia entre las dos últimas es importante: en `texto::length` ya se fija una instancia concreta; en `String::length`, el receptor del método todavía queda por proporcionar cuando se invoque la función.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Versión manual con lambda directamente en `Collections.sort`:

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Persona {
    private final String nombre;
    private final int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }

    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Bea", 25));

        Collections.sort(personas, (p1, p2) -> {
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacionEdad != 0) return comparacionEdad;
            return p1.getNombre().compareTo(p2.getNombre());
        });
    }
}
```

Versión usando `Comparator`, que deja la intención más clara:

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class PersonaOrdenacion {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Bea", 25));

        Comparator<Persona> porEdadYNombre = Comparator
            .comparingInt(Persona::getEdad)
            .thenComparing(Persona::getNombre);

        Collections.sort(personas, porEdadYNombre);
    }
}
```

La segunda forma suele ser preferible porque es más expresiva, más componible y aprovecha mejor las utilidades estándar del lenguaje. Si más adelante se quiere ordenar al revés, se puede encadenar con `reversed()` o cambiar fácilmente el criterio sin reescribir toda la comparación.
