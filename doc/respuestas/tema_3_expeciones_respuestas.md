# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

La idea de las excepciones es que se vea explícito y claro, evita código "mierdoso" y así en vez de propagar excepciones de forma liosa, esta sigue subiendo la cadena hasta llegar a un punto donde el programa sabe que hacer.

## Opción A: pasar un valor concreto:
```C 
//por ejemplo, quiero hacer raíces cuadradas solamente de números negativos
float raiz(float num) {
  if (num < 0.0) {
    return -1.0; //devuelvo un valor especial, esto significa error
  } else {
    return sqrt(num); //funcionamiento normal
  } 
}
```
```C 
int main() {
  float num = leerTeclado();
  float resultado = raiz(num);
  if (num != -1.0) { //si todo va bien hago el camino normal
    fprint("Raíz: " + resultado);
  } else {
    fprint("No se pudo calcular, raíz negativa");
  }
}
```
## Opción B: pasar un parámetro por referencia para devolver el error allí:
```C 
//por ejemplo, quiero hacer raíces cuadradas solamente de números negativos
float raiz(float num, int *error) {
  if (num < 0.0) {
    *error = 1; //digo que error vale 1, hay fallo
    return -1; //ahora un valor cualquiera, no es lo relevante
  } else {
    *error = 0; //digo que error vale 1, todo OK
    return sqrt(num); //funcionamiento normal
  }
}
```
```C 
int main() {
  int error = 1;
  float num = leerTeclado();
  float resultado = raiz(num);
  if (*error != 1) { //si todo va bien hago el camino normal
    fprint("Raíz: " + resultado);
  } else {
    fprint("No se pudo calcular, raíz negativa");
  }
}
```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es una situación atípica, generalmente errores de entrada de usuario (de validación), de programación (código errado, falla) o de entorno (E/S; es decir, no hay internet, no tiene permisos de escritura sobre archivos que usa...). Estas situaciones está contempladas como posible error, no como condición normal, por extensión se busca lidiar con estas situaciones para volver el programa estable nuevamente.

--> Usamos `try {}`, `throw ()` y `catch()`; el código posterior al throw de un método no se ejecutará. Mientras que sin excepciones se ejecuta siempre la misma línea (y vuelvo los programas una fiesta de ifs y fáciles de romper), las excepciones salen del flujo normal.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

```java
class Calculadora {
  static float raiz(float num) {
    if (num < 0.0) {
      throw new IlegalArgumentExpection("Número negativo"); //son clases especialmente creadas para ser lanzables, hay varios en la librería de java. Este sirve para señalar que no me gusta el parámetro que me pasan. Al crearla, el constructor sobrecargado permite informar adecuadamente.
    }
    return Math.sqrt(num); //como la excepción rompe el flujo normal, no hace falta hacer un else. Si falla no llega a este paso (y los throw se consideran salidas válidas).
  }
}
```
```java
//sin lógica de excepción específica
class App {
  public static void main(String[] args) {
    float num = leerDeTeclado();
    float resultado = Calculadora.raiz(num);
    System.out.println("Raíz: " + resultado); //ya antes de implementar un tratamiento específico para el error, si al fallar devuelve una excepción se romperá el flujo y nos iremos a otra parte. No se imprimirá un valor ilegítimo.
  }
}

//con lógica de excepción específica
class App {
  public static void main(String[] args) {
    float num = leerDeTeclado();
    try {
      float resultado = Calculadora.raiz(num);
      System.out.println("Raíz: " + resultado);

    } catch (IlegalArgumentExpection e) {
      System.out.println("No se pudo calcular");
      System.out.println("Problema con: " + e);
    }
  }
}
```
--> Al implementar una función puedo lanzarla, al llamar a funciones puedo capturarla si quiero.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción es generarla, controlarla y capturarla es emplearla y propagarse en este contexto es su movimiento por el programa. Son muy importantes para mantener el funcionamiento adecuado del software.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación natural quiere decir que si no pongo `try() catch()` todos los métodos se paran de forma vertical (la que llama -> la que llama a la que llama -> la que llama a la que llama la que llama...) así hasta encontrar un punto seguro.

