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

### Respuesta


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta


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
