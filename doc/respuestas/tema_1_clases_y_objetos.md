CUESTIONARIO: CLASES Y OBJETOS

ESTAS RESPUESTAS HAN SIDO GENERADAS POR EL MODELO CLAUDE SONNET 4.5 COMO "DRAFT" INICIAL, PERO HAN SIDO REVISADAS UNA A UNA Y RECIBIDO LOS CAMBIOS QUE CONSIDERÉ PERTINENTES.
***

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

**Encapsulación**: consiste en agrupar datos (atributos) y comportamiento (métodos) dentro de una misma unidad (clase), ocultando los detalles internos. Permite controlar el acceso mediante modificadores de visibilidad (public, private, protected).

Consiste principalmente en 2 cosas (más como un gran grupo con un subgrupo muy importante):
1- Unir información y funciones (sobre esa información) en un mismo artefacto (Usamos clases en vez de structs, son básicamente structs mamadísimos con cosas como herencia, (2-) ocultar partes con private, etc... La "función" (el **método**) se encapsula dentro de la clase).

**Herencia**: permite que una clase derive de otra, heredando sus atributos y métodos. Facilita la reutilización de código y la creación de jerarquías conceptuales entre tipos de datos (Perro y Gato heredan de Animal, Perro o Gato pueden tener una especialización adicional).

La Herencia permite abstracción (recomendable) y reutilización (deprecado).

```java
    class Animal {
        dormir();
        ...
    }
```

```java
    
    Perro perro = new Perro();
    perro.ladrar();
    perro.dormir();
        
    Gato gato = new Gato();
    gato.maullar();
    gato.dormir();
        
```



**Polimorfismo**: es la capacidad de un objeto de adoptar múltiples formas. Permite que métodos con el mismo nombre tengan comportamientos diferentes según el contexto o la clase que los implemente (polimorfismo por sobrecarga o por sobrescritura).

Misma función, distintas implementaciones en función del tipo.

**Abstracción**: consiste en representar características esenciales de un objeto ocultando detalles innecesarios (manejar mejor los temas complejos) y facilitando la modificación y mantenimiento. Se logra mediante clases abstractas e interfaces, permitiendo trabajar con conceptos generales sin preocuparse por la implementación específica. Se podría decir que el resto de características son auxiliares a esta (Leer el índice de un libro es abstraerse del contenido, teniendo una visión global de mas alto nivel). 

-> Ejemplo de modificación con abstracción: si quiero cambiar el índice de un libro (modo 1), no me importa si cambia el contenido de cada apartado (modo 2).

-> Otro ejemplo, puede cambiar el funcionamiento interno de la API pero que las llamadas externas funcionen igual.

***

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java, Rust, C++ (que nació literalmente como C pero con orientación a objetos), C#.

***
-> Saber si un lenguaje es compilado no es tan importante como que tenga comprobación estática de tipos.

