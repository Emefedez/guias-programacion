## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La **herencia** es un mecanismo de modelado que permite definir una clase nueva (subclase) a partir de otra ya existente (superclase), de modo que la subclase especializa o extiende el comportamiento común. La relación conceptual se expresa como **"A es-un B"**: si `Artillero` hereda de `Soldado`, entonces un artillero **es un** soldado.

Las dos implicaciones principales son las siguientes:

1. **Compatibilidad de tipos**
Una referencia del supertipo puede apuntar a objetos de cualquier subtipo. En consecuencia, allí donde se espera un `Soldado`, se puede proporcionar un `Artillero` o un `Zapador`.

2. **Herencia de estado y comportamiento**
La subclase recibe los atributos y métodos heredables de la superclase. En este ejemplo, tanto `Artillero` como `Zapador` disponen del estado `nombre` y del método `saludar()` definidos en `Soldado`.

Ejemplo completo en Java:

```java
class Soldado {
	private String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public void saludar() {
		System.out.println("Hola, soy " + nombre + ".");
	}
}

class Artillero extends Soldado {
	private int numeroCohetes;

	public Artillero(String nombre, int numeroCohetes) {
		super(nombre);
		this.numeroCohetes = numeroCohetes;
	}

	public int getNumeroCohetes() {
		return numeroCohetes;
	}
}

class Zapador extends Soldado {
	private int numeroMinas;

	public Zapador(String nombre, int numeroMinas) {
		super(nombre);
		this.numeroMinas = numeroMinas;
	}

	public int getNumeroMinas() {
		return numeroMinas;
	}
}

public class DemoHerencia {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Lara", 5),
			new Zapador("Iker", 8),
			new Artillero("Noa", 3)
		};

		for (Soldado s : peloton) {
			s.saludar();
		}
	}
}
```

En este recorrido del array se observa la compatibilidad de tipos: todos los elementos pueden almacenarse como `Soldado` y responder al mensaje común `saludar()`.


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una subclase, se ejecuta **una cadena de constructores**, desde la clase más general hacia la más específica.

Si se instancia `new Artillero("Lara", 5)`, se ejecuta:

1. Constructor de `Soldado`.
2. Constructor de `Artillero`.

Esto ocurre porque una instancia de la subclase contiene también la parte heredada de la superclase; por tanto, primero debe inicializarse esa parte.

La palabra clave `super` dentro de un constructor sirve para invocar explícitamente el constructor de la superclase. Ejemplo:

```java
public Artillero(String nombre, int numeroCohetes) {
	super(nombre); // inicializa la parte Soldado
	this.numeroCohetes = numeroCohetes;
}
```

Si no se escribe nada, Java intenta insertar automáticamente `super()` (sin argumentos). Por ello, cuando la clase base **no** tiene constructor sin parámetros visible, es obligatorio invocar de forma explícita el constructor adecuado con `super(...)`.

Conclusión:

1. Siempre hay llamada a constructor de superclase, explícita o implícita.
2. Si no existe `super()` accesible, debe escribirse `super(conArgumentos)` de forma explícita.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase **forman parte de la instancia real** de la subclase en memoria. Un objeto `Artillero` incluye su zona heredada de `Soldado` (por ejemplo, `nombre`) y su zona propia (por ejemplo, `numeroCohetes`).

Sin embargo, que formen parte de la memoria del objeto **no** implica acceso directo desde el código de la subclase. El modificador `private` restringe el acceso al cuerpo de la clase donde se declara.

Ejemplo:

```java
class Soldado {
	private String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}
}

class Zapador extends Soldado {
	public Zapador(String nombre) {
		super(nombre);
	}

	public void ponerMina() {
		// System.out.println(nombre); // ERROR: nombre es private en Soldado
		System.out.println("Zapador " + getNombre() + " ha colocado una mina.");
	}
}
```

La subclase puede usar métodos públicos/protegidos de la superclase para interactuar con esos datos, pero no puede acceder de forma directa al campo privado.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos incrementa la **extensibilidad** porque permite añadir nuevas subclases sin alterar el código cliente que trabaja contra el supertipo. Este principio reduce acoplamiento y facilita evolución.

