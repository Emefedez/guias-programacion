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
  3.  Opcionalmente, otra excepción causa.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, en Java se pueden tener múltiples bloques `catch` asociados a un solo bloque `try`. Solo se ejecuta **un** bloque `catch`: el primero que coincida con el tipo de excepción lanzada (siguiendo el orden de herencia de clases, de más específica a más general).

## Sintaxis básica
La estructura permite varios `catch` consecutivos:

```
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

## ¿Cuántos se ejecutan?
- **Solo uno**: Java evalúa los `catch` secuencialmente hasta encontrar coincidencia.
- Si no hay match, la excepción propaga al bloque padre (escalado vertical).
- Nunca se ejecutan varios `catch` por `try`; el flujo salta al `catch` correcto y continúa. [codegym](https://codegym.cc/es/forum/1621)

## Corrección de razonamiento original
Tu idea de "tantos catch como try errados" es incorrecta: cada `try-catch` es independiente, y múltiples `catch` manejan **tipos** dentro del **mismo** `try`. El anidamiento (try dentro de try) es posible, pero cada nivel maneja su propia excepción.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

El bloque `finally` garantiza la ejecución de código crítico (como cerrar ficheros o liberar recursos) independientemente de si ocurre una excepción, se captura o el método termina normalmente. Se ejecuta **siempre** tras el `try` (y `catch` si existe), incluso si la excepción se propaga al llamador.

## Con `catch` (excepción capturada):
```java
public void ejemploConCatch() {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream("archivo.txt");
        // Simular lectura que falla
        int dato = fis.read(); // Puede lanzar IOException
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
        throw e; // Propaga la excepción
    } finally {
        System.out.println("Cerrando fichero...");
        if (fis != null) {
            try { fis.close(); } catch (IOException ignored) {}
        }
    }
}
```
El `finally` cierra el fichero **antes** de que la `IOException` suba al llamador.
## Sin `catch` (excepción propaga directamente):
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

## Nota moderna: `try-with-resources`
Para Java 7+, se recomienda usar esto (automático, prefiere sobre `finally` manual):
```java
try (FileInputStream fis = new FileInputStream("archivo.txt")) {
    fis.read();
} // Se cierra automáticamente, incluso con excepción
```
Más limpio y garantiza cierre vía `AutoCloseable`.

En definitiva, el `finally` sirve para permitir una salida segura del programa aunque todo "se vaya al garete", no tieme por que solucionar los problemas que dan pie al error, si no que limpia el camino para una ejecución adecuada la próxima vez que se emplee el programa.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, como se explicó en la respuesta anterior.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, las **excepciones controladas (checked)** son clases derivadas de `Exception` (pero **no** de `RuntimeException`) y el compilador **obliga** a declararlas con `throws` o capturarlas con `try‑catch`. Las **excepciones no controladas (unchecked)** son subclases de `RuntimeException` (o de `Error`) y el compilador **no exige** que se manejen, aunque puedes capturarlas si quieres. 

### Rol de `RuntimeException`

`RuntimeException` es la clase base de las excepciones no controladas. Representa normalmente errores de **lógica de programa** (índices fuera de rango, referencias nulas, cast incorrecto…), no condiciones externas “recuperables”. Al heredar de `RuntimeException`, cualquier excepción que extienda esta clase se comporta como no controlada: no se exige manejo explícito en el bytecode ni en la firma del método.

***

### Ejemplos típicos de excepciones

**Controladas (checked):**  
- `IOException` → problemas de entrada/salida (ficheros, red).  
- `SQLException` → errores al ejecutar una consulta SQL.  
- `FileNotFoundException` → intentar abrir un fichero que no existe.  
- `ClassNotFoundException` → intentar cargar una clase por nombre que no está en el classpath. 

**No controladas (unchecked / `RuntimeException`):**  
- `NullPointerException` → llamar método o atributo sobre un objeto `null`.  
- `ArrayIndexOutOfBoundsException` → acceder fuera de los límites de un array.  
- `IllegalArgumentException` → argumento inválido (por ejemplo, saldo negativo).  
- `ClassCastException` → intentar castear un objeto a un tipo incompatible.

Si tú mismo defines una excepción, puedes controlar si es controlada o no:

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

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

