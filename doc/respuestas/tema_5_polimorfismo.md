<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El polimorfismo es un principio de la programación orientada a objetos que permite tratar objetos de distintas clases relacionadas como si fueran del mismo tipo base, haciendo que puedan responder de manera diferente a una misma operación. Esto se apoya directamente en la herencia: una referencia de la clase padre puede apuntar a objetos de cualquiera de sus clases hijas. 

El polimorfismo sirve principalmente para escribir código más flexible, reutilizable y extensible. En lugar de depender de clases concretas, el código se apoya en la superclase, lo que permite añadir nuevas subclases sin modificar el código existente

La sobreescritura de métodos es el mecanismo que hace posible el polimorfismo en Java. Consiste en que una subclase proporciona su propia implementación de un método que ya está definido en la superclase, manteniendo exactamente la misma firma (nombre, parámetros y tipo de retorno compatible). Cuando se invoca ese método a través de una referencia del tipo padre, Java decide automáticamente qué implementación ejecutar según la clase real del objeto


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica o enlace tardío consiste en que la decisión sobre qué implementación concreta de un método se ejecuta se retrasa hasta tiempo de ejecución, en lugar de resolverse en tiempo de compilación. Esto implica que, cuando se invoca un método a través de una referencia a una clase base, el sistema selecciona automáticamente la versión del método correspondiente a la clase real del objeto.  El polimorfismo solo es posible porque el enlace del método es dinámico: una misma llamada puede producir comportamientos distintos según el objeto concreto que reciba el mensaje

En C++, la ligadura dinámica no es el comportamiento por defecto, debe declararse como virtual en la clase base. Si no se hace así, el enlace es estático, y el método que se ejecuta depende del tipo de la variable, no del objeto
En Java la ligadura dinámica es el comportamiento normal para todos los métodos de instancia (salvo final, static o private), por lo que no es necesario indicarlo. En Python, la ligadura dinámica también es el comportamiento por defecto


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("El zapador saluda mientras revisa explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe el método saludar
}

public class PruebaPolimorfismo {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[3];
        soldados[0] = new Soldado();
        soldados[1] = new Zapador();
        soldados[2] = new Artillero();

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, cuando se sobreescribe un método, es posible invocar el método de la clase base para reutilizar su comportamiento y ampliarlo, en lugar de sustituirlo por completo. Al usar super.nombreMetodo(), se fuerza explícitamente la llamada a la versión de la superclase, incluso aunque esté sobreescrito en la subclase

class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
La lista de parámetros debe ser exactamente la misma que en el método de la clase base, tanto en número como en tipo y orden; de lo contrario, no se consideraría una sobreescritura. El tipo de retorno debe ser el mismo o un subtipo compatible. Además, no se puede reducir la visibilidad del método (public->protected)  ni lanzar excepciones más generales que las declaradas en el método original

La sobreescritura (overriding) ocurre entre una superclase y una subclase, afecta a métodos con la misma firma y está ligada al polimorfismo y al enlace dinámico, ya que la decisión se toma en tiempo de ejecución.
La sobrecarga (overloading), en cambio, ocurre dentro de una misma clase (o entre clase y subclase) y consiste en definir varios métodos con el mismo nombre pero distinta lista de parámetros; su selección se resuelve en tiempo de compilación y no implica polimorfismo dinámico

La anotación @Override indica explícitamente que un método pretende sobreescribir uno heredado de la superclase. Si el método no cumple las reglas de sobreescritura, el compilador generará un error


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Todas las clases en Java heredan implícitamente de Object, y métodos como toString, equals o hashCode están pensados precisamente para ser sobreescritos
Al sobrescribir toString, por ejemplo, se define cómo debe representarse un objeto concreto como texto, pero la llamada suele hacerse a través de código genérico, como System.out.println(obj), que trata el objeto como un Object. Lo mismo ocurre con equals: muchas bibliotecas y estructuras de datos (como colecciones) trabajan con referencias de tipo Object y llaman a equals sin conocer la clase concreta


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que representa un concepto general e incompleto, pensado para ser extendido por otras clases, pero no para crear objetos directamente a partir de ella
Un método abstracto es un método que no tiene implementación, es decir, solo se declara su firma. Su finalidad es obligar a que las subclases proporcionen su propia versión del método
El uso de clases y métodos abstractos está estrechamente relacionado con el polimorfismo, ya que permite tratar distintos tipos de objetos de manera uniforme, asegurando que todos implementen ciertos métodos, aunque cada uno lo haga de forma distinta.

En este diseño, no se pueden crear objetos de Soldado, pero sí usar referencias de ese tipo para trabajar polimórficamente con Zapador y Artillero

abstract class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }
}



## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
En Java, la palabra clave final aplicada a un método indica que dicho método no puede ser sobreescrito. El método sigue siendo heredado y puede invocarse de forma normal, pero su definición queda fijada de manera definitiva
Cuando final se aplica a una clase, se está impidiendo completamente la herencia: no es posible crear subclases a partir de ella.
Marcar una clase o un método como final limita o anula el polimorfismo
Un ejemplo en la API estándar de Java es String. Esta clase está declarada como final para evitar que se altere su comportamiento, lo cual es crucial para la seguridad, la gestión de memoria y el funcionamiento correcto de estructuras como el String Pool



## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Una interfaz es un tipo que define un contrato, es decir, un conjunto de métodos que una clase se compromete a implementar, qué se puede hacer.

Las interfaces se parecen a las clases abstractas, pero no son lo mismo. Una clase abstracta puede contener atributos, constructores y métodos con implementación normal ("x es un"), mientras que una interfaz está pensada para definir capacidades sin imponer una implementación concreta ("x puede hacer"). Ademas una clase puede implementar más de una interfaz, pero solo puede heredar de una única clase (abstracta o no).


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}


class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto2D p = (Punto2D) otro; // downcasting
        double dx = x - p.x;
        double dy = y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}


class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto3D p = (Punto3D) otro; // downcasting
        double dx = x - p.x;
        double dy = y - p.y;
        double dz = z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}


class Linea {
    private Punto a;
    private Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}




## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces en Java consiste en que una interfaz puede extender a otra interfaz, heredando sus métodos abstractos. Una interfaz puede extender una o varias interfaces a la vez, usando la palabra clave extends seguida de una lista separada por comas. Esto no plantea los problemas clásicos de la herencia múltiple de clases, ya que no hay estado ni implementación obligatoria que pueda entrar en conflicto.


public interface Fichero {
    String leerContenido();
}
´´
public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
