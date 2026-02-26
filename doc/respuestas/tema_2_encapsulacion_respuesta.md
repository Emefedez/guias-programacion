<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

**Aunque a menudo se usen como sinónimos, hay matices que diferencian ambos términos:**

- **Encapsulación -->** Es el proceso de agrupar datos (atributos) y los métodos que operan sobre esos datos en una sola unidad o "caja" (la clase). Su objetivo es mantener todo lo relacionado con un objeto en un mismo lugar. Actúa como un escudo que añade protección a mi clase, la cuál es un artefacto con estado y con comportamiento, me permite ocultar miembros al exterior de la clase para:
    - Garantizar que mi estado interno es siempre válido.
    - Evitar que otro código dependa o acceda a partes que no quiero.
    - Facilitarme poder cambiar partes sin afectar a otras.

- **La Ocultación de información -->** Es la capacidad de restringir el acceso directo a los detalles internos de esa "caja". Busca separar la interfaz (lo que el objeto hace) de la implementación (cómo lo hace internamente).

- **En resumen-->** buscan que el mundo exterior solo interactúe con el objeto a través de una interfaz controlada (métodos públicos), impidiendo que se modifique el estado interno de forma arbitraria o errónea.


### En definitiva, estas son las principales ventajas:

**Protección de la integridad:** Evita que el código externo asigne valores inválidos a los atributos (por ejemplo, una "edad" negativa), ya que el acceso se filtra mediante métodos (getters/setters) que validan los datos.

**Facilidad de mantenimiento:** Puedes cambiar la lógica interna de un método o el tipo de una variable privada sin que el resto del programa se entere ni deje de funcionar.

**Reducción del acoplamiento:** Al depender solo de la interfaz pública, los diferentes módulos de un programa están menos "atados" entre sí, lo que facilita hacer cambios en uno sin romper los demás.

**Abstracción y simplicidad:** Como bien mencionaste, el usuario de la clase solo ve lo que necesita para trabajar, eliminando el ruido visual de la complejidad interna. 



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

El término interfaz pública hace referencia a todos los miembros (típicamente métodos, aunque también potencialmente atributos) de un objeto/clase a los que se puede acceder desde fuera (lo no explícitamente oculto). Requiere que sean clases públicas, si no sólo puedes acceder desde el mismo `.java`. Los métodos y atributos ocultos son aquellos que no salen en la interfaz pública.

Se podría decir entonces, que la interfaz pública es, fundamentalmente, la respuesta a la pregunta: "¿Qué puede hacer este objeto por mí?", sin importar cómo lo haga por dentro (Imagina llamar a una API).

Si especificamos más respecto a la relación que hay entre el término interfaz pública y la ocultación de objetos, diríamos que esta se basa en la **separación de preocupaciones:**

- **Punto de contacto-->** La interfaz pública es la única vía permitida para interactuar con el objeto. Todo lo que no está en la interfaz pública (atributos privados, métodos auxiliares de cálculo, etc.) está "oculto".

- **Abstracción-->** La ocultación permite que la interfaz sea minimalista. Al esconder los detalles complejos, la interfaz pública se mantiene simple y fácil de usar.

- **Estabilidad del contrato-->** Al ocultar la implementación interna, puedes cambiar el código de tus métodos privados o la estructura de tus datos sin cambiar la interfaz pública. Esto significa que los otros programadores que usan tu clase no tendrán que cambiar su código aunque tú modifiques el tuyo por completo.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

No es que la Interfaz Pública sea particularmente difícil de cambiar por ella misma, el problema es que cambiarla tiene más consecuencias que cambiar partes ocultas, porque todo aquello que dependa de alguna parte pública puede necesitar un cambio.

