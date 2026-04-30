 <!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un puntero a una función es una variable que almacena la dirección de memoria de una función, permitiendo invocarla de forma indirecta. En C, las funciones no son elementos orientados a objetos, pero pueden tratarse como datos en el sentido de que su dirección puede asignarse a una variable y pasarse como parámetro

En el siguiente ejemplo se define una función que recibe una cadena de caracteres y devuelve la misma cadena convertida a mayúsculas. A continuación, se crea un puntero local llamado aMayusculas que apunta a dicha función y se utiliza para realizar la llamada indirecta:

#include <stdio.h>
#include <ctype.h>

char* convertirMayusculas(char* texto) {
    char* p = texto;
    while (*p) {
        *p = toupper((unsigned char)*p);
        p++;
    }
    return texto;
}

int main() {
    char cadena[] = "Programacion funcional en C";

    char* (*aMayusculas)(char*);
    aMayusculas = convertirMayusculas;

    printf("%s\n", aMayusculas(cadena));
    return 0;
}


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda es una función anónima, es decir, una función que no tiene nombre explícito y que puede definirse directamente allí donde se necesita. A diferencia de los punteros a funciones en C, las funciones lambda encapsulan tanto el comportamiento como, en muchos lenguajes, el contexto en el que se definen. 

En Java, las funciones lambda no existen como entidades independientes, sino que se representan mediante interfaces funcionales. Usando la interfaz Function<String, String>, la lambda se asigna igualmente a una variable local llamada aMayusculas y se invoca a través de su método apply

import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = texto -> texto.toUpperCase();
        System.out.println(aMayusculas.apply("Programación funcional en Java"));
    }
}


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El paradigma funcional es un enfoque de programación que modela los programas como la evaluación de funciones matemáticas, priorizando la descripción de qué se calcula frente a cómo se ejecuta paso a paso. 
Java 8 es multi‑paradigma porque permite utilizar diferentes estilos de programación dentro del mismo lenguaje. Aunque Java nació como un lenguaje puramente orientado a objetos, a partir de Java 8 incorpora elementos propios del paradigma funcional, como las funciones lambda y las interfaces funcionales.
La expresión “funciones como ciudadanos de primera clase” indica que las funciones se tratan igual que otros valores del lenguaje, como números o referencias a objetos. Esto implica que pueden asignarse a variables, pasarse como argumentos, devolverse como resultado de otras funciones y almacenarse en estructuras de datos.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
Una función lambda siempre se basa en una interfaz funcional, es decir, una interfaz que contiene exactamente un método abstracto

parámetros, el operador -> (flecha lambda) y el cuerpo de la expresión
Los parámetros pueden ir entre paréntesis y su tipo suele inferirse automáticamente por el compilador, aunque también puede indicarse de forma explícita. El cuerpo puede ser una única expresión, cuyo resultado se devuelve implícitamente, o un bloque de sentencias encerrado entre llaves, en cuyo caso debe usarse return si se devuelve un valor

Sin parametros: () -> expresión
Uno:            x -> x.toUpperCase()
varios: (a, b) -> a + b
bloque: (x) -> { /* sentencias */ }


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
import java.util.function.Function;

public class EjemploTransformar {
    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        System.out.println(transformar("Paradigma funcional", aMayusculas));
    }
}
``


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
import java.util.function.Function;

public class EjemploTransformar {
    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        System.out.println(
            transformar("Paradigma funcional",
                s -> new StringBuilder(s).reverse().toString()
            )
        );
    }
}


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre o closure es una función que, además de su propio código, captura y retiene acceso a variables del contexto en el que fue definida, incluso cuando se ejecuta en un momento posterior. Estas variables no son parámetros explícitos de la función, pero forman parte de su entorno léxico. En Java, las funciones lambda pueden formar cierres, con la particularidad de que solo pueden capturar variables locales que sean finales o efectivamente finales. Esto significa que, aunque no estén declaradas con final, su valor no debe cambiar después de ser inicializado

import java.util.function.Function;

public class EjemploClosure {
    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " - programación funcional";

        System.out.println(
            transformar("Java",
                s -> s + sufijo
            )
        );
    }
}


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Un puntero a función en C es simplemente una dirección de memoria que apunta a una función concreta; no encapsula ningún estado adicional ni información sobre el entorno en el que se utiliza. En cambio, una función lambda es una construcción de más alto nivel que representa un comportamiento completo, y que puede capturar variables del contexto donde se define, formando lo que se denomina un closure.

Otra diferencia relevante es la integración con el sistema de tipos del lenguaje. En C, el uso de punteros a funciones es más flexible pero también más propenso a errores, ya que no existe un mecanismo que garantice comportamientos adicionales más allá de la signatura. En Java, las funciones lambda están ligadas a interfaces funcionales, lo que proporciona comprobación de tipos en tiempo de compilación, mejor legibilidad y una semántica más clara


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
En Java, una interfaz funcional es una interfaz que representa un único comportamiento y que sirve como tipo objetivo para una función lambda. Dado que Java es un lenguaje con comprobación estática de tipos y las funciones no existen como entidades independientes, toda función lambda debe asociarse a un tipo concreto, y ese tipo es precisamente una interfaz funcional.

El requisito fundamental de una interfaz funcional es que contenga exactamente un método abstracto. Este método define la “forma” de la función lambda: su lista de parámetros y su tipo de retorno. Puede contener, sin embargo, otros elementos sin romper esta condición, como métodos default, métodos static o métodos heredados de Object (toString, equals, etc.), ya que estos no cuentan como métodos abstractos propios. Para facilitar su identificación y evitar errores, Java proporciona la anotación @FunctionalInterface

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
La interfaz Transformador genérica podría definirse de la siguiente manera:
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
``