-> También si tienen o no tienen recolector de basura [Java y C# (Memory Safe) vs C++]. Rust es Memory Safe pero tampoco tiene recolector de basura, complica trabajar con el lenguaje pero da resultados muy seguros.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Primer lenguaje de programación con software **ensamblador** -> Secuencia de instrucciones (intención de programa) y saltos arbitrarios (da pie a muchas limitaciones, un for por ejemplo se hace con saltos hasta la misma función de memoria hasta que se cumpla cierta condición). Esto es considerado código spaghetti.

La **programación estructurada** surgió en los años 60-70 como alternativa a la programación sin control de flujo (spaghetti code). Se basa en el uso de estructuras de control bien definidas (if-else, while, for) y en la división del código en bloques lógicos. El énfasis está en cómo ejecutar instrucciones de forma ordenada (no más saltos constantes a direcciones de memorias, claras condiciones fáciles de seguir en comparación), no en organizar datos. Se conoce también como **programación procedural** y aún hoy en día mucha gente aboga por ella, por lo "limpio" que es un flujo donde la función es la unidad de ordenación.

La **programación modular** lleva la estructuración un paso más allá: divide el programa en módulos independientes y reutilizables, cada uno con una responsabilidad clara. Cada módulo puede tener sus propias variables (datos locales) y funciones que operan sobre esos datos. Sin embargo, la separación entre datos y operaciones sigue siendo explícita: pasas los datos a funciones que los manipulan. En C, esto se emula con estructuras de datos (struct) y funciones que reciben punteros a esas estructuras. La POO unifica este concepto: los objetos son módulos que combinan datos y comportamiento en una sola entidad.

***

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

-En el paradigma OOP, en memoria tenemos Objetos (instancias de clase). Se dice que trabajamos con Perros y Gatos, no necesariamente todos los Animales.

**Identidad**: cada objeto es único y distinguible de los demás, identificado por su posición en memoria (referencia/dirección). Puedo tener dos variables Punto cuyo `x` e `y` tienen el mismo valor, pero gracias a que cada uno tiene su propia identidad, puedo emplear el que considere conveniente (independientemente del estado que tengan!).

**Estado**: representado por los valores de sus **atributos** en un momento dado. Dos objetos de la misma clase pueden tener estados diferentes (El estado de un `Punto` concreto es lo que valgan su `x` e `y`).

**Comportamiento**: definido por (el conjunto de los) los métodos que pueden ejecutarse sobre el objeto, que pueden cambiar su estado o devolver información sobre él. 

***

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es una plantilla o esquema abstracto que define la estructura (atributos) y el comportamiento (métodos) de un tipo de objeto.

 Un **objeto** es una entidad concreta que existe en memoria, creada a partir de esa clase. Un gato es un objeto de clase animal, con sus características como número de patas, cantidad de ternura, etc. Otro objeto de la misma clase puede ser el ornitorrinco. 
  
 La **instancia** es exactamente lo mismo que un objeto: el resultado de crear una variable del tipo definido por la clase. Si la clase es el "plano" de una casa, cada objeto es una casa real construida según ese plano.

  Puedo tener tantos objetos de una clase (aquí es cuando generalmente decimos instancia); Sea la clase `Coche(marca, año)`, dos instancias son `Coche(Mercedes,2009)`, `Coche(Mazda,2020)`.

No todos los lenguajes orientados a objetos requieren clases en el sentido tradicional. Algunos lenguajes, como JavaScript (en su versión original) o Lua, utilizan **programación orientada a objetos basada en prototipos**, donde los objetos se crean directamente desde otros objetos (prototipos) sin necesidad de clases explícitas. Sin embargo, la mayoría de los lenguajes modernos (Java, C++, C#, Python) sí utilizan clases.

***

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?

En Java, los objetos se almacenan en el **heap** (montículo), un área de memoria dinámica que gestiona el programa en tiempo de ejecución. Las variables que hacen referencia a esos objetos (referencias) se almacenan en la **pila** (stack) si son locales. Esto es diferente a C/C++, donde decidir si usar stack o heap es responsabilidad del programador: en C, un struct se puede declarar en la pila, o asignar dinámicamente con malloc en el heap.

A modo de explicación breve y condensada, mientras que el heap es un espacio de memoria más grande donde el acceso es más lento porque hay que localizar y poner las cosas en su sitio según lo que ya está, en el stack predefinimos las direcciones de memoria de cosas como variables locales. Esto es un problema para lenguajes interpretados como Python que van sobre la marcha.

No es igual en todos los lenguajes: en C++, un objeto puede almacenarse en la pila (como variable local) o en el heap (con new). En Rust, existe un sistema de propiedad que controla automáticamente cuándo liberar memoria. El **recolector de basura (garbage collector)** es un mecanismo automático que detecta qué objetos ya no son alcanzables (no hay referencias apuntando a ellos) y libera su memoria automáticamente. En Java es transparente para el programador, a diferencia de C/C++ donde debes liberar manualmente con delete o free. Esto evita memory leaks, aunque tiene el costo de pausas ocasionales en la ejecución.

***

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**?

Un **método** es una función definida dentro de una clase que opera sobre los objetos de esa clase. Puede acceder a los atributos del objeto (estado) e interactuar con otros métodos. En esencia, es similar a una función en C que recibe un puntero a una estructura (this implícito), pero con sintaxis integrada en el lenguaje.

La **sobrecarga de métodos (method overloading)** permite definir múltiples métodos con el mismo nombre dentro de la misma clase (ojo: **no confundir con shadowing**, que es cuando una variable en un ámbito interno oculta a otra del mismo nombre en un ámbito externo), siempre que tengan diferentes **firmas**: distinto número de parámetros, tipos diferentes, o ambos. El compilador elige automáticamente cuál ejecutar según los argumentos pasados. Por ejemplo, puedes tener un constructor Punto() sin parámetros y otro Punto(int x, int y). En C no existe sobrecarga; deberías crear funciones distintas con nombres diferentes (ej: Punto_crear() y Punto_crearCon()).

***

```java

class Calculadora {
    //sin estado

    int sumar (int a, int b) { //suma1
        return a+b;
    }

    double sumar(double a, double b) { //suma2
        return a+b;
    }

    main() {
        Calculadora miCalculadora = new Calculadora();

        int primeraSuma = miCalculadora.sumar(4,6); //El compilador sabe que suma1 es el método a usar
        int segundaSuma = miCalculadora.sumar(4.6,5.2) //Emplea suma2
    }
}

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

```java
class Punto {
    int x;
    int y;
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y); //se que, al estar dentro de la clase, puedo utilizar los valores que contiene la clase Punto
    }
}