Diseñar la interfaz pública de una clase es casi como redactar un contrato legal: una vez que ambas partes lo firman y empiezan a trabajar, cualquier modificación posterior puede ser costosa, frustrante y generar conflictos.

 ### ¿Por qué diseñarla con cuidado?

  - **Es el "Contrato" de tu clase:** La interfaz pública es la promesa de lo que tu objeto puede hacer. Si expones demasiados métodos o los diseñas de forma confusa, obligas a los demás programadores (o a ti mismo en el futuro) a depender de detalles que quizás no deberían conocer.

  - **Mantiene el Control:** Una interfaz pequeña y bien definida te da libertad. Cuantos menos métodos públicos tengas, más libertad tendrás para cambiar el funcionamiento interno sin que nadie se dé cuenta.

  - **Previene el uso incorrecto:** Una interfaz clara guía al usuario de la clase. Si solo expones lo necesario, reduces las posibilidades de que alguien llame a un método en un orden incorrecto o con datos que corrompan el estado del objeto.

### ¿Es fácil cambiarla?

La respuesta corta es no. De hecho, es una de las tareas más difíciles en el mantenimiento de software por las siguientes razones:

 - **El Efecto Dominó (Breaking Changes):** Si cambias el nombre de un método público, eliminas un parámetro o cambias el tipo de dato que devuelve, romperás todo el código que use esa clase. Esto obliga a revisar y actualizar cada punto del programa donde se instanció ese objeto.

  - **Dependencias externas:** Si tu clase forma parte de una librería o API que utilizan otros equipos o empresas, cambiar la interfaz pública es un desastre. Podrías inutilizar miles de líneas de código ajeno.

  - **Coste de refactorización:** Mientras que cambiar algo privado es instantáneo y seguro, cambiar algo público requiere pruebas de regresión masivas para asegurar que no se ha roto la comunicación entre módulos.

  - **Regla de oro:** En POO, intenta que tu interfaz pública sea lo más pequeña posible. Es mucho más fácil hacer público un método privado más tarde que intentar convertir un método público en privado cuando ya se está usando en todo el proyecto.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son las reglas fundamentales (condiciones) que definen qué es un "estado válido" para un objeto. Son condiciones que deben cumplirse siempre, desde que el objeto termina de construirse hasta que se destruye. Nos referimos habitualmente al estado interno.

Veamos un desglose de este concepto crucial para la robustez del código:

 ------------------------------------------

  ### 1. ¿Qué son exactamente?

  Una invariante es una afirmación lógica sobre los atributos de una clase que **siempre es verdadera**. Si en algún momento la invariante se rompe, el objeto entra en un estado corrupto o inconsistente, lo que suele provocar errores graves en la aplicación.

  **Ejemplos clásicos:**

  * **En una clase `CuentaBancaria`:** "El saldo tiene que ser siempre >=0".
  * **En una clase `Fecha`:** "El día siempre debe estar entre 1 y el máximo de días del mes actual (ej. 1-31)".
  * **En una clase `Triángulo`:** "La suma de sus ángulos internos siempre debe ser 180°".

  * --> También puede ser importante el tipo de dato que pasas (no podemos pasar un float para la fecha, hay que implementar manualmente reglas de la invariante de clase)

 ------------------------------------------

  ### 2. ¿Cómo ayuda la ocultación de información?

  La ocultación de información (usar atributos `private`) es el mecanismo de defensa que permite que las invariantes existan. Sin ella, las invariantes serían simples "sugerencias" que cualquiera podría romper.Es decir, la ocultación de información permite saber que el estado de los atributos serán siempre válidos pues se pueden hacer comprobaciones internas de condiciones, así desde el exterior no se modificarán los valores para incumplir la invariable de clase.

  * **Control de acceso (El "Portero"):** Si los atributos son públicos, cualquier parte del programa puede modificarlos y romper la regla. Al hacerlos privados, obligas a que todo cambio pase por métodos públicos (como un `setFecha` o `retirarDinero`).
  * **Validación centralizada:** Dentro de esos métodos, puedes escribir código de validación. Si alguien intenta poner un valor que rompería la invariante, el método lo rechaza (lanza una excepción o ignora el cambio).
  * **Garantía de consistencia:** El objeto se vuelve responsable de su propio estado. No tiene que "confiar" en que los demás programadores usen bien sus datos; él mismo se asegura de que sus reglas se cumplan.

  > **En resumen:** La **invariante** es la regla y la **ocultación** es el muro que impide que alguien se salte esa regla desde fuera.

 ------------------------------------------

  ### Ejemplo práctico

  Véase una clase `Fraccion`. Su **invariante** es que el denominador nunca puede ser cero ().

  1. **Sin ocultación:** Alguien hace `miFraccion.denominador = 0;`. El objeto se rompe y la siguiente operación matemática colapsará el programa.
  2. **Con ocultación:** El atributo es `private`. Si alguien intenta llamar a `miFraccion.setDenominador(0);`, el método interno detecta el error, lo bloquea y la invariante se mantiene a salvo.

  ¿Te gustaría que pasemos a ver el concepto de **Herencia** o prefieres profundizar en los **Constructores** y cómo inicializan estas invariantes?


  ```java
  package encapsulacion;

  class Punto {
    private double x; //privado, no se pueden cambiar desde fuera
    private double y;

      /*public*/ Punto (double x, double y) { //si usamos todo dentro del mismo paquete, es mejor que no sea un método público
        this.x = x; 
        this.y = y;
      }

      double distanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
      }

    //Interfaz pública: el constructor y el método de distancia
    // * Punto
    // * distanciaAOrigen

  }
  ```
  ```java
  package encapsulacion;

  public EjercicioEncapsulacion {
    public static void main(String[] args) {
      Punto p = new Punto (4.6,5.6);
      System.out.println("Tu resultado es: ");
      System.out.print(p.distanciaAOrigen());
    }
  }
  ```

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