A partir de esta definición, puede crearse fácilmente un transformador que convierta un Double en un Integer redondeado, usando una función lambda:
public class EjemploGenerico {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            d -> (int) Math.round(d);

        Integer resultado = redondear.transformar(3.6);
        System.out.println(resultado);
    }
}


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
En Java, a partir de Java 8, se introdujo un conjunto de interfaces funcionales predefinidas en el paquete java.util.function. Estas interfaces cubren los casos de uso más habituales cuando se trabaja con funciones lambda y programación funcional básica, evitando la necesidad de definir interfaces propias como Transformador en la mayoría de situaciones.

Function<T, R>, que representa una transformación de un valor de un tipo T en otro de tipo R, siendo conceptualmente equivalente al Transformador genérico planteado anteriormente. Junto a ella existen variaciones muy comunes como UnaryOperator<T> (cuando el tipo de entrada y salida es el mismo) y BinaryOperator<T> (cuando se combinan dos valores del mismo tipo para producir uno solo)

Para funciones que consumen valores pero no devuelven resultado, Java proporciona Consumer<T> (un parámetro) y BiConsumer<T, U> (dos parámetros). Para el caso contrario, cuando se desea producir un valor sin recibir parámetros, se dispone de Supplier<T>. También existen interfaces específicas para predicados, es decir, funciones que devuelven un valor booleano, como Predicate<T> y BiPredicate<T, U>.

Además de estas, Java incluye numerosas versiones especializadas para tipos primitivos (IntFunction, DoubleConsumer, LongPredicate, etc.) con el objetivo de evitar el coste de autoboxing.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(-3, 0, 4, 7, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo");
            }
        });
    }
}

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
No se utiliza Consumer<T> para permitir mayor flexibilidad con la herencia y los genéricos. Un Consumer<T> consume valores de tipo T, pero también es perfectamente válido que consuma objetos de un supertipo de T. Por ejemplo, si se tiene una List<Integer>, una función que consuma Number o incluso Object puede operar correctamente sobre sus elementos. El uso de ? super T refleja precisamente esta posibilidad y evita restringir innecesariamente el tipo de la función recibida.

Este diseño sigue el principio conocido como PECS, siglas de Producer Extends, Consumer Super. Esta regla establece que, cuando un genérico produce valores (se leen), debe usarse ? extends T, y cuando consume valores (se escriben o se reciben como entrada), debe usarse ? super T. En el caso de forEach, la colección produce elementos de tipo T, pero la función Consumer los consume, por lo que resulta correcto y más general emplear Consumer<? super T>

Una firma más flexible del método transformar sería la siguiente:

public static <T, R> R transformar(
        T valor,
        java.util.function.Function<? super T, ? extends R> transformadora) {
    return transformadora.apply(valor);
}

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos permiten tratar un método existente como un valor que puede asignarse a una variable y ejecutarse posteriormente.
En Java, las referencias a métodos están integradas formalmente en el lenguaje a partir de Java 8 y se expresan mediante el operador ::. Una referencia a un método de instancia necesita un objeto concreto y es compatible con una interfaz funcional cuya signatura coincida con la del método.
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class EjemploReferenciaMetodo {
    public static void main(String[] args) {
        Persona persona = new Persona("Iker");
        Runnable referenciaSaludar = persona::saludar;

        referenciaSaludar.run();
    }
}


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
El primer tipo es la referencia a un método estático, que no depende de ninguna instancia concreta. Se escribe usando el nombre de la clase seguido de :: y el nombre del método.
import java.util.function.Function;

public class EjemploEstatico {
    public static Integer redondear(Double d) {
        return (int) Math.round(d);
    }

    public static void main(String[] args) {
        Function<Double, Integer> f = EjemploEstatico::redondear;
        System.out.println(f.apply(3.7));
    }
}

El segundo tipo es la referencia a un constructor, que permite tratar la creación de objetos como una función. Se expresa con el nombre de la clase seguido de ::new

import java.util.function.Supplier;

class Persona {
    public Persona() {
        System.out.println("Persona creada");
    }
}

public class EjemploConstructor {
    public static void main(String[] args) {
        Supplier<Persona> crearPersona = Persona::new;
        crearPersona.get();
    }
}

El tercer tipo es la referencia a un método de instancia de una instancia concreta, donde el método se invoca siempre sobre un objeto específico

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class EjemploInstanciaConcreta {
    public static void main(String[] args) {
        Persona p = new Persona("Iker");
        Runnable saludo = p::saludar;
        saludo.run();
    }
}

Por último, existe la referencia a un método de instancia sobre cualquier instancia de una clase, donde la instancia no se fija de antemano y se recibe como primer parámetro implícito

import java.util.List;

public class EjemploCualquierInstancia {
    public static void main(String[] args) {
        List<String> palabras = List.of("hola", "java", "funcional");
        palabras.forEach(String::toUpperCase);
    }
}



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
En una primera versión, la función de comparación se implementa manualmente dentro de la expresión lambda 

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }
}

public class OrdenacionManual {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Ana", 30));
        personas.add(new Persona("Luis", 25));
        personas.add(new Persona("Carlos", 30));

        Collections.sort(personas, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return p1.getEdad() - p2.getEdad();
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });
    }
}

En una segunda versión, se emplean los métodos auxiliares de la clase Comparator, lo que permite construir el comparador de forma más declarativa y compacta.

import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class OrdenacionConComparator {
    public static void main(String[] args) {
        List<Persona> personas = List.of(
            new Persona("Ana", 30),
            new Persona("Luis", 25),
            new Persona("Carlos", 30)
        );

        Collections.sort(
            personas,
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );
    }
}