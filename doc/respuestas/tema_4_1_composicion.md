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

En C, la composición se modela de forma natural con `struct`: una estructura grande contiene, como campos, otras estructuras más pequeñas. En este caso, un `Punto` tiene dos coordenadas (`x`, `y`) y una `Linea` tiene dos puntos (`inicio`, `fin`). Esto expresa exactamente la idea de "línea tiene dos puntos" sin necesidad de orientación a objetos.

Para calcular la distancia entre dos puntos del plano se aplica la distancia euclídea: $d = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$. Después, la longitud de una línea puede definirse reutilizando esa misma operación sobre los dos puntos que contiene la `Linea`. Esta reutilización evita duplicar lógica y mantiene el código más claro.

```c
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

Este diseño es correcto en C, pero no impide que el código cliente modifique libremente los campos de `Punto` o `Linea` en cualquier momento. Esa limitación es importante porque abre la puerta a estados no deseados; precisamente ahí es donde Java, con encapsulación e inmutabilidad, aporta una mejora clara.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, la misma idea se expresa mediante clases: `Linea` contiene dos objetos `Punto`. La diferencia clave frente a C es que puede protegerse el estado interno con encapsulación (`private`) y construir objetos inmutables declarando atributos `final` y sin métodos *setter*. Así, una vez creado un `Punto` o una `Linea`, su contenido no cambia.

`Punto` expone un comportamiento (`distanciaA`) y no sus detalles internos para modificación. `Linea`, por su parte, delega el cálculo de su longitud en los puntos que la forman. Además, el constructor de `Linea` valida que no se reciban referencias nulas, evitando errores en tiempo de ejecución y manteniendo una invariante básica del objeto: una línea siempre debe tener dos puntos válidos.

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

### Respuesta

La multiplicidad indica cuántas instancias de una clase pueden o deben relacionarse con una instancia de otra clase. Es una restricción del modelo, no solo un detalle de implementación: ayuda a razonar sobre la validez de los objetos y evita diseños ambiguos. Se suele expresar con rangos como `1`, `0..1`, `0..*`, `1..*`, etc.

En el ejemplo propuesto, de `Linea` hacia `Punto`, la multiplicidad es `2` (o, si se prefiere notación de intervalo, `2..2`), porque cada línea está formada exactamente por dos puntos: inicio y fin. No existe, en este modelo concreto, una línea con un solo punto o con tres puntos.

En sentido inverso, de `Punto` hacia `Linea`, la multiplicidad típica es `0..*`: un punto puede no pertenecer a ninguna línea, o pertenecer a una, o a muchas líneas distintas (por ejemplo, un vértice compartido entre varios segmentos). Por tanto, la relación completa se resume como: `Linea -> Punto: 2` y `Punto -> Linea: 0..*`.

Expresar estas multiplicidades de forma explícita mejora el diseño desde el inicio, porque obliga a pensar en invariantes y casos límite. Además, prepara el terreno para implementar validaciones coherentes cuando la relación se vuelva más compleja en ejercicios posteriores.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