Si se agrega una nueva clase `Francotirador` que hereda de `Soldado`, el código que recorre `Soldado[]` para pedir saludos no necesita cambios.

```java
class Francotirador extends Soldado {
	private int alcance;

	public Francotirador(String nombre, int alcance) {
		super(nombre);
		this.alcance = alcance;
	}

	public int getAlcance() {
		return alcance;
	}
}

public class DemoExtensible {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Lara", 5),
			new Zapador("Iker", 8),
			new Francotirador("Vega", 900)
		};

		// No se modifica aunque aparezcan nuevos tipos de Soldado
		for (Soldado s : peloton) {
			s.saludar();
		}
	}
}
```

Esta idea se relaciona con programar contra abstracciones (supertipos) y no contra implementaciones concretas.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, en Java una referencia del supertipo puede apuntar a un objeto real de un subtipo.

```java
Soldado s = new Artillero("Lara", 5);
```

Con esa referencia `s` solo se pueden invocar, en compilación, los métodos visibles en `Soldado` (aunque en ejecución se aplique enlace dinámico para métodos sobreescritos).

Conceptos clave:

1. **Upcasting**: conversión de subtipo a supertipo. Es implícita y segura.
   Ejemplo: `Soldado s = new Artillero("Lara", 5);`

2. **Downcasting**: conversión de supertipo a subtipo. Es explícita y puede fallar en ejecución si el tipo real no coincide.
   Ejemplo: `Artillero a = (Artillero) s;`

3. **instanceof**: operador que comprueba si un objeto es instancia de un tipo (o subtipo), útil para realizar downcasting seguro.

Ejemplo solicitado:

```java
public class DemoCasting {
	public static void main(String[] args) {
		Soldado[] peloton = {
			new Artillero("Lara", 5),
			new Zapador("Iker", 8),
			new Artillero("Noa", 3)
		};

		for (Soldado s : peloton) {
			s.saludar();

			if (s instanceof Artillero) {
				Artillero a = (Artillero) s; // downcasting seguro
				System.out.println("Cohetes disponibles: " + a.getNumeroCohetes());
			}
		}
	}
}
```

Desde Java 16 también puede usarse pattern matching:

```java
if (s instanceof Artillero a) {
	System.out.println(a.getNumeroCohetes());
}
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso **protegido** (`protected`) permite que un miembro sea accesible:

1. Desde la propia clase.
2. Desde sus subclases.
3. Desde clases del mismo paquete.

Se implementa en Java usando el modificador `protected` en atributos o métodos.

Ejemplo solicitado (nombre protegido en `Soldado`):

```java
class Soldado {
	protected String nombre;

	public Soldado(String nombre) {
		this.nombre = nombre;
	}

	public void saludar() {
		System.out.println("Hola, soy " + nombre + ".");
	}
}

class Zapador extends Soldado {
	private int numeroMinas;

	public Zapador(String nombre, int numeroMinas) {
		super(nombre);
		this.numeroMinas = numeroMinas;
	}

