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

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


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

