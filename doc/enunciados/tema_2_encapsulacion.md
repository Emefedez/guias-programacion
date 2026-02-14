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

- **Encapsulación -->** Es el proceso de agrupar datos (atributos) y los métodos que operan sobre esos datos en una sola unidad o "caja" (la clase). Su objetivo es mantener todo lo relacionado con un objeto en un mismo lugar.

- **La Ocultación de información -->** Es la capacidad de restringir el acceso directo a los detalles internos de esa "caja". Busca separar la interfaz (lo que el objeto hace) de la implementación (cómo lo hace internamente).

- **En resumen-->** buscan que el mundo exterior solo interactúe con el objeto a través de una interfaz controlada (métodos públicos), impidiendo que se modifique el estado interno de forma arbitraria o errónea.


### En definitiva, estas son las principales ventajas:

**Protección de la integridad:** Evita que el código externo asigne valores inválidos a los atributos (por ejemplo, una "edad" negativa), ya que el acceso se filtra mediante métodos (getters/setters) que validan los datos.

**Facilidad de mantenimiento:** Puedes cambiar la lógica interna de un método o el tipo de una variable privada sin que el resto del programa se entere ni deje de funcionar.

**Reducción del acoplamiento:** Al depender solo de la interfaz pública, los diferentes módulos de un programa están menos "atados" entre sí, lo que facilita hacer cambios en uno sin romper los demás.

**Abstracción y simplicidad:** Como bien mencionaste, el usuario de la clase solo ve lo que necesita para trabajar, eliminando el ruido visual de la complejidad interna. 



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

El término interfaz pública hace referencia a todos los métodos de un objeto/clase a los que se puede acceder desde fuera. Requiere que sean clases públicas, si no sólo puedes acceder desde el mismo `.java`. Los métodos y atributos ocultos son aquellos que no salen en la interfaz pública.

Se podría decir entonces, que la interfaz pública es, fundamentalmente, la respuesta a la pregunta: "¿Qué puede hacer este objeto por mí?", sin importar cómo lo haga por dentro.

Si especificamos más respecto a la relación que hay entre el término interfaz pública y la ocultación de objetos, diríamos que esta se basa en la **separación de preocupaciones:**

- **Punto de contacto-->** La interfaz pública es la única vía permitida para interactuar con el objeto. Todo lo que no está en la interfaz pública (atributos privados, métodos auxiliares de cálculo, etc.) está "oculto".

- **Abstracción-->** La ocultación permite que la interfaz sea minimalista. Al esconder los detalles complejos, la interfaz pública se mantiene simple y fácil de usar.

- **Estabilidad del contrato-->** Al ocultar la implementación interna, puedes cambiar el código de tus métodos privados o la estructura de tus datos sin cambiar la interfaz pública. Esto significa que los otros programadores que usan tu clase no tendrán que cambiar su código aunque tú modifiques el tuyo por completo.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

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

Las **invariantes de clase** son las reglas fundamentales que definen qué es un "estado válido" para un objeto. Son condiciones que deben cumplirse siempre, desde que el objeto termina de construirse hasta que se destruye.

Veamos un desglose de este concepto crucial para la robustez del código:

---

### 1. ¿Qué son exactamente?

Una invariante es una afirmación lógica sobre los atributos de una clase que **siempre es verdadera**. Si en algún momento la invariante se rompe, el objeto entra en un estado corrupto o inconsistente, lo que suele provocar errores graves en la aplicación.

**Ejemplos clásicos:**

* **En una clase `CuentaBancaria`:** "El saldo nunca puede ser inferior al límite de crédito permitido".
* **En una clase `Fecha`:** "El día siempre debe estar entre 1 y el máximo de días del mes actual (ej. 1-31)".
* **En una clase `Triángulo`:** "La suma de sus ángulos internos siempre debe ser 180°".

---

### 2. ¿Cómo ayuda la ocultación de información?

La ocultación de información (usar atributos `private`) es el mecanismo de defensa que permite que las invariantes existan. Sin ella, las invariantes serían simples "sugerencias" que cualquiera podría romper.

* **Control de acceso (El "Portero"):** Si los atributos son públicos, cualquier parte del programa puede modificarlos y romper la regla. Al hacerlos privados, obligas a que todo cambio pase por métodos públicos (como un `setFecha` o `retirarDinero`).
* **Validación centralizada:** Dentro de esos métodos, puedes escribir código de validación. Si alguien intenta poner un valor que rompería la invariante, el método lo rechaza (lanza una excepción o ignora el cambio).
* **Garantía de consistencia:** El objeto se vuelve responsable de su propio estado. No tiene que "confiar" en que los demás programadores usen bien sus datos; él mismo se asegura de que sus reglas se cumplan.

> **En resumen:** La **invariante** es la regla y la **ocultación** es el muro que impide que alguien se salte esa regla desde fuera.

---

### Ejemplo práctico

Véase una clase `Fraccion`. Su **invariante** es que el denominador nunca puede ser cero ().