	public void ponerBombas() {
		System.out.println("El zapador " + nombre + " ha colocado " + numeroMinas + " minas.");
	}
}
```

Aunque este enfoque es válido, desde diseño suele preferirse mantener campos privados y exponer métodos controlados para preservar encapsulación.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En muchos lenguajes orientados a objetos existe una clase raíz común para todos los objetos, pero **no es una regla universal idéntica en todos los lenguajes**.

**En Java sí** ocurre: la raíz es **`java.lang.Object`** _(Hay un `extends Object` implícito)_.

Implicaciones en Java:

1. Toda clase hereda directa o indirectamente de `Object`.
2. Todos los objetos disponen de **métodos públicos comunes** como `toString()`, `equals()`, `hashCode()` y `getClass()`. Podemos sobreescribirlos con `@Override`.
3. Una referencia `Object` puede apuntar a cualquier objeto, con pérdida de especificidad de tipo.

Por tanto, Java sí tiene una clase base universal para los objetos de su sistema de tipos de referencia.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La **herencia múltiple** de clases consiste en que una clase hereda implementación y estado de más de una superclase a la vez.

En Java **no existe herencia múltiple de clases** _(Dato: en C++ sí)_. Una clase solo puede extender una clase:

```java
class B extends A { }
```

pero no:

```java
// No permitido en Java
// class C extends A, B { }
```

Sí existe, en cambio, **implementación múltiple de interfaces**:

```java
class C implements I1, I2 { }
```

Esta decisión evita ambigüedades clásicas de herencia múltiple de implementación (por ejemplo, variantes del problema del diamante).


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
-> (Excepciones controladas heredad de `Exception` y requieren `try/catch` o `throws` en la firma del método).

Una excepción no controlada es aquella que hereda de `RuntimeException`. Puede crearse una excepción personalizada que encapsule (composición) el objeto `Usuario` que originó el problema y opcionalmente la causa. 

Esto se hace agregando al método que define la excepción un campo para el parámetro `Usuario` y sobrecargando el constructor para aceptar también una causa. Se hace una sobrecarga, porque se quiere permitir la creación de la excepción con o sin causa.

**Entonces, las excepciones personalizadas, además de tener un nombre más adecuado, pueden transportar información adicional relevante para el manejo de errores.**

Ejemplo:

```java
class Usuario {es(Dato: en C++ sí)_(Dato: en C++ sí)_p32 derivative
	private final String id;
	private final String nombre;

	public Usuario(String id, String nombre) {
		this.id = id;
		this.nombre = nombre;
	}

	public String getId() {
		return id;
	}

	public String getNombre() {
		return nombre;
	}

	@Override
	public String toString() {
		return "Usuario{id='" + id + "', nombre='" + nombre + "'}";
	}
}

class UsuarioNoEncontradoException extends RuntimeException {
	private final Usuario usuario;

	public UsuarioNoEncontradoException(Usuario usuario) {
		super("No se encontró el usuario: " + usuario);
		this.usuario = usuario;
	}

	public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
		super("No se encontró el usuario: " + usuario, causa);
		this.usuario = usuario;
	}

	public Usuario getUsuario() {
		return usuario;
	}
}

class ServicioUsuarios {
	public Usuario buscarPorId(String id) {
		Usuario candidato = new Usuario(id, "Desconocido");
		throw new UsuarioNoEncontradoException(candidato);
	}
}
```

Con esta composición, quien captura la excepción puede recuperar el `Usuario` asociado para registrar trazas, auditar o mostrar información contextual.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

**Si sólo necesito reutilizar código debo hacer composición, no herencia. En su defecto, si además necesito heredar comportamiento y abstracción, entonces sí considero la herencia.** Véase un `Cliente` que tiene un `Motor` (composición) frente a un `Vehículo` que es un `Motor` (herencia).

No conviene usar herencia solo para reutilizar código porque la herencia establece una relación semántica fuerte (**es-un**) y un acoplamiento estructural rígido entre clases.

Riesgos principales cuando se usa únicamente por reutilización:

1. Se fuerza una jerarquía artificial que no representa el dominio.
2. Se heredan miembros que pueden no tener sentido en el subtipo.
3. Cambios en la superclase pueden romper subclases (fragilidad).
4. Se reduce la flexibilidad para combinar comportamientos.

Por ello, si el objetivo es compartir funcionalidad sin relación "es-un" clara, suele ser preferible la composición o delegación.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

**Si me vale la composición debo usarla en vez de la herencia dado que la herencia impone una dependencia muy fuerte de las clases que heredan respecto a la clase base. Es decir, la herencia es un compromiso de diseño que puede dificultar la evolución futura.**

-> De hacer composición donde B y C tienen un campo común de tipo A, cambiar el funcionamiento interno de B o C sólo implica cambiar la interfaz pública de A.

-> Si fuese herencia, B y C dependen de la estructura y comportamiento de A, por lo que cualquier cambio en A puede afectar a B y C, incluso si el cambio es interno.

**Es decir, la herencia es menos flexible porque B y C siempre serán un A, no es una condición necesaria para la herencia, donde la referencia de B y C hacia A puede cambiar durante la ejecución.** Por ejemplo, un `Vehículo` puede tener una `Tapicería` que se puede cambiar en tiempo de ejecución (composición), mientras que un `Coche` que hereda de `Vehículo` siempre es un `Vehículo` (herencia).

Se favorece la composición frente a la herencia porque la composición permite ensamblar comportamiento mediante objetos colaboradores, reduciendo acoplamiento y mejorando evolución del diseño.

Ventajas habituales de composición:

1. Cambios más localizados y menor efecto cascada.
2. Posibilidad de sustituir componentes en ejecución o por configuración.
3. Menor dependencia de detalles internos de una superclase.
4. Mejor adhesión al principio de responsabilidad única.
5. Facilita pruebas unitarias con dobles de prueba de dependencias.

En términos de diseño, composición permite modelar relaciones **tiene-un**, mientras que herencia exige **es-un**. Cuando ambas podrían aplicarse, composición suele ofrecer mayor mantenibilidad.

**Comentario histórico**: La herencia fue un reclamo atractivo de OOP, pero ha desarrollado una fama tan mala que muchos lenguajes modernos ni siquiera la implementan (véase Rust). La composición sí se sigue implementando en (casi) todo.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

La afirmación "la herencia rompe la encapsulación" se refiere a que la subclase depende de la estructura y del comportamiento interno heredado, no solo de una interfaz estable.

Aunque exista ocultación (`private`), la subclase queda condicionada por:

1. Métodos protegidos y contratos implícitos de la superclase.
2. Suposiciones sobre orden de llamadas, estados internos o extensiones permitidas.
3. Cambios en implementación base que alteran el comportamiento heredado.

Esto puede generar clases hijas frágiles y difíciles de mantener. En cambio, con composición, la clase cliente utiliza otra clase a través de una interfaz explícita, disminuyendo conocimiento de internals y preservando mejor el encapsulamiento.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

Se pueden modelar los mismos datos compartidos de dos formas, con consecuencias de diseño distintas.

## Opción A: Herencia (`Persona`), con `get()`s y toda la parafernalia habitual

```java
class Persona {
	private String dni;
	private String nombre;

