
# Tema 5. Polimorfismo

## 1. y 2. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

**Objetivo:** Extender programas de la forma más sencilla y segura, por el que las modificaciones vienen principalmente añadiendo nuevas clases, antes que tocando el código de las clases existentes.

**Mecanismos:** En java tenemos la **sobreescritura de métodos** en `Clases Abstractas` con sus métodos abstractos, además de las `Interfaces`, que son Clases Abstractas sin estado.

Entonces, la sobreescritura es el "desheredar" ligero, sobreescribimos un método heredado o externo por uno nuevo, de modo que no rompemos funcionamientos anteriores.

```java
Soldado s = new Artillero();
s.saludar();
/*
- Si `.saludar()` existe tanto en Soldado cono en Artillero, un lenguaje con ligadura dinámica como Java llamará al método del propio objeto que somos (es decir, como `Artillero` es un `Soldado`, por extensión se llamará al saludar del `Artillero`).
- Soldado es un supertipo de `Artillero`, `Artillero` un subtipo de `Soldado`.
- La ligadura tardía posibilita el polimorfismo. */
```

En java la ligadura tardía se hace constantemente con los métodos no estáticos. En C++ los métodos de las clases no se pueden sobreescribir al menos que lo identifiques explícitamente como virtual. El polimorfismo sacrifica algo de rendimiento, en java se puede prohibir el polimorfismo para un parámetro/método empleando la palabra clave `virtual`.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

## 3. y 4.

* Zapador:
```java
  public class Zapador extends Soldado {
    public void saludar() {
      super.saluda(); //primero hace el saludo normal llamando a "Soldado".
      System.out.println("Soy un Zapador!"); //ahora agrega su parte personalizada
    }
  }
```

* Soldado:
```java
public class Soldado {
  public void saludar() {
    System.out.println("Hola!");
  }
}
```

* Artillero:
```java
public class Artillero extends Soldado {

}
```

* PruebaPolimorfismo:
```java
public class Pruebapolimorfismo {

  public static void pasarRevista(soldado[] soldados) {
    for (Soldado s : soldados) {
      s.saludar();
    }
  }

  public static void main(String[] args) {
    Soldado[] soldados = new Soldado[2];
    soldados[0] = new Soldado();
    soldados[1] = new Artillero();
    soldados[2] = new Zapador();

    pasarRevista(soldados);
  }
}
```

--> Artillero saluda como `Soldado` (¡sin código!) y `Zapador` saluda diferente (¡sobreescrito!)


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

El overloading consiste en meter varias versiones de un mismo método (a través de ligadura estática) en una misma clase, siendo capaz de diferenciar a través del tipo y/o número de parámetros.

El overriding es más estricto, el método de la subclase debe mantener los mismos parámetros y nombre. Véase el ejercicio anterior. Solamente podemos cambiar el tipo de retorno, y eso si escogemos un subtipo (Es decir, como todo es un subtipo de `Object`, podríamos cambiar el subtipo a `String`, esto no da problemas pues un tipo más concreto sigue perteneciendo al esperado).

`@Override` meramente verifica si se sobreescribe adecuadamente, si no lo pones puede funcionar igual pero dar errores en funcionamiento (pensemos en faltas de ortografía, `saluda()` vs `saludar()`).


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es un tipo especial de clase con 0 o varios métodos abstractos. No se pueden inicializar pues suelen tener métodos abstractos (con cabecera pero sin código), valiendo entonces como base para otras clases pero no pudiendo construirse (es decir, no puedes hacerle `new`).

* Zapador:
```java
  public class Zapador extends Soldado {
    public void saludar() {
      super.saluda(); //primero hace el saludo normal llamando a "Soldado".
      System.out.println("Soy un Zapador!"); //ahora agrega su parte personalizada
    }

    public void atacar() {
    System.out.println("*Tengo que saber atacar aunque no lo haga porque si no no compilo muajajaja*");
  }
  }
```

* Soldado:
```java
public abstract class Soldado { //no se pueden hacer new Soldado ya
  public void saludar() {
    System.out.println("Hola!");
  }
  public abstract void atacar(); //método sin cuerpo, obligando a zZapador y a Artillero a implementar estos métodos.
}
```

* Artillero:
```java
public class Artillero extends Soldado { //si me declaro como clase abstracta también podría no implementarlo, pero mi artillero requiere saber .atacar()
  @Override
  public void atacar() {
    System.out.println("*Ataca épicamente*");
  }

}
```

* PruebaPolimorfismo:
```java
public class Pruebapolimorfismo {

  public static void pasarRevista(soldado[] soldados) {
    for (Soldado s : soldados) {
      s.saludar();
    }
  }

  public static void main(String[] args) {
    Soldado[] soldados = new Soldado[1];
    //soldados[0] = new Soldado();
    soldados[0] = new Artillero();
    soldados[0].atacar();
    soldados[1] = new Zapador();

    pasarRevista(soldados);
  }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

-> `final` en una clase prohibe herencia.
-> `final` en un método prohibe sobreescribir.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las interfaces son un mecanismo en parte similar a las clases abstractas pero:
  1. No tienen estado (no hay atributos).
  2. Todos los métodos tiene que ser abstractos (sin código). 
  3. Una clase puede implementar varias interfaces (existe una herencia múltiple).
   
--> Todo el que implemente la interfaz es un A, sea la interfaz creada por A.

* A partir de Java 8 ya se permite dar códigos en los métodos de la interfaz con tal de usar la palabra clave `default`, pero siguen limitadas en aspectos como estados.

Aún así, pensamos en la interfaz como una clase abstracta muy pura, cuanto menos especificada por dentro mejor.

Ejemplo: `List` con `ArrayList` y `LinkedList`.

De esta manera, se podría usar de `EntradaSalida` un archivo, escribir en teminal u otra idea diferente.

* **Ejemplo:**

```java
public interface EntradaSalida {
  public String leerEntrada();
  public void escribirEnSalida(String texto);
}
```

```java
public class TecladoPantalla implements EntradaSalida {

  private Scanner entrada = new Scanner(System.in);

  @Override
  public String leerEntrada() {
    return entrada.nextLine();
  }

  @Override
  public void escribirEnSalida(String texto) {
    System.out.println(texto);
  }
}
```

```java
public class UsandoFicheros implements EntradaSalida {

  private File entrada;
  private File salida;

  public UsandoFicheros(File entrada, File salida {
    this.entrada = entrada;
    this.salida = salida;
  })

  @Override
  public String leerEntrada() {
    //leo del File de `entrada` y devuelvo la línea
    return loqueseaqueretornexd;
  }

  @Override
  public void escribirEnSalida(String texto) {
    //escribo en el File `salida`
  }
}
```
Tenemos dos maneras de usar la misma interfaz cómodamente. El núcleo del programa que procese la información no tiene que saber la manera en la que se recopila la entrada o salida, pero sabe que hacer con ella. Al tener los métodos así separados puede simplemente llamar al módulo que procesa la entrada y salida, que ya decide por dónde se hace.

No hay un estado compartido necesariamente, cada forma de obtener datos funciona sin ser dependiente de las demás. Esto se considera bastante cómodo (claro, cuando no se necesita un estado común).

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

**Sería mejor hacer esto con una `interface`, pero poder se puede hacer lo pedido.**

-> Sólo podemos computar `Puntos` de una dimensión con otros de la misma.

Haremos una Linea que contiene 2 puntos abstractos. Tendremos una clase abstracta Punto con calcularDistancia() a otro Punto. Implementaremos Punto2D y Punto3D. A la linea no le importa.

```java
public abstract class Punto {
  public abstract double distanciaA(Punto otro);
}
```
```java
public class Punto2D extends Punto {
  private int x, y;
  
  public Punto2D(int x, int y) {
    this.x = x;
    this.y = y;
  }

  @Override
  public double distanciaA(Punto otro) {
    if (otro instanceof Punto2D) {
      Punto2D otro2d = (Punto2D) otro; //downcasting, esto especifica que otro es un Punto2D sin afectarle verdaderamente, así podemos sacarle el x y.
      return Math.sqrt(Math.pow(this.x - otro2d.x, 2) + Math.pow(this.y - otro2d.y , 2));
    } else throw new IllegalArgumentException("Puntos de diferente dimensionalidad");
  }
}
```
```java
public class Punto3D extends Punto {
  @Override
  public double distanciaA(Punto otro) {
    if (otro instanceof Punto3D) {
      Punto2D otro2d = (Punto3D) otro; //downcasting, esto especifica que otro es un Punto2D sin afectarle verdaderamente, así podemos sacarle el x y.
      return Math.sqrt(Math.pow(this.x - otro3d.x, 2) + Math.pow(this.y - otro3d.y , 2)) + Math.pow(this.z - otro3d.z , 2);
    } else throw new IllegalArgumentException("Puntos de diferente dimensionalidad");
  }
}
```
```java
class Linea { //vive abstraída
private Punto puntoInicial;
private Punto puntoFinal;

public Linea (Punto puntoInicial, Punto puntoFinal) {
  this.puntoInicial = puntoInicial;
  this puntoFinal = puntoFinal;
}
public double calcularLongitud() {
  return this.puntoInicial.distanciaA(this.puntoFinal);
}
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La **herencia de interfaces** en Java significa que una interfaz puede `extender` a otra(s) interfaz(es), heredando sus métodos (cabeceras). Sí existe **herencia múltiple de interfaces**: una interfaz puede `extends` varias interfaces a la vez (por ejemplo `interface C extends A, B { ... }`) y una clase puede `implementar` varias interfaces simultáneamente. Esto permite componer tipos por comportamiento sin heredar implementación de estado.

Ejemplo:

```java
public interface Fichero {
  String leerContenido() throws java.io.IOException;
}

public interface FicheroEscribible extends Fichero {
  void escribirContenido(String contenido) throws java.io.IOException;
  boolean eliminar() throws java.io.IOException;
}

// Implementación de ejemplo
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;

public class FicheroLocal implements FicheroEscribible {
  private final Path ruta;
  public FicheroLocal(Path ruta) { this.ruta = ruta; }

  @Override
  public String leerContenido() throws java.io.IOException {
    return Files.readString(ruta);
  }

  @Override
  public void escribirContenido(String contenido) throws java.io.IOException {
    Files.writeString(ruta, contenido, StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
  }

  @Override
  public boolean eliminar() throws java.io.IOException {
    return Files.deleteIfExists(ruta);
  }
}
```

Notas breves: desde Java 8 las interfaces pueden declarar métodos `default` con implementación; si hay conflictos (método `default` con la misma firma en varias interfaces) el compilador obliga a la clase implementadora a resolverlo mediante una implementación concreta o a especificar cuál usar. Las reglas de resolución (método de clase > default de interfaz, y especificidad entre interfaces) evitan los problemas clásicos de la herencia múltiple de implementación.
