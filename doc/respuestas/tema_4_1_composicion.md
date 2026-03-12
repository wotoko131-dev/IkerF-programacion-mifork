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

double distancia(Punto a, Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx* dx + dy*dy);
}

double longitud(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main() {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la línea: %.2f\n", longitud(l));

    return 0;
}


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}

public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distanciaA(fin);
    }

    public Punto getInicio() {
        return inicio;
    }

    public Punto getFin() {
        return fin;
    }
}


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad en una relación de composición indica cuántas instancias de una clase participan en la relación respecto a una instancia de la otra clase. En composición, la multiplicidad expresa tanto cuántos objetos “forma parte de” otro objeto como la restricción de que esos objetos componentes no pueden existir de manera independiente fuera del todo que los contiene. Esta información suele representarse mediante rangos, como 1, 0..1, *, 1..*, según cuántos elementos estén implicados

Linea y Punto, una línea está formada exactamente por dos puntos: un punto de inicio y un punto de fin. Por tanto, la multiplicidad desde Linea hacia Punto es 2, que formalmente puede expresarse como 1 Linea → 2 Puntos o 1 → 2.
En la dirección inversa, desde Punto hacia Linea, Un punto concreto puede no formar parte de ninguna línea o puede pertenecer a varias líneas distintas: 1 Punto → 0..* Lineas


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La distinción entre composición fuerte y composición débil se basa en el grado de dependencia entre los objetos que forman parte de una relación “todo–parte”. 
En una composición fuerte o composición, la parte no puede existir sin el todo.Esto significa que cuando el objeto contenedor deja de existir, todas sus partes se destruyen también.
La composición débil o asociacion o agregacion, implica que las partes pueden existir de manera independiente del todo, aunque formen conceptualmente parte de él.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Cuando una clase utiliza a otra únicamente como parámetro, como valor devuelto, como variable local o creándola dentro de un método con new, no se considera que exista una relación de composición entre ellas. En estos casos, la relación es mucho más ligera y se denomina dependencia. La clase solo necesita a la otra de manera puntual para realizar alguna operación, pero no la “posee” ni controla su ciclo de vida


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
COMPOSICION FUERTE

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    // La línea crea sus propios puntos: COMPOSICIÓN FUERTE
    public Linea(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distanciaA(fin);
    }
}



COMPOSICION DEBIL

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    // La línea recibe puntos creados fuera: COMPOSICIÓN DÉBIL (AGREGACIÓN)
    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distanciaA(fin);
    }
}
``

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

El objeto contenedor no destruye explícitamente a los objetos que contiene. En su lugar, la gestión de memoria se realiza mediante el garbage collector (GC), que elimina automáticamente los objetos cuando ya no existe ninguna referencia accesible. Cuando el contenedor deja de ser accesible, sus referencias internas también quedan inaccesibles.

Este comportamiento refleja la diferencia entre control lógico y control físico de la memoria. La composición fuerte asegura que las partes no existan sin el todo en el diseño de la clase, pero no implica que el código del contenedor llame a ningún método especial para destruir sus componentes

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null) {
            throw new IllegalArgumentException("El nombre no puede ser null.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    /**
     * Construye un departamento. Debe indicarse un director que formará parte
     * de la lista de profesores desde el inicio.
     */
    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director inicial no puede ser null.");
        }

        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = directorInicial;
        this.numProfesores = 1;

        this.director = directorInicial;
    }

    /**
     * Devuelve cuántos profesores hay en el departamento.
     */
    public int getNumProfesores() {
        return numProfesores;
    }

    /**
     * Devuelve el profesor en una posición dada.
     */
    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion);
        }
        return profesores[posicion];
    }

    /**
     * Añade un profesor al final de la lista.
     */
    public void addProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("No se puede añadir null.");
        }
        if (numProfesores == MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores.");
        }

        profesores[numProfesores] = p;
        numProfesores++;
    }

    /**
     * Elimina un profesor dada su posición.
     * Se debe mantener la invariante: el director no puede ser eliminado.
     */
    public void removeProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion);
        }

        Profesor eliminado = profesores[posicion];

        // Invariante: no se puede eliminar al director
        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        // Compactación del array
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    /**
     * Cambia el director siempre que el nuevo esté dentro del departamento.
     */
    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null.");
        }

        // Comprobación: el nuevo director debe estar en la lista
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento.");
        }

        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null) {
            throw new IllegalArgumentException("El nombre no puede ser null.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public final class Departamento {

    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director inicial no puede ser null.");
        }

        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        return profesores.get(posicion);
    }

    public void addProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("No se puede añadir null.");
        }
        profesores.add(p);
    }

    public void removeProfesor(int posicion) {
        Profesor eliminado = profesores.get(posicion);

        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director.");
        }

        profesores.remove(posicion);
    }

    public Profesor getDirector() {
        return director;
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null.");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El nuevo director debe ser profesor del departamento.");
        }
        this.director = nuevoDirector;
    }

    /**
     * Método seguro para devolver los profesores si se quisieran todos a la vez.
     */
    public List<Profesor> getProfesoresSeguro() {
        return Collections.unmodifiableList(profesores);
    }
}

1. No hace falta mantener manualmente un contador gracias a size()
2. No es necesario controlar un tamaño máximo (MAX_PROFESORES), ArrayList crece dinámicamente según se necesite.
3. Desaparece la compactación manual del array al borrar un profesor con profesores.remove(posicion);
4. No se necesita inicializar el array ni llenar posiciones con null

El metodo para devolver la lista de profesores puede romper la encapsulación. Para resolverlo, se puede devolver una lista inmutable o una copia de la original.

public List<Profesor> getProfesores() {
    return Collections.unmodifiableList(profesores);
}

public List<Profesor> getProfesores() {
    return new ArrayList<>(profesores);
}

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

public final class Persona {
    private final String nombre;
    private final Persona madre;  // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null) {
            throw new IllegalArgumentException("El nombre no puede ser null.");
        }
        this.nombre = nombre;
        this.madre = madre; // puede ser null (por ejemplo, al no registrar generaciones anteriores)
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return nombre;
    }
}

public class Main {
    public static void main(String[] args) {

        Persona abuela = new Persona("María", null);
        Persona madre = new Persona("Laura", abuela);
        Persona hijo  = new Persona("Iker", madre);

        System.out.println("Hijo: " + hijo.getNombre());
        System.out.println("Madre: " + hijo.getMadre().getNombre());
        System.out.println("Abuela: " + hijo.getMadre().getMadre().getNombre());
    }
}


Usa composición recursiva, porque la clase Persona contiene dentro de sí objetos de su misma clase. La composición no es fuerte ni débil en el sentido clásico de “ciclo de vida”, sino estructural: cada persona tiene una madre, que también es una persona. La estructura resultante forma una cadena recursiva de objetos, análoga a las excepciones en Java que contienen una “causa”, que a su vez puede contener otra causa.

Otros ejemplos son los Nodos y Listas enlazadas.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición bidireccionales son aquellas en las que cada objeto conoce al otro: el “todo” conoce a sus partes, y cada parte mantiene también una referencia hacia el todo

En el ejemplo de Profesor y Departamento, para implementar una relación bidireccional habría que modificar ambas clases. En la clase Profesor debería añadirse un atributo privado que represente el departamento al que pertenece. El departamento, al añadir un profesor con addProfesor, tendría que llamar a un método interno del profesor para establecer su departamento; y al eliminarlo con removeProfesor, ese departamento debería eliminarse del profesor poniéndolo a null, garantizando la consistencia.