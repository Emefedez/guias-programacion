<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se modela de forma natural con `struct`: una estructura grande contiene, como campos, otras estructuras más pequeñas (**hacemos `struct`s de `struct`s**).

En este caso, un `Punto` tiene dos coordenadas (`x`, `y`) y una `Linea` tiene dos puntos (`inicio`, `fin`). Esto expresa exactamente la idea de "línea tiene dos puntos" sin necesidad de orientación a objetos.

Para calcular la distancia entre dos puntos del plano se aplica la distancia euclídea: $d = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$. Después, la longitud de una línea puede definirse reutilizando esa misma operación sobre los dos puntos que contiene la `Linea`. Esta reutilización evita duplicar lógica y mantiene el código más claro.

```c (programación estructurada, las estructuras de datos van por un lado y las funciones por otro)
#include <stdio.h>
#include <math.h>

typedef struct {
	double x;
	double y;
} Punto;

typedef struct {
	Punto inicio;
	Punto fin;
} Linea;

double distanciaPuntos(Punto a, Punto b) {
	double dx = b.x - a.x;
	double dy = b.y - a.y;
	return sqrt(dx * dx + dy * dy);
}

double longitudLinea(Linea l) {
	return distanciaPuntos(l.inicio, l.fin);
}

int main(void) {

	Punto p1 = {0.0, 0.0};
	Punto p2 = {3.0, 4.0};
	Linea segmento = {p1, p2};

	printf("Distancia p1-p2: %.2f\n", distanciaPuntos(p1, p2));
	printf("Longitud del segmento: %.2f\n", longitudLinea(segmento));
	return 0;

}
```

```c (versión del profe)
struct Punto {
	double x;
	double y;
}

struct Linea {
	struct Punto p1;
	struct Punto p2;
}

double distancia(struct Punto p1, struct Punto p2) {
	double dx = p1.x - p2.x;
	double dy = p1.y - p2.y;
	return sqrt(dx * dx + dy * dy);
}

double longitud (struct Linea l) {
	return distancia(l.p1, l.p2);
}

int main(void) {
	
	Punto p1 = {0.0, 0.0};
	Punto p2 = {3.0, 4.0};
	Linea segmento = {p1, p2};

	printf("Distancia p1-p2: %.2f\n", distanciaPuntos(p1, p2));
	printf("Longitud del segmento: %.2f\n", longitudLinea(segmento));
	return 0;
}
```

Este diseño es correcto en C, pero no impide que el código cliente modifique libremente los campos de `Punto` o `Linea` en cualquier momento. Esa limitación es importante porque abre la puerta a estados no deseados; precisamente ahí es donde Java, con encapsulación e inmutabilidad, aporta una mejora clara.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, la misma idea se expresa mediante clases: `Linea` contiene dos objetos `Punto`. La diferencia clave frente a C es que puede protegerse el estado interno con encapsulación (`private`) y construir objetos inmutables declarando atributos `final` y sin métodos *setter*. Así, una vez creado un `Punto` o una `Linea`, su contenido no cambia.

`Punto` expone un comportamiento (`distanciaA`) y no sus detalles internos para modificación. `Linea`, por su parte, delega el cálculo de su longitud en los puntos que la forman. Además, el constructor de `Linea` valida que no se reciban referencias nulas, evitando errores en tiempo de ejecución y manteniendo una invariante básica del objeto: una línea siempre debe tener dos puntos válidos.

```java (profe, orientación a objetos)
class Punto {
	private double x;
	private double y;

	public Punto (double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double distancia(/*El primer punto no se usa, recordemos que usamos .this*/ struct Punto p2) {
		double dx = this.x - p2.x;
		double dy = this.y - p2.y;
		return sqrt(dx * dx + dy * dy);
	}
	//es necesario crear getters de los primitivos para que la clase Linea pueda acceder a las coordenadas de los puntos, pero no es necesario crear setters, porque queremos que los puntos sean inmutables
}

class Linea {
	private Punto p1;
	private Punto p2;

	public Linea (Punto p1, Punto p2) {
		this.p1 = p1;
		this.p2 = p2;
	}

	public double longitud() {
		return this.p1.distancia(this.p2);
	}
}

```