* **private -->** miembros solamente accesibles desde el código de la propia clase.
* **public -->** miembros accesibles desde cualquier código de otras clases.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

* `public` se puede aplicar a **clases** y **miembros**.
  
* `private` se puede aplicar a clases **internas** (no puedes hacer `private class Punto {...}`) y **miembros** (métodos y atributos).

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

A parte de `public` y `private`, existen 2 "grises":
  - `protected`: miembros accesibles desde subclases (lo veremos en el tema de **herencia**).
  
  - sin modificador/`package private`: miembros accesibles por otras clases **del mismo paquete** (aquellos miembros de una clase a los que no se les pone `public` o `private` son de este tipo).

* --> Cuanto menos privado es un miembro, más consecuencias conlleva cambiarlo (más potenciales usuarios).
* **REGLA DE ORO:** TODO DEBE SER `private` HASTA QUE SE DEMUESTRE LO CONTRARIO. 
  
## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

```java
  package encapsulacion;

  class Punto {
    private double x; 
    private double y;

      Punto (double x, double y) {
        this.x = x; 
        this.y = y;
      }

      double distanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
      }

    double distanciaA(Punto otro) {
      double dx = this.x - otro.x;
      double dy = this.y - otro.y;

      return Math.sqrt(dx*dx + dy*dy);
    }

  }
  ```

  --> Lo de `public` y `private` va más para el momento de programar (proteje al programa de otros programadores, que pueden cambiar algo accidentalmente), en tiempo de ejecución podré acceder a ambos puntos. La respuesta correcta es la *a*. Esto reduce los bugs.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

  * Los métodos de acceso (`getter`) y modificación de atributos (`setter`) sirven para recuperar y editar atributos de manera segura.

  * Estos métodos permiten controlar el acceso a los atributos privados de una clase. El "getter" devuelve el valor de un atributo, mientras que el "setter" permite modificarlo, generalmente realizando comprobaciones para mantener la validez del estado interno del objeto (por ejemplo, no permitir valores negativos en una edad). Así, se protege la integridad de los datos y se facilita la implementación de invariantes de clase. En Java, la convención es nombrar los getters como `getAtributo()` y los setters como `setAtributo(valor)`.

  * Por ejemplo, para la clase `Punto`, se podrían añadir los siguientes métodos:

  ```java
  public double getX() { //siendo X un atributo; por convención de nombre se llaman get + Atributo()
    return this.x;
  }

  public void setX(double x) { //siendo X un atributo; por convención de nombre se llaman set + Atributo()
    this.x = x;
  }
  ```

  El uso de getters y setters es una práctica habitual en la programación orientada a objetos para mantener la encapsulación y controlar cómo se accede y modifica el estado de los objetos.En `java` hace falta escribirlos manualmente (aunque haya ayudas de IDE que los generan automáticamente), en algunos lenguajes no.

  **OJO**, por muy comunes que sean, no se deberían hacer sin pensar, no todos los atributos necesitan `getter`, algunos tienen un uso perfectamente válido y exclusivo de manera interna. AÚN MÁS CUIDADO CON EL `setter`, A PARTE DE AMPLIAR LA INTERFAZ PÚBLICA (como los `getter`s), TAMBIÉN VUELVE UNA CLASE **INMUTABLE** **MUTABLE**.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No nos referimos a un tema de ciberseguridad si no a la reducción de posibles errores en la programación.

