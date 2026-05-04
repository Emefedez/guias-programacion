# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

La filosofía principal de los genéricos implica asegurarse de que todo tipo de errores deben aparecer en tiempo de compilación y no ejecución.

* La invarianza existe cuando dos tipos básicos tienen relación de subtipo (`Double -> Number -> Object`, `Integer -> Number -> Object` y `String -> Object`). Esto quiere decir que "`Double` es un tipo de `Number`, que es un tipo de `Object`.

¿`Object[]` puede ser padre de un `Number[]`?:
1. Sí: Covarianza.
2. No: Invarianza.
3. Al revés (_`Number es padre de Object[]`_): Contravarianza.

- Invarianza -> Genéricos por defecto.
- Covarianza -> Arrays primitivos.

Veamos...

```java
String[] misStrings = {"A","B"};
Object[] misObjetos = misStrings;
```

Esto puede parecer correcto, pero...

* En el Heap tenemos String[] con sus propios objetos String.
* en el Stack, tenemos unas referencias misStrings y misObjetos que apuntan a donde estamos en el heap.

```java
String[] misStrings = {"A","B"};
Object[] misObjetos = misStrings;
misObjetos[0] = new Empleado();
```

* Aquí se intentará agregar un `Empleado` al array de Strings del Heap, pero Java dará un `ArrayStoreException`, dado que meter algo que no sea un String en un array de Strings no funciona. 

* De hacerse con colecciones genéricas...
```java
List<String> misStrings = List.of{"A","B"};
List<Object> misObjetos = misStrings; //error de compilación
```

* Esto erra porque sería problemático permitir esto: 

```java
misObjetos.add(new Empleado()); //el problema es el mismo que antes, no se puede permitir porque la lista no está preparada para llevar Empleados dentro.
```


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Podemos "prometerle" con las `wildcard (?), extends y super` que sólo haremos operaciones seguras:

```java
List<? extends Number> // permite desde Number hacia abajo (aquello que viene de Number)
List<? super Number> // permite de Number hacia arriba (de donde viene Number)
```
* `List<? super String>` : 
    - Si llamo a métodos que devuelven un valor genérico (`get(0)`), devolverá `Object`. **[T -> Object]**
    - Si llamo a métodos que reciben el valor genérico (`add(T)`), reciben `String` _(Esto es seguro, un String cabe en una lista de Strings o de Objects)_. **[T -> String]**

* `List<? extends Number>`:
    - Si llamo a métodos que devuelven un valor genérico (`get(0)`), devolverá el `Number` concreto (sea el propio `Number`. `Integer`, `Double`...). **[T -> Number]**
    - Si llamo a métodos que reciben el valor genérico (`add(T)`), habrá un **error de compilación** _(No podemos saber si devolvemos un Integer a una Lista segura, o un Number a una segura, etc... Vaya, que no se pueden llamar genéricos)_. **[T -!> XXX]**

* Es decir, el *super* está pensado para métodos que *reciben* y **extends** para aquellos que **devuelven**.


* --> **REGLA NEMOTÉCNICA PECS** --> `Producer "extends", Consumer "Super"`.

"¿La lista la vas a usar como productor o consumidor? si en una función de suma empleamos get(i), necesitaremos emplear esos valores para la suma, somos productores." **[sumaTodos(List<? extends Number> nums)]** _Piensa que los todos los Integer son Number, pero no todos los Number son Integer. En este caso, estaría prohibido hacer `nums.add(n)`, podemos recorrer cualquier Number, pero no necesariamente añadirlo_.

"En añadeNumeros, si usamos add(n), estamos consumiendo un valor para calcular; somos consumidores".
**[añadeNumeros(List<? super Number> nums)]** _Si añadimos numbers, podemos también pasar algo por encima, pues requerimos que no esté por debajo de Number, no por encima. Hacemos un método más útil_.

* vaya, que **get() produce un valor que será consumido por add()**.