```java
public final class Punto {
	private final double x;
	private final double y;

	public Punto(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double getX() {
		return x;
	}

	public double getY() {
		return y;
	}

	public double distanciaA(Punto otro) {
		if (otro == null) {
			throw new IllegalArgumentException("El punto destino no puede ser null");
		}
		double dx = otro.x - this.x;
		double dy = otro.y - this.y;
		return Math.sqrt(dx * dx + dy * dy);
	}
}

public final class Linea {
	private final Punto inicio;
	private final Punto fin;

	public Linea(Punto inicio, Punto fin) {
		if (inicio == null || fin == null) {
			throw new IllegalArgumentException("Los puntos de la linea no pueden ser null");
		}
		this.inicio = inicio;
		this.fin = fin;
	}

	public Punto getInicio() {
		return inicio;
	}

	public Punto getFin() {
		return fin;
	}

	public double longitud() {
		return inicio.distanciaA(fin);
	}
}
```

Con este enfoque, se obtiene un primer ejemplo claro de composición orientada a objetos: `Linea` "tiene dos" `Punto`, y el estado queda protegido. Se gana seguridad y claridad de diseño, porque los cambios en coordenadas o extremos no pueden hacerse de forma arbitraria desde fuera de la clase.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Definición del profesor: 
**-->** La multiplicidad establece con cuántas instancias de tipo B se relaciona una instancia de tipo A. Como mínimo y como máximo, se puede ver en ambos sentidos: de A a B y de B a A.

**Ejemplo: Entre punto y línea (Emplearemos la notación UML)**
					 _______________                 									_______________
Clase:		|     Línea		  |				`(mín*máx)`					 2*2 |     Punto		 |
Atributos:|		p1: Punto   |--------------------------------|	  x: double  |
					|		p2: Punto   | Ninguna*'Infinitas'            |  	y: double  |
Métodos:  |			....   		|                                |			....   	 |

**-->** Una Linea se relaciona como mínimo con `2` y como máximo con `2`.
**-->** Un Punto se relaciona como mínimo con `0` y como máximo con `muchas` Lineas.


La multiplicidad indica cuántas instancias de una clase pueden o deben relacionarse con una instancia de otra clase. Es una restricción del modelo, no solo un detalle de implementación: ayuda a razonar sobre la validez de los objetos y evita diseños ambiguos. Se suele expresar con rangos como `1`, `0..1`, `0..*`, `1..*`, etc.

En el ejemplo propuesto, de `Linea` hacia `Punto`, la multiplicidad es `2` (o, si se prefiere notación de intervalo, `2..2`), porque cada línea está formada exactamente por dos puntos: inicio y fin. No existe, en este modelo concreto, una línea con un solo punto o con tres puntos.

En sentido inverso, de `Punto` hacia `Linea`, la multiplicidad típica es `0..*`: un punto puede no pertenecer a ninguna línea, o pertenecer a una, o a muchas líneas distintas (por ejemplo, un vértice compartido entre varios segmentos). Por tanto, la relación completa se resume como: `Linea -> Punto: 2` y `Punto -> Linea: 0..*`.

Expresar estas multiplicidades de forma explícita mejora el diseño desde el inicio, porque obliga a pensar en invariantes y casos límite. Además, prepara el terreno para implementar validaciones coherentes cuando la relación se vuelva más compleja en ejercicios posteriores.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

**Composición fuerte vs Composición débil**

Hace falta pensar más cuando queremos composición fuerte.

==> **Fuerte:** El objeto contenido vive en función del objeto que lo contiene. No existe antes, ni vive más allá. Por tanto, el ciclo de vida no es independiente.

|--------------------------------| Contenedor
. - - - - - - - |----------------| Contenido (no puede vivir más allá del Contenedor, su ciclo de vida es dependiente de este).

**Ejemplo:** Linea CREA sus Punto, los Punto **mueren** si muere Linea.

==> **Débil:** Los ciclos de vida son independientes.

**Ejemplo:** Linea CREA sus Punto, los Punto **no** mueren si muere Linea.

Evidentemente, como varias Lineas pueden compartir Punto, lo que tiene sentido (y lo que se hizo) para el programa de Punto y Linea es composición débil.

La composición propia suele dibujarse en UML con un rombo relleno antes de la línea, con un rombo vacío (agregación) o sin rombo (asociación) la composición débil.

El rombo quiere decir que la clase contenedora es meramente un contenedor. Si la clase que agrega solamente actúa como capa asociativa se pone rombo. En el caso de Linea, como la linea se ha creado sólo para guardar 2 puntos, se podría usar el rombo.

**Dependencia:** Veamos unos ejemplos...
  * A recibe parámetros de B en un método.
  * A devuelve B en algún método.
  * A hace new de B.
  * A llama a métodos de B.
  	- **ESTOS CASOS SON DEPENDENCIAS Y NO COMPOSICIÓN, PORQUE A NO CONTIENE VALORES DE B.**
  	- Las composiciones son un tipo de dependencia, pero no todas las dependencias son composiciones.
  
**-->** Como conclusión, es importante recordar y marcar que usar != componerse con.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Con composición fuerte:
La clase Punto se mantiene pero Linea cambia...

```java
class Linea {
	private Punto p1;
	private Punto p2;

	public Linea(double x1, double x2, double y1, double y2) {
			this.p1 = new Punto (x1, y1);
			this.p2 = new Punto (x2, y2); //así controlo que los puntos no sean creados de antemano, sabemos que no han podido existir antes de que instanciara la línea
	}

	public Punto getP1() { //(A)
		return new Punto(this.p1.x, this.p1.y);
	}

	public Punto getP1Alternativo() { //(B)
		return this.p1.x;
	}
}

```

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

Por el recolector de basura:
* Cuando una `Linea` es inaccesible según el código, sus `Punto` también lo son, porque nada los referencia, de este modo, el GC limpiará ambos.Esto requiere que el Punto a devolver sea una copia (A) u ocultar `Punto` y no sacarlo nunca por la interfaz pública (B).

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor. (Lo de la lista es en el 9, no en el 8).

--> Director es una invariante de clase (siempre existirá).

					 _______________                                      				 ________________
Clase:    | Departamento  |          `(mín..máx)`                 			|    Profesor    |
Atributos:| profesores    | 0..Mucho ------------------------ 1.1 			| nombre: String |
          | director      |                 (director)            		  |                |
          | numProfesores | 0..Mucho ◇----------------------- 1..Mucho |                |
Métodos:  | ....          |                 (profesores)          			|   getNombre()  |
                                                                    			 Profesor(...)

```java (versión array)
public class Profesor {
	private String nombre;

	public Profesor(String nombre) {
		this.nombre = nombre;
	}
}


***

public Departamento { //composicion 1: miembros del dpto; array de primitivos
	private Profesor[] profesores = new Profesor[50]; //50 refs null
	private int numProfesores = 0;

	//composicion 2: el director
	private Profesor director; // <-- null

	public Departamento(Profesor director) {
		//0. Si director es null, lanzo IAE
		//1. Establezco el atributo director
		this.director = director;

		//2. Añado el director como primer miembro del departamento, así garantizando a invariante de clase
		this.profesores[0] = director;
		numProfesores++;
	}
		//3. Poder acceder al listado, sin dar acceso al array interno (de forma controlada)
		public int getNumProfesores() {
			return this.numProfesores;
		}
		public Profesor getProfesor(int pos) {
			//si pos no es válida, lanzar AIOBE
			return this.profesores[pos];
		}

		//metodos para gestionar el listado y el director
		public void addProfesor(Profesor p) { //recibo profesor, composicion debil
			//0. Si p es nulo, lanzar IAE
			//1. Si está lleno, lanzar otra excepción
		
			this.progesores[this.numProfesores] = p;
			this.numProfesores++;

		}
		/** 
		 Elimina un profesor de la lista, pero no puede ser el director actual.
		 */
		 public void eliminarProfesor(int pos) {
		 	//0. Si pos no es válida, no está entre 0 y numProfesores-1,lanzar IAE
			//1. Si el profesor a eliminar es el director, lanzamos excepción
		 	//2. Eliminar el profesor de la posición dada, moviendo los siguientes para tapar el hueco
			//3. Para eliminarlo del array, hay que hacer la tediosa gestión de desplazar todos los elementos una posición a la izquierda y machacar la posición a eliminar.
		 }

		 public void cambiarDirector(Profesor nuevoDirector) {
		 	//0. Si nuevoDirector es nulo, lanzar IAE
		 	//1. Si nuevoDirector no está en el departamento, lanzar IAE
			// 1.2 alternativamente, el nuevo director de fuera podría ser añadido primero al dpto, y luego establecerlo como director, así garantizando la invariante de clase
		 	//2. En otro caso el director por el nuevoDirector
			this.director = nuevoDirector;
		}
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

```java (versión de List, una lista abstracta)

public class Profesor {
	private String nombre;

	public Profesor(String nombre) {
		this.nombre = nombre;
	}
}


***

public Departamento { //composicion 1: miembros del dpto; lista que solo puede almacenar referencias de tipo Profesor
	private List<Profesor> profesores = new ArrayList<>(); //Tiene muchas ventajas, con List: Se facilita un tamaño dinámico e ilimitado (y no reservado de antemano), facilita el eliminado (no se necesita gestionar el array) pues se delega en métodos de List.

	//composicion 2: el director
	private Profesor director; // <-- null

	public Departamento(Profesor director) {
		//0. Si director es null, lanzo IAE
		//1. Establezco el atributo director
		this.director = director;

		//2. Añado el director como primer miembro del departamento, así garantizando a invariante de clase
		this.profesores[0] = director;
		numProfesores++;
	}
		//3. Poder acceder al listado, con métodos de List
		public int getNumProfesores() {
			return this.profesores.size();

		}
		public Profesor getProfesor(int pos) {
			//si pos no es válida, lanzar AIOBE
			return this.profesores.get(pos);
		}

		//viniendonos arriba con getters, para todos los profesores
		public List<Profesor> getProfesores() {
			return this.profesores;
		} //El problema de este método es que si devuelvo directamente la lista o el array recibiría una copia de la referencia, permitiendo MUTARLA desde fuera y exponiéndonos a la posibilidad de que se quite el director de la lista y así violar la invariante de clase.

			//si insisto en emplear un getter, devolvemos una copia:
			// return new ArrayList<>(this.profesores); Esta manera es lenta, O(n) siendo n el orden de la lista.

			//Alternativa B, envolver la lista en una lista no modificable O(1); esta lista tiene una referencia a la original, pero si le llamas a métodos modificadores, no delega en la original y lanza una excepción de tipo UnsupportedOperationException.

			public List<Profesor> getProfesoresSafeFast() {
			return Collections.unmodifiableList(this.profesores);
		}

		/**
		 Versión insegura con array: 

		 public Profesor[] getProfesores() {
		 	return this.profesores;
			}
		 * /

		//metodos para gestionar el listado y el director
		public void addProfesor(Profesor p) { //recibo profesor, composicion debil
			//0. Si p es nulo, lanzar IAE
			//1. Si está lleno, lanzar otra excepción
		
			this.progesores[this.numProfesores] = p;
			this.numProfesores++;

		}
		/** 
		 Elimina un profesor de la lista, pero no puede ser el director actual.
		 */
		 public void eliminarProfesor(int pos) {
		 	//0. Si pos no es válida, no está entre 0 y numProfesores-1,lanzar IAE
			//1. Si el profesor a eliminar es el director, lanzamos excepción
		 	//2. Eliminar el profesor de la posición dada, moviendo los siguientes para tapar el hueco
			//3. Para eliminarlo del array, hay que hacer la tediosa gestión de desplazar todos los elementos una posición a la izquierda y machacar la posición a eliminar.
		 }

		 public void cambiarDirector(Profesor nuevoDirector) {
		 	//0. Si nuevoDirector es nulo, lanzar IAE
		 	//1. Si nuevoDirector no está en el departamento, lanzar IAE
			// 1.2 alternativamente, el nuevo director de fuera podría ser añadido primero al dpto, y luego establecerlo como director, así garantizando la invariante de clase
		 	//2. En otro caso el director por el nuevoDirector
			this.director = nuevoDirector;
		}
}
```

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