Hay una librería estándar de `java` que sirve para modificar todo (**exclusivamente útil para casos extremos**), por lo que escoger si un método es privado no sirve para más que para guiar a los programadores.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

* Los **miembros de clase** están asociados a la clase, compartidos por todas las instancias de esta. Es decir, si yo tuviera un atributo de clase, su valor es compartido para todas las instancias de una clase (y sólo ocuparía un valor en memoria, no habría copias). Por extensión, estos miembros no tienen `this.`, pues no hay diferencias en este atributo/método en ninguna instancia. Se pueden ocultar (se les puede poner `private` por ejemplo).

* Los **miembros de instancia** están asociados a una instancia concreta. Cada `x` e `y` de una clase `Punto` deben ser distintos.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

* Un constructor privado sólo puede ser accedido por miembros de la misma clase (es decir, un `private Punto(double x, double y) {...}`no puede ser llamado desde un `main.java` (asumiendo que están en dos archivos distintos)), de modo que los constructores privados valen sólo si/para:

  - **Quiero crear objetos solamente a través de "métodos factoría".**
  - **La clase sólo tiene miembros clase.**
  
    *Por ejemplo, una clase de utilidad matemática podría tener únicamente métodos y atributos estáticos, sin necesidad de instanciar objetos. En Java, esto se logra usando el modificador `static`. Un ejemplo típico sería:*

    ```java
    public class Matematicas {
      public static final double PI = 3.1415926535;

      public static double cuadrado(double x) {
        return x * x;
      }
    }
    ```

    *En este caso, `PI` y `cuadrado` son miembros de clase y se accede a ellos como `Matematicas.PI` y `Matematicas.cuadrado(5)`, sin crear instancias de `Matematicas`.*

  - **Controlar el número de instancias que se crean (si oculto el constructor y uso un método público que accede al constructor privado puede emplear un contador para gestionar esto).**

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

--> Se indica con static.

Si yo quiero conocer el valor máximo que se ha creado para `x` y cual para `y` de todas las instancias, habría que crear métodos adicionales. Con este ejemplo podemos llevar la cuenta de cual ha sido la variable más alta hasta el momento, vemos que la clase de por sí tiene este valor y por extensión es compartida para todas las instancias.

```java
  package encapsulacion;

  class Punto {
    private double x; 
    private double y;
    private static double MAX_X = Double.NEGATIVE_INFINITY;
    private static double MAX_Y = Double.NEGATIVE_INFINITY;

      Punto (double x, double y) {
        this.x = x; 
        this.y = y;

        if (x>MAX_X) {
          MAX_X = x;
        }

        if (y>MAX_Y) {
          MAX_Y = y;
        }
      }

      static double getMaxX() {
        return MAX_X; //static, no necesito un this.
      }

      static double getMaxY() {
        return MAX_Y; //static, no necesito un this.
      }

      double distanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
      }

    double distanciaA(Punto otro) {
      double dx = this.x - otro.x;
      double dy = this.y - otro.y;

      return Math.sqrt(dx*dx + dy*dy);
    }

  }
  ```

  ```java
  package encapsulacion;

  public EjercicioEncapsulacion {
    public static void main(String[] args) {
      Punto p1 = new Punto (4.6,5.6);
      Punto p2 = new Punto (50.6 ,5.6);
      Punto p3 = new Punto (4.6,80.2);
      System.out.println("El máximo X de todo tu programa es: " + Punto.getMaxX()) //50.6
      System.out.println("El máximo Y de todo tu programa es: " + Punto.getMaxY()) //80.2
      
    }
  }
  ```

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