	public Persona(String dni, String nombre) {
		this.dni = dni;
		this.nombre = nombre;
	}

	public String getDni() {
		return dni;
	}

	public String getNombre() {
		return nombre;
	}
}

class Estudiante extends Persona {
	private String carrera;

	public Estudiante(String dni, String nombre, String carrera) {
		super(dni, nombre);
		this.carrera = carrera;
	}

	public String getCarrera() {
		return carrera;
	}
}

class Trabajador extends Persona {
	private String empresa;

	public Trabajador(String dni, String nombre, String empresa) {
		super(dni, nombre);
		this.empresa = empresa;
	}

	public String getEmpresa() {
		return empresa;
	}
}
```

## Opción B: Composición (`DatosPersonales`) --> Una composición DatosPersonales puede reducir reutilización (es como pensar al revés), generamos el DatosPersonales desde Estudiante y Trabajador

```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class EstudianteComp {
    private DatosPersonales datosPersonales;
    private String carrera;

    public EstudianteComp(String dni, String nombre, String carrera) {
        this.datosPersonales = new DatosPersonales(dni, nombre);
        this.carrera = carrera;
    }

    public DatosPersonales getDatosPersonales() {
        return datosPersonales;
    }

    public String getCarrera() {
        return carrera;
    }
}

class TrabajadorComp {
    private DatosPersonales datosPersonales;
    private String empresa;

    public TrabajadorComp(String dni, String nombre, String empresa) {
        this.datosPersonales = new DatosPersonales(dni, nombre);
        this.empresa = empresa;
    }

    public DatosPersonales getDatosPersonales() {
        return datosPersonales;
    }

    public String getEmpresa() {
        return empresa;
    }
}
```

Comparación breve:

1. Herencia es adecuada cuando realmente se cumple una jerarquía conceptual fuerte (`Estudiante` es `Persona`).
2. Composición es preferible cuando se busca reutilización de datos/comportamiento con menor acoplamiento y mayor flexibilidad.
3. En el modelo por composición, tal como se solicita, `Estudiante` y `Trabajador` reciben `DatosPersonales` por constructor, permitiendo compartir o sustituir fácilmente ese componente.
