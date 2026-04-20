<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
Un array de Object puede almacenar referencias a cualquier objeto, independientemente de su tipo concreto. La estructura de datos es flexible y permite almacenar valores de cualquier tipo, pero a costa de perder seguridad de tipos, ya que los errores aparecen en tiempo de ejecución si el tipo no coincide

Object[] vector = new Object[10];

vector[0] = Integer.valueOf(5);
vector[1] = Double.valueOf(3.14);

// Recuperación (requiere casting explícito)
int valorA = (Integer) vector[0];
double valorB = (Double) vector[1];

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La programación genérica es un paradigma que permite definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos que manejan.La genericidad busca separar qué hace una estructura o algoritmo de con qué tipo de datos trabaja. De este modo, una pila, una lista o un vector pueden definirse una sola vez y reutilizarse con enteros, cadenas u otros objetos, sin modificar su implementación

El ejemplo anterior puede considerarse un ejemplo muy básico o rudimentario de programación genérica, ya que permite almacenar cualquier tipo de dato en una misma estructura.  Sin embargo, no constituye programación genérica en sentido estricto, porque la comprobación de tipos se pierde y los errores solo se detectan en tiempo de ejecución. La programación genérica moderna, surge precisamente para solventar esas limitaciones manteniendo flexibilidad y seguridad de tipos simultáneamente
## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema es que se elimina la verificación de tipos en tiempo de compilación. La estructura de datos deja de conocer qué tipo concreto contiene cada elemento, por lo que el compilador no puede detectar accesos incorrectos. Como consecuencia, muchos errores que podrían haberse detectado al compilar pasan a manifestarse únicamente en tiempo de ejecución: al recuperar un elemento es necesario hacer un cast al tipo esperado. Si el objeto almacenado no es de ese tipo, se produce una excepción ClassCastException en tiempo de ejecución pero  el compilador no puede garantizar que la conversión sea correcta.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son un mecanismo de la programación genérica que permite definir clases, interfaces o métodos de forma independiente del tipo concreto de los datos que van a manejar. Se emplea un identificador de tipo (habitualmente T, E, K, etc.) que actúa como un marcador y se sustituye por un tipo real cuando se utiliza. Ya no es necesario hacer conversiones explícitas al recuperar los datos, y los usos incorrectos se detectan antes de ejecutar el programa.

// Clase genérica con parámetro de tipo
class Caja<T> {
    private T contenido;

    public void guardar(T valor) {
        contenido = valor;
    }

    public T obtener() {
        return contenido;
    }
}

// Uso
Caja<Integer> cajaEnteros = new Caja<>();
cajaEnteros.guardar(10);
int x = cajaEnteros.obtener(); // No requiere casting

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

import java.util.ArrayList;
import java.util.List;

List<String> lista = new ArrayList<>();

lista.add("uno");
lista.add("dos");
lista.add("tres");

for (String s : lista) {
    System.out.println(s.toUpperCase()); // s es String con total seguridad
}

En C++, la programación genérica se consigue mediante templates, que cumplen un propósito similar, pero resuelto en tiempo de compilación mediante generación de código para cada tipo concreto. Al instanciar un std::vector<std::string>, la estructura queda especializada para cadenas de texto, impidiendo la inserción de otros tipos

#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;

    lista.push_back("uno");
    lista.push_back("dos");
    lista.push_back("tres");

    for (const std::string& s : lista) {
        std::cout << s.length() << std::endl; // s es std::string con total seguridad
    }
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase con parámetros de tipo, el compilador debe decidir cómo gestionar esa información de tipos genéricos para que el programa funcione correctamente

En Java, el compilador aplica un mecanismo llamado type erasure (borrado de tipos). Durante la compilación, los parámetros de tipo (<T>, <E>, etc.) se eliminan y se sustituyen por su límite superior. El compilador añade conversiones de tipo y comprobaciones donde es necesario para mantener la seguridad de tipos, pero en tiempo de ejecución no se distingue, por ejemplo, entre una List<String> y una List<Integer>

En C++, el enfoque es completamente distinto y se basa en la instanciación de plantillas (templates). Cuando se utiliza una plantilla con un tipo concreto, el compilador genera una versión específica del código para ese tipo. Por ejemplo, vector<string> y vector<int> producen dos implementaciones distintas en el código compilado. En este caso, la información de tipos se conserva plenamente en tiempo de compilación y no existe borrado de tipos.


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


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