Son métodos siempre `static` pues son métodos que generan otros métodos, esto es lógico dado que no tengo instancia antes de usarlo.

A veces podemos inclusive omitir el constructor tradicional y depender sólo de métodos factoría.

```java
static Punto nuevoPuntoConRedondeo(double x, double y) {
  return new Punto(Math.round(x), Math.round(y));
}
```
```java
  public EjercicioEncapsulacion {
    public static void main(String[] args) {
      Punto p1 = new Punto (4.6,5.6);
      Punto p2 = new nuevoPuntoConRedondeo(50.6 ,5.6);
      Punto p3 = new nuevoPuntoConRedondeo(4.6,80.2);
      System.out.println("El máximo X de todo tu programa es: " + Punto.getMaxX()) //51.0
      System.out.println("El máximo Y de todo tu programa es: " + Punto.getMaxY()) //80.0
      
    }
  }
```

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

```java
  package encapsulacion;

  class Punto {
    /* Quitamos estos dos atributos privados, NO AFECTA AL EXTERIOR
    private double x; 
    private double y;
    */

    private double[] coordenadas = new double[2]; //nuevo array PRIVADO de 2 doubles

    private static double MAX_X = Double.NEGATIVE_INFINITY;
    private static double MAX_Y = Double.NEGATIVE_INFINITY;

      Punto (double x, double y) { 
        this.coordenadas[0] = x; 
        this.coordenadas[1] = y;
        //usar el array ha servido para guardar los datos

        if (x>MAX_X) {
          MAX_X = x;
        }

        if (y>MAX_Y) {
          MAX_Y = y;
        }
      }

      double getX() {
        return this.coordenadas[0];
      }

      double getY() {
        return this.coordenadas[1];
      }

      static double getMaxX() {
        return MAX_X; //static, no necesito un this.
      }

      static double getMaxY() {
        return MAX_Y; //static, no necesito un this.
      }

      double distanciaAOrigen() {
        return Math.sqrt(this.getX() * this.getX() + this.getY() * this.getY());
      }

      static Punto nuevoPuntoConRedondeo(double x, double y) { //no me afectan los cambios pues llamo al propio constructor, ventaja de usar un método público con acceso a datos privados
      return new Punto(Math.round(getX()), Math.round(getY()));
      }

    double distanciaA(Punto otro) {
      double dx = this.getX() - otro.getX();
      double dy = this.getY() - otro.getY();

      return Math.sqrt(dx*dx + dy*dy);
    }
  //usar estos getX() y getY() permite que si quiero cambiar como funciona la obtención de las coordenadas, no tenga que variar como funcionan el resto de métodos.
  }
  ```

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No, porque perdemos la posibilidad de cambiarlo y garantizar invariantes de clase, es decir, los getters y setters permiten verificarlas antes de retornar o modificar un miembro.

```java
Punto(double x, double y) {
  if (x>100) { //Imagina que no quiero valores de x mayores a 100
    System.out.println("Value too big");
    return
  } else {
    //....
  }
}
```
```java
double getX() {
  return this.x; //los double son primitivos, lo que devuelvo en este caso es, por extensión, una copia. Me despreocupo de este valor, que hagan lo que quieran, no cambiará la instancia de la clase.
}
```java
double getNombre() {
  return this.nombre; //los string no son primitivos (es de tipo objeto), lo que devuelvo en este caso es, por extensión, una copia de la referencia. El string no se ha duplicado, sólo se garantiza que desde fuera no se podría cambiar si el string es inmutable (¡¡por suerte lo es!!), con todos sus atributos internos siendo privados. De ser un miembro mutable, lo usual es generar una copia para devolverla.
}

```
## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