Al salir una excepción puede ser que, por ejemplo, el usuario metiera datos inválidos (por ejemplo un bucle while con try catch).

Si sale una excepción en pantalla es que el programador no la ha tenido en cuenta adecuadamente, pero es mejor que el programa se acabe a que funcione inadecuadamente.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En java sí, en la mayoría de lenguajes orientados a objetos también. Esto permite crear excepciones personalizadas, con su propia lógica típica dentro de cada una. Dentro de cada excepción hay información única de cada error. Es habitual, por legibilidad y comodidad, ir creando excepciones personalizados para programas grandes.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Relacionado con la encapsulación; 3 elementos tiene todo objeto Excepción:
  1. Un mensaje (definido por el programador).
  2. Una traza de llamadas guardada como una array del stack (da contexto a como se llegó al error [La excepción sucedió en raíz, que viene del método hipotenusa, que viene del método calculadora...]).
  3.  Opcionalmente, otra excepción causa (obtenida con un `getCause()`).

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, en Java se pueden tener múltiples bloques `catch` asociados a un solo bloque `try`. Solo se ejecuta **un** bloque `catch`: el primero que coincida con el tipo de excepción lanzada (siguiendo el orden de herencia de clases, de más específica a más general). Es decir, se va declarando por orden de declaración del `catch`, el primero que encaje será el que se ejecute.

* ==> **NOTA:** Para que el comportamiento sea el esperado, los `catch` se deben ordenar de más específico a más general.


```
|Exception|  
  ^
  | es un...
  |
|IOException|
  ^
  | es un...
  |
|AccesDeniedException|
```
### OK ! (intenta hacer la concreta y si no vale va a lo genérico)
```java
try {
  ......
  } catch (AccesDeniedException e) {
    ......
    }catch (IOEXception e2)
}
```
### MAL ! (AccesDeniedException es un IOException, no accederemos al caso concreto)

```java
try {
  ......
  } catch (IOException e) {
    ......
    }catch (AccesDeniedException e2)
}
```
### Sintaxis básica
La estructura permite varios `catch` consecutivos:

```java
try {
    // Código riesgoso
} catch (ExcepcionEspecifica1 e) {
    // Manejo específico
} catch (ExcepcionEspecifica2 e) {
    // Otro manejo
} catch (Exception e) {
    // Genérico (último)
}
```