public class Main {
    public static void main(String[] args) {
        Punto miPunto = new Punto();
        miPunto.x = 3;
        miPunto.y = 4;
        
        System.out.println("Distancia al origen: " + miPunto.calculaDistanciaAOrigen());
    }
}
```

```C

struct Punto {
    int x;
    int y;
}

double calculaDistanciaAOrigen(struct Punto p) {
    //distancia de p a (0,0)
    return sqrt((p.x * p.x)+(p.y * p.y));
}


void main() {
    Punto miPunto;
    miPunto.x = 5;
    miPunto.y = 3;
    calculaDistanciaAOrigen(miPunto);

}

```

**Compila y ejecuta con:**
```bash
javac Main.java
java Main
```

***

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa Java es el método `main` con firma: `public static void main(String[] args)`. Es el primer método que la JVM (máquina virtual) busca y ejecuta cuando se lanza el programa.

**`static`** indica que el método o atributo pertenece a la **clase**, no a instancias particulares. Un método estático puede ejecutarse sin crear un objeto de su clase; se invoca usando el nombre de la clase (ej: `Main.main()`). En C, es similar a variables y funciones de ámbito global dentro de un archivo. `main` es estático porque la JVM no tiene objetos creados aún cuando lanza el programa; solo tiene acceso a la clase.

`static` no se usa solo en `main`. Puedes tener atributos estáticos (compartidos por todas las instancias) y métodos estáticos. Cuando se combina con **`final`**, se crea una **constante de clase**: una variable que es estática (existe una única copia) e inmutable (no puede cambiar). Ejemplo:

```java
public static final double PI = 3.14159;
```

***

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutar desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar:

```bash
javac Programa.java    # Compila el archivo .java a bytecode
java Programa          # Ejecuta el programa
```

Java es un lenguaje **compilado a bytecode**, no a código máquina nativo. El compilador `javac` traduce el código fuente `.java` a instrucciones intermedias (bytecode) guardadas en archivos `.class`. Esta es la clave de la filosofía "write once, run anywhere" de Java. Es muy común ver por ejemplo mods de Minecraft empaquetados como un `.jar`, compatible con todas el juego en todas las plataformas de escritorio.

La **máquina virtual de Java (JVM)** es un programa que actúa como intérprete: lee el bytecode del archivo `.class` y lo ejecuta en la máquina actual. Cada sistema operativo (Windows, Linux, macOS) tiene su propia JVM, pero todos entienden el mismo bytecode. En C/C++, compilas directamente a código máquina para un sistema operativo específico; en Java, compilas una sola vez y ejecutas en cualquier JVM. El **bytecode** es un formato intermedio legible pero no ejecutable directamente por el procesador; la JVM lo interpreta (y en tiempo de ejecución, puede compilar partes críticas a código nativo con JIT compilation).

Esto significa que Java nunca será tan rápido como lenguajes compilados directamente a lenguaje máquina como C++, pero también suele ser mucho más veloz que Python. El lenguaje es tan valioso justamente por, entre otras cosas, encontrarse en un punto medio de rendimiento y compatibilidad.

***

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

**`new`** es el operador que crea una nueva instancia (objeto) en el heap. Alloca memoria, inicializa los atributos con valores por defecto (0 para números, null para referencias), y llama al constructor.

Un **constructor** es un método especial con el mismo nombre que la clase, sin tipo de retorno explícito. Se ejecuta automáticamente cuando se crea una instancia (con `new`). Permite inicializar el objeto con valores específicos y así hacerlos accesibles desde otro punto más allá de su propia clase (caso donde solo necesitaríamos que fuera `public`). Ejemplo:

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;
    
    // Constructor
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

// Uso
Empleado emp = new Empleado("12345678X", "Juan", "García");
```

En C, no existen constructores; deberías crear una función `Empleado_crear()` que aloque memoria y inicialice el struct manualmente.

***

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia **`this`** es una variable especial disponible **dentro de los métodos** de una clase que siempre apunta al **objeto actual** sobre el que se está ejecutando ese método. Cuando se llama a un método de un objeto, Java automáticamente pasa una referencia a ese objeto como primer parámetro **invisible** llamado `this`. Es como si cada método recibiera un parámetro adicional que dice "este es el objeto concreto con el que estás trabajando".

Piensa en `this` como un "**yo**" del objeto. Cuando dentro de un método escribes `x = 5`, Java no sabe si te refieres al parámetro `x` de ese método o al atributo `x` del objeto. **`this.x = 5`** deja claro que hablas del atributo `x` **del objeto actual**. Sin `this`, hay ambigüedad entre parámetros locales y atributos de clase.

No se llama igual en todos los lenguajes: en C++ también es `this`, en Python es `self` (debes pasarlo explícitamente como primer parámetro), en JavaScript es `this` pero con comportamiento diferente. Ejemplo detallado en `Punto`:

```java
class Punto {
    int x;  // ATRIBUTO de la clase
    int y;
    
    // Constructor con parámetros que se LLAMAN igual que los atributos
    Punto(int x, int y) {
        // PROBLEMA: x e y son parámetros LOCALES del constructor
        // x = x;  // ¡Esto NO funciona! Asigna el parámetro al mismo parámetro
        // y = y;  // ¡Esto NO funciona!
        
        this.x = x;  // OK: el atributo x del objeto actual = parámetro x
        this.y = y;  // OK: el atributo y del objeto actual = parámetro y
    }
    
    void desplazar(int x, int y) {
        // Mismo problema: parámetros x,y ocultan los atributos x,y
        // this.x = this.x + x;  // atributo_x = atributo_x + parámetro_x
        this.x += x;  // Sintaxis abreviada
        this.y += y;
    }
}
```

**¿Qué pasa SIN `this`?** Si quitas `this.` el compilador asume que `x` y `y` son los **parámetros locales**, no los atributos. Los atributos de la clase quedarían sin inicializar (con valor 0).

**Uso típico del constructor:**
```java
Punto p1 = new Punto(3, 4);  // Java internamente hace:
// Punto.this_implicito = new Punto()
// this_implicito.x = 3
// this_implicito.y = 4
// p1 apunta a ese objeto

p1.desplazar(2, -1);  // Java hace:
// desplazar(p1, 2, -1)
// dentro del método: this == p1
// this.x = this.x + 2  →  3+2 = 5
// this.y = this.y + (-1) → 4-1 = 3
```

**Resumen visual:**
```
Cuando ejecutas: p1.desplazar(2, -1)
Java realmente ejecuta: desplazar(p1, 2, -1)
Donde dentro del método:
- this     → p1 (el objeto actual)
- parámetro x → 2  
- parámetro y → -1
```

**`this` nunca es `null`** dentro de métodos de instancia y **siempre apunta al objeto que recibió la llamada**. Es la forma que tiene Java de decirte "trabajas con ÉSTE objeto concreto".

***

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

```java
class Punto {
    int x;
    int y;
    
    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
    
    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Uso
Punto p1 = new Punto(0, 0);
Punto p2 = new Punto(3, 4);
System.out.println(p1.distanciaA(p2));  // Imprime 5.0
```

Aún así en `distanciaA` no necesitamos `this` puesto que otro.x viene de otro objeto y por defecto asumimos que una variable es interna a un método.
Sería problemático hacer `x-x` , al pasar el segundo `x` sin su objeto `otro`  se produce incertidumbre.


**Orden de prioridad de resolución de nombres:**

1. this explícito  (SIEMPRE máxima prioridad)
2. Parámetros del método  (ocultan atributos)
3. Variables locales 
4. ATRIBUTOS DE INSTANCIA (this.x implícito - ÚLTIMO)

***

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función?

En Java, **se pasan referencias por valor**. Esto significa que se copia la referencia (la dirección de memoria del objeto), no el objeto en sí. Si modificas los atributos del objeto dentro del método, esos cambios persisten fuera del método porque ambas referencias apuntan al mismo objeto en el heap.