1. **Sin ocultación:** Alguien hace `miFraccion.denominador = 0;`. El objeto se rompe y la siguiente operación matemática colapsará el programa.
2. **Con ocultación:** El atributo es `private`. Si alguien intenta llamar a `miFraccion.setDenominador(0);`, el método interno detecta el error, lo bloquea y la invariante se mantiene a salvo.

¿Te gustaría que pasemos a ver el concepto de **Herencia** o prefieres profundizar en los **Constructores** y cómo inicializan estas invariantes?

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

Es necesario añadir un constructor, un método público alcanzable y getters que no acceden al atributo si los quiero a parte puesto que este se mantiene privado para imposibilitar su modificación no deseada.

## Ejemplo Completo de la Clase `Punto`

```java
public class Punto {
    // Atributos privados: Ocultación de información
    private double x;
    private double y;

    // Constructor: Permite inicializar el objeto
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método público: Parte de la interfaz
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(this.x*this.x + this.y*this.y);
    }

    // Métodos Getter: Permiten leer los datos sin acceder directamente al atributo
    public double getX() { return x; }
    public double getY() { return y; }
}

```


**--> ¿Cuál es la interfaz pública de la clase `Punto`?**

La **interfaz pública** es el conjunto de métodos y miembros que son accesibles desde fuera de la clase. Es el "contrato" que la clase ofrece al resto del programa para interactuar con ella.

En el ejemplo anterior, la interfaz pública consiste en:

* El **constructor**: `Punto(double x, double y)`.
* El **método**: `calcularDistanciaAOrigen()`.
* Los **métodos de acceso** (si se incluyen): `getX()` y `getY()`.

Los atributos `x` e `y` **no** forman parte de la interfaz pública porque están ocultos tras el modificador `private`.

---

**--> ¿Qué significa `public` y `private`?**

Estos términos se conocen como **modificadores de acceso** y definen el nivel de visibilidad de los componentes de una clase:

* **`public`**: Indica que el miembro (atributo o método) es accesible desde **cualquier otra clase** en cualquier paquete. Se usa para definir qué servicios ofrece el objeto al mundo exterior.
* **`private`**: Indica que el miembro solo es accesible **dentro de la propia clase**. Nadie fuera de las llaves `{ ... }` de la clase `Punto` puede ver o modificar `x` o `y` directamente.

> **Nota sobre la ocultación de información:** No privatizamos los atributos solo porque una función interna los use. Lo hacemos para proteger la **integridad de los datos**. Si `x` fuera `public`, cualquier otra parte del código podría cambiar su valor sin que la clase `Punto` se entere, lo que podría romper la lógica del objeto. Al ser `private`, la clase tiene el control total.



## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de acceso **`public`** y **`private`** se pueden aplicar a diferentes elementos del código para controlar su visibilidad. Sin embargo, las reglas cambian dependiendo de si hablamos de una clase en sí misma o de lo que hay dentro de ella.

Aquí tienes el desglose detallado:

### 1. A nivel de Miembros de Clase

Es el uso más común. Se aplican a todo lo que define el comportamiento y estado de un objeto:

* **Atributos (Variables de instancia o de clase):** Para proteger los datos (encapsulamiento).
* **Métodos:** Para definir qué acciones puede realizar el objeto desde fuera (`public`) o qué procesos internos necesita (`private`).
* **Constructores:** Determinan quién puede crear instancias de la clase. Un constructor `private` impide que se creen objetos fuera de la clase (común en el patrón *Singleton*).
* **Clases e Interfaces Anidadas:** Las clases que se definen dentro de otra clase sí pueden ser marcadas como `private`.

---

### 2. A nivel de Clase (Nivel Superior)

Para las clases e interfaces que definimos directamente en un archivo `.java`, las reglas son más estrictas:

* **`public`**: La clase es accesible desde cualquier otra clase de cualquier paquete.
* **`private`**: **No está permitido** para clases de nivel superior. Una clase de nivel superior solo puede ser `public` o tener acceso por defecto (*package-private*).

---

### Tabla Comparativa de Visibilidad
```md

| Modificador   |     Clase de nivel superior     | Miembros (Atributos, Métodos, etc.)                       |
|_______________|_________________________________|___________________________________________________________|
| **`public`**  |  Visible desde cualquier lugar. | Visible desde cualquier lugar donde la clase sea visible. |
| **`private`** |.       **No permitido**.        | Visible **solo** dentro de la clase donde se declaró.     |
```
---

### Resumen de Aplicación
```md

| Elemento                | ¿Puede ser `public`? | ¿Puede ser `private`? |
|_________________________|______________________|_______________________|
| Clases (Nivel superior) |           Sí         |           No          |
| Clases Anidadas         |           Sí         |           Sí          |
| Interfaces (Niv. sup.)  |           Sí         |           No          |
| Atributos / Campos      |           Sí         |           Sí          |
| Métodos                 |           Sí         |           Sí          |
| Constructores           |           Sí         |           Sí          |
```

> **Dato clave:** Si no pones ni `public` ni `private`, Java aplica el nivel de acceso por defecto (*default* o *package-private*), lo que significa que el elemento solo es visible para las clases que están en el mismo paquete.

¿Te gustaría ver un ejemplo de cómo un **constructor privado** puede ser útil en un programa real?

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


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