Esto es útil cuando el `try` puede generar distintos tipos de errores. [datacamp](https://www.datacamp.com/es/doc/java/catch)

### ¿Cuántos se ejecutan?
- **Solo uno**: Java evalúa los `catch` secuencialmente hasta encontrar coincidencia.
- Si no hay match, la excepción propaga al bloque padre (escalado vertical).
- Nunca se ejecutan varios `catch` por `try`; el flujo salta al `catch` correcto y continúa. [codegym](https://codegym.cc/es/forum/1621)

### Corrección de razonamiento original
Tu idea de "tantos catch como try errados" es incorrecta: cada `try-catch` es independiente, y múltiples `catch` manejan **tipos** dentro del **mismo** `try`. El anidamiento (try dentro de try) es posible, pero cada nivel maneja su propia excepción.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

El bloque `finally` garantiza la ejecución de código crítico (como cerrar ficheros o liberar recursos) independientemente de si ocurre una excepción, se captura o el método termina normalmente. Se ejecuta **siempre** tras el `try` (y `catch` si existe), incluso si la excepción se propaga al llamador.

### Con `catch` (excepción capturada):
```java
public void ejemploConCatch() {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream("archivo.txt");
        // Simular lectura que falla
        int dato = fis.readAllBytes(); // Puede lanzar IOException
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
        throw e; // Propaga la excepción
    } finally { //bloque opcional asociado con el `try`, se ejecutará siempre que se haya entrado en el `try`
        System.out.println("Cerrando fichero...");
        if (fis != null) {
            try { fis.close(); } catch (IOException ignored) {}
        }
    }
}
```
El `finally` cierra el fichero **antes** de que la `IOException` suba al llamador.
### Sin `catch` (excepción propaga directamente):
```java
public void ejemploSinCatch() {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream("archivo.txt");
        int dato = fis.read(); // Lanza IOException
    } finally {
        System.out.println("Liberando recursos...");
        if (fis != null) {
            try { fis.close(); } catch (IOException ignored) {}
        }
    }
    // La IOException propaga aquí, pero recursos ya cerrados
}
```
`finally` se ejecuta antes de propagar la excepción, asegurando limpieza. 

### Nota moderna: `try-with-resources`
Para Java 7+, se recomienda usar esto (automático, prefiere sobre `finally` manual):
```java
try (FileInputStream fis = new FileInputStream("archivo.txt")) {
    fis.read();
} // Se cierra automáticamente, incluso con excepción
```
Más limpio y garantiza cierre vía `AutoCloseable`.

En definitiva, el `finally` sirve para permitir una funcionamiento seguro del programa aunque todo "se vaya al garete". **Se ejecuta incluso si hay un `return` dentro del `try`**: el `finally` se ejecutará antes de que el método retorne, asegurando la limpieza aunque se salga del método.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, como se explicó en la respuesta anterior.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException` (`RuntimeException` es jerga de `java`)? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, las **excepciones controladas (checked)** son clases derivadas de `Exception` (pero **no** de `RuntimeException`) y el compilador **obliga** a declararlas con `throws` o capturarlas con `try‑catch`. Las **excepciones no controladas (unchecked)** son subclases de `RuntimeException` (o de `Error`) y el compilador **no exige** que se manejen, aunque puedes capturarlas si quieres. 

Al crear tu propia excepción, decides si es controlada o no controlada dependiendo de si extiendes `Exception` (controlada) o `RuntimeException` (no controlada).

Vaya, que hay tipos de excepciones diferentes para las 2 familias de errores en `java`.

### Rol de `RuntimeException`

`RuntimeException` es la clase base de las excepciones no controladas. Representa normalmente errores de **lógica de programa** (índices fuera de rango, referencias nulas, cast incorrecto…), no condiciones externas “recuperables”. Al heredar de `RuntimeException`, cualquier excepción que extienda esta clase se comporta como no controlada: no se exige manejo explícito en el bytecode ni en la firma del método.

Básicamente, engloba todas las excepciones no controladas. Los dos grandes son las RuntimeException (errores de programación) y las Exception (errores de entorno), donde la más común es la `IOException`. Las primeras no se pueden controlar, las segundas sí.

***

### Ejemplos típicos de excepciones

**Controladas (checked):**  (Errores de entorno, siempre pueden ocurrir) 

- `IOException` → problemas de entrada/salida (ficheros, red).  
- `SQLException` → errores al ejecutar una consulta SQL.  
- `FileNotFoundException` → intentar abrir un fichero que no existe.  
- `ClassNotFoundException` → intentar cargar una clase por nombre que no está en el classpath. 

--> Como el programador no puede controlar que pasen, el compilador obliga al programador a poner try-catch o throws para asegurar que el programador es consciente de que pueden pasar y que se ha planteado qué hacer en ese caso (reintentar, mostrar mensaje, etc.).

**No controladas (unchecked / `RuntimeException`):**  (Errores de programación, internos)

--> No estamos obligados a poner try-catch o throws (los bugs deben estar ya corregidos)

- `NullPointerException` → llamar método o atributo sobre un objeto `null`.  
- `ArrayIndexOutOfBoundsException` → acceder fuera de los límites de un array.  
- `IllegalArgumentException` → argumento inválido (por ejemplo, saldo negativo).  
- `ClassCastException` → intentar castear un objeto a un tipo incompatible.
- 
--> Las excepciones no controladas verifican condiciones de funcionamiento del propio programa, si falta validación o algo por el estilo se pueden pasar valores incorrectos por programación errónea, estos checkeos.

### Si tú mismo defines una excepción, puedes controlar si es controlada o no:

```java
// Controlada: NO extiende RuntimeException
class MiExcepcionControlada extends Exception {
    public MiExcepcionControlada(String msg) { super(msg); }
}

// No controlada: sí extiende RuntimeException
class MiExcepcionNoControlada extends RuntimeException {
    public MiExcepcionNoControlada(String msg) { super(msg); }
}
```

***

### Situaciones donde se prefiere una excepción controlada

Se suele usar una **excepción controlada** cuando:

- El problema es **externo** al programa y **recuperable** (por ejemplo, fallo de red, disco lleno, base de datos caída).  
- Quieres **forzar** al cliente del código a pensar explícitamente qué hacer (reintentar, mostrar un mensaje, cambiar de servidor, etc.).  
- El error es **esperable** y forma parte del contrato público de la API (por ejemplo, métodos de lectura de ficheros, conexión a BBDD, envío de emails).  

Ejemplos de escenarios:

- Lectura/escritura de ficheros: puede haber I/O, permisos, disco lleno → usar `IOException` o subclases de `Exception`.  
- Consultas a base de datos: SQL puede fallar por sintaxis, tablas inaccesibles, etc. → `SQLException`.  
- Operaciones de red que pueden fallar por timeout o caída de conexión → `IOException` o excepciones propias que extiendan `Exception`.  
- Servicios que dependen de APIs externas: el cliente debe saber que puede fallar y decidir reintentar o fallback.  

***

### Situaciones donde se prefiere una excepción no controlada

Se suele usar una **excepción no controlada** cuando:

- El problema es un **error de programación** (argumento inválido, precondición violada, estado inconsistente).  
- No veas sentido en que el cliente lo “reabra” porque el bug debería corregirse en el código, no paliarse.  
- No quieres “obligar” a cada llamador a escribir `try‑catch` para errores meramente de uso incorrecto.  

Ejemplos de escenarios:

- Validación de parámetros: `null` donde no debe haberlo, número negativo donde debe ser positivo → `IllegalArgumentException`, `NullPointerException`.  
- Índices fuera de rango en colecciones propias: `ArrayIndexOutOfBoundsException` o una `RuntimeException` propia.  
- Invariantes de negocio: por ejemplo, restar saldo negativo, actualizar un objeto borrado, dividir por cero en lógica interna.  
- Errores de uso de API: el desarrollador llama mal a tu método (por ejemplo, sin haber inicializado algo primero) → `IllegalStateException`.  

En resumen, **controladas** = casos recuperables y esperados, **no controladas** = errores de lógica o de invocación que deberían corregirse en el código fuente.

**Algunos lenguajes** (como C#) no diferencian entre los tipos de excepciones, pero todos tienen una manera de lidiar con ellas. En Java, la distinción entre controladas y no controladas es una característica clave del lenguaje que influye en el diseño de APIs y en la gestión de errores.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

`throws` != `throw` ; `throws` se emplea en la cabecera de un método para indicar que puede lanzar una **excepción controlada*.

definir algo como `public String leerFichero (Path p) throws IOException {...}` avisa al compilador de cómo tratar el método, es muy importante. El compilador nos obliga a definir lo que pasa, si decidimos que "no puedo hacer nada para salvar el método", nos "desentendemos" de la excepción y la dejamos que se propague al llamador, que será el que decida qué hacer con ella (capturarla o volver a propagarla).

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

1. Sin finally:
```java
public String leerFichero (Path p) throws IOException { //la alternativa al throws sería usar un try{...} catch(IOException) {...} ; Pero es una función menos reutilizable y, potencialmente devuele un String vacío si no se retorna dentro del try. Por extensión tengo que empezar a devolver un "valor especial" de error (como null), pero es progresivamente más desordenado.
  
  String cadena = (String) Files.readAllBytes(p);
  return cadena;
}
```

2. Con finally (mejor, libera recursos):
```java
public String leerFichero (Path p) throws IOException { //recuerda que podemos poner varias excepciones separadas por comas, lo habitual es encontrar excepciones controladas
  try {
    String cadena = (String) Files.readAllBytes(p);
    return cadena;
  } finally {
    System.out.println("Liberando recursos..."); 
    cadena.close();
  }
}
```

--> Si le agrego a una función sin `throws` un `throws`, es muy probable que ya no compile porque el método que llama no sabe lidiar con la excepción. Poner `throws` tarde rompe retrocompatibilidad, y da pie a dejarlo de manera ineficiente como antes, o capturar la excepción del nulls y devolver una `RuntimeException` en vez de una `IOException` (que es lo que realmente se ha producido). Por eso es importante planificar bien el diseño de excepciones desde el principio.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Poder se puede, el compilador no obliga a hacer `try-catch` o declarar `throws` en el código llamar (una especie de `throws` placebo). Si se hace, suele ser por propósitos de documentación (hacer el método más legible).

Aún así, no es una práctica común ni recomendada, ya que las excepciones no controladas se asumen como errores de programación que deberían corregirse, no como condiciones esperables que el llamador deba manejar. En general, si un método lanza una `RuntimeException`, se espera que el programador corrija el código para evitar esa situación, en lugar de capturarla.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

* No controladas --> errores de programación, condiciones que deberían corregirse en el código (argumentos inválidos, estado inconsistente). No se espera que el cliente las maneje, sino que corrija el bug.

* Controladas --> errores de entorno o condiciones recuperables (fallos de I/O, red, base de datos). Se espera que el cliente las maneje explícitamente (reintentar, mostrar mensaje, fallback).

-------

* No hay ambas opciones en todos los lenguajes, de usar una sóla, suele ser más común la opción de **no controladas (unchecked)**, porque no gustaba que el compilador fuera tan "pesado" obligando a manejar excepciones que el programador no quería manejar (por ejemplo, `NullPointerException` o `IllegalArgumentException`), lo que llevaba a un código lleno de `try-catch` innecesarios. Por eso, lenguajes como Python o JavaScript optan por un modelo de excepciones sin distinción entre controladas y no controladas, dejando al programador la responsabilidad de decidir cómo manejarlas.

* Rust hace un poco lo contrario:
  1. No controladas ==> Panic (no se pueden recuperar, es un error de lógica que debe corregirse).
  2. Controladas ==> Obliga a cambiar el tipo de retorno (puede devolver un result que sea por ejemplo   `String` y otro que es `Error`). Esta decisión se debe a la presunción de que va a fallar eventualmente de manera garantizada.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

* Sí, tiene sentido.
* Sí, se puede relanzar el mismo objeto excepción (sin crear una excepción nueva), tras, por ejemplo, haber ejecutado algo en el `catch` que me interesase (Es decir, no considero que lo que haga en el `catch` sea suficiente).

* --> Ejemplos: 
  1. **Lanzar la misma excepción:** 
    ```java
    try {
      ...
    } catch (IOException e) {
      ...
      throw e; //lanzo la misma que capturé, mismo mensaje, causa, etc...
    }
    ```

  2. **Lanzar una nueva excepción:** (la primera se pierde)
      ```java
      try {
        ...
      } catch (IOException e) {
        ...
        throw new NetFluxException("Error I/O"); //lanzo una nueva (sea estándar de java o no), es común crear excepciones típicas y propias para programas más grandes, suelen lanzarse así
      }
      ```
  
  3. **Respuesta en la pregunta 17...**

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Cuando una excepción es la causa de otra, significa que la primera (de bajo nivel) se ha producido durante el manejo de la segunda (de alto nivel). Esto se puede lograr usando el constructor de la nueva excepción que acepta una causa (`Throwable cause`), lo que permite encadenar excepciones y preservar la información original del error.

3. **Causa:** (La excepción causante de otra, en este caso `e`)
      ```java
      try {
        ...
      } catch (IOException e) {
        ...
        throw new NetFluxException("Error I/O", e); //le paso como parámetro la excepción original, así la nueva excepción tiene toda la información de la original (mensaje, stack trace, etc.) como causa. Esto es útil para no perder el contexto del error original.
      }
      ```