```java
void modificaPunto(Punto p) {
    p.x = 10;  // Modifica el objeto original
}

Punto p = new Punto(3, 4);
modificaPunto(p);
System.out.println(p.x);  // Imprime 10
```

Sin embargo, si reasignas la referencia dentro del método, el cambio **no afecta al original**:

```java
void reasignaPunto(Punto p) {
    p = new Punto(100, 100);  // Solo afecta a la copia local de la referencia
}

Punto p = new Punto(3, 4);
reasignaPunto(p);
System.out.println(p.x);  // Imprime 3, no 100
```

Con tipos primitivos como `int`, ocurre **paso por valor verdadero**: se copia el valor, y cualquier modificación dentro del método no afecta al original.

```java
void incrementa(int valor) {
    valor = valor + 1;  // No afecta al original
}

int x = 5;
incrementa(x);
System.out.println(x);  // Imprime 5, no 6
```

***

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

**`toString()`** es un método especial definido en la clase base `Object` que devuelve una representación en texto del objeto. Cuando usas un objeto en un contexto donde se espera un String (como `System.out.println()`), se invoca automáticamente `toString()`. Por defecto, devuelve algo como "Punto@1a2b3c4d" (nombre de clase + código hash), poco útil.

Se puede **sobreescribir** (override) para proporcionar una representación más legible:

```java
class Punto {
    int x;
    int y;
    
    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    @Override
    public String toString() {
        return "Punto(" + this.x + ", " + this.y + ")";
    }
}

Punto p = new Punto(3, 4);
System.out.println(p);  // Imprime: Punto(3, 4)
```

Otros lenguajes como C++ tienen métodos similares (operator<<), y Python tiene `__str__()`. Es una práctica común en OOP para depuración y legibilidad.

***

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Un `struct` en C es **conceptualmente similar a una clase**, pero con diferencias fundamentales. Ambos agrupan datos en una estructura con nombre. Sin embargo, un struct es **solo datos**: no puede encapsular comportamiento (métodos) directamente. En C, separas datos y funciones:

```c
struct Punto {
    int x;
    int y;
};

double distancia(struct Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}
```

Lo que le falta al struct para ser una clase es:

1. **Métodos integrados**: la función existe fuera de la estructura, no forma parte de ella
2. **Control de acceso**: no hay modificadores private/protected, todo es accesible
3. **Constructores**: no hay inicialización automática al crear la instancia
4. **Herencia y polimorfismo**: no puedes derivar un struct de otro ni sobrescribir comportamiento
5. **Abstracción**: no hay encapsulación real de responsabilidades

En C++, los structs son técnicamente clases (solo que con visibilidad pública por defecto), así que pueden tener métodos, constructores y herencia. La diferencia entre struct y class en C++ es meramente sintáctica. En Java, no existen structs; solo clases.

***

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría "emular", con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Así es cómo se emularía la clase `Punto` de Java usando C:

```c
#include <math.h>
#include <stdio.h>

struct Punto {
    int x;
    int y;
};

// "Constructor": función que inicializa el struct
struct Punto Punto_crear(int x, int y) {
    struct Punto p;
    p.x = x;
    p.y = y;
    return p;
}

// "Método": función que recibe el struct como primer parámetro
double Punto_calculaDistanciaAOrigen(struct Punto *this) {
    return sqrt(this->x * this->x + this->y * this->y);
}

// Uso
int main() {
    struct Punto p = Punto_crear(3, 4);
    printf("Distancia: %f\n", Punto_calculaDistanciaAOrigen(&p));
    return 0;
}
```

**`this` ha reaparecido explícitamente**: en Java es implícito (el compilador lo añade automáticamente), pero en C debes pasarlo manualmente como parámetro. La convención es hacerlo el primer parámetro. En lugar de `p.calculaDistanciaAOrigen()`, escribes `Punto_calculaDistanciaAOrigen(&p)`, pasando la dirección del objeto.

Esta es la "magia" que Java oculta: el compilador convierte tus llamadas a métodos en llamadas a funciones que reciben un puntero al objeto implícitamente. Cuando escribes `p.metodo()`, Java internamente hace algo equivalente a `Punto_metodo(&p)`. La orientación a objetos no es un concepto mágico; es **azúcar sintáctico sobre programación estructurada**: combina datos y funciones en módulos con sintaxis integrada, en lugar de pasarlos manualmente como en C.

***

**Documento completado y formateado correctamente.**