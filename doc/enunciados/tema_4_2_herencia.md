<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En orientación a objetos, la herencia es un mecanismo que permite definir una clase nueva a partir de otra ya existente, estableciendo una relación conceptual del tipo “A es‑un B”. Esto significa que una clase derivada (subclase) representa una especialización de la clase base (superclase) y, por tanto, cumple todo lo que define la clase más general

La primera implicación importante es la compatibilidad de tipos. En Java, una referencia de la superclase puede apuntar a un objeto de cualquiera de sus subclases. Esto permite tratar de forma uniforme objetos distintos que comparten una base común, por ejemplo almacenándolos en la misma estructura de datos o invocando métodos comunes sin conocer su tipo concreto

La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan automáticamente los atributos y métodos no privados de la superclase, evitando duplicar código y garantizando coherencia


// Clase base
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}


// Subtipo Artillero
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}


// Subtipo Zapador
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}


public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = new Soldado[3];
        escuadra[0] = new Soldado("Carlos");
        escuadra[1] = new Artillero("Luis", 5);
        escuadra[2] = new Zapador("Miguel", 3);

        for (Soldado s : escuadra) {
            s.saludar(); // compatibilidad de tipos en acción
        }
    }
}


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un objeto de una subclase en Java, siempre se ejecutan dos constructores: primero el de la clase base y después el de la clase derivada. Este orden es obligatorio y está garantizado por el lenguaje, ya que antes de construir la parte específica del objeto (la subclase) es necesario inicializar correctamente la parte común heredada (la superclase)

La palabra clave super dentro de un constructor, sirve para invocar explícitamente a un constructor de la clase base. Esta llamada debe aparecer siempre como la primera instrucción del constructor, porque Java exige la inicialización de la superclase antes de cualquier la subclase.  Si el constructor de la subclase no invoca explícitamente a super, el compilador intenta insertar automáticamente una llamada a super() sin parámetros. Sin embargo, esto solo es posible si la clase base dispone de un constructor sin parámetros visible (public o protected)

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los atributos privados de la superclase forman parte físicamente de una instancia de la subclase en memoria. Cuando se crea un objeto de una subclase, Java reserva memoria para todo el objeto completo, lo que incluye tanto los atributos definidos en la clase base como los definidos en la subclase. Sin embargo, la palabra clave private restringe el acceso al código, no a la memoria. Por tanto, una subclase no puede leer ni modificar directamente un atributo privado de su superclase. La única forma correcta de interactuar con ese dato es a través de métodos definidos en la superclase, como el método saludar() o, si existiera, un método getNombre()

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.


### Respuesta
Cuando varias clases son compatibles con una superclase común, el código que trabaja con esa superclase no necesita conocer ni anticipar todas las posibles subclases existentes o futuras. Esto permite ampliar el sistema añadiendo nuevos tipos sin modificar el código ya escrito

public class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}

Soldado[] escuadra = new Soldado[4];
escuadra[0] = new Soldado("Carlos");
escuadra[1] = new Artillero("Luis", 5);
escuadra[2] = new Zapador("Miguel", 3);
escuadra[3] = new Medico("Ana", 2);

for (Soldado s : escuadra) {
    s.saludar();
}


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Es posible tener una referencia del supertipo que apunte a un objeto real de un subtipo:  si Artillero hereda de Soldado, entonces un objeto Artillero es un Soldado y puede ser almacenado en una referencia de tipo Soldado.

Sin embargo, desde una referencia del supertipo solo se pueden invocar los métodos definidos en ese supertipo, aunque el objeto real sea de un subtipo más específico. Por tanto, no es posible llamar directamente a métodos públicos exclusivos de Artillero (como getCohetes()) usando una referencia de tipo Soldado.

El upcasting consiste en tratar un objeto de un subtipo como si fuera del supertipo. El downcasting, en cambio, consiste en convertir una referencia del supertipo en una referencia del subtipo, y solo es seguro si el objeto real es efectivamente de ese subtipo.  Para evitar errores en tiempo de ejecución, Java proporciona el operador instanceof, que permite comprobar el tipo real del objeto antes de hacer la conversión

Soldado[] escuadra = new Soldado[3];
escuadra[0] = new Soldado("Carlos");
escuadra[1] = new Artillero("Luis", 5);
escuadra[2] = new Zapador("Miguel", 3);

for (Soldado s : escuadra) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Cohetes disponibles: " + a.getCohetes());
    }
}


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.


### Respuesta
Es accesible desde la propia clase,  cualquier clase del mismo paquete ylas subclases, incluso si están en otro paquete, sin hacerla totalmente pública. En el ejemplo, Zapador puede acceder a nombre en Soldado sin metodos, gracias a protected.

public class Soldado {

    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
}
## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En muchos lenguajes orientados a objetos existe una clase raíz (o clase base universal) de la que heredan todas las demás.
En Java, absolutamente todas las clases heredan de java.lang.Object así que para el compilador 
( public class Soldado{}) = (public class Soldado extends Object{})

Object aporta metodos como:

public class Object {
    public String toString();
    public boolean equals(Object obj);
    public int hashCode();
    protected Object clone();
    public final void wait();
    public final void notify();
}

Por lo que todos se pueden comparar (equals), convertir a texto (toString) o usarse de forma generica (decir que es tipo Object)

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es un mecanismo por el cual una clase puede heredar de más de una clase base al mismo tiempo. En Java, solo se puede conseguir usando interfaces, nunca directamente.

public interface Explosivos {
    void ponerBomba();
}

public interface Camuflaje {
    void camuflarse();
}

public class Zapador extends Soldado implements Explosivos, Camuflaje {

    @Override
    public void ponerBomba() {
        System.out.println("Poniendo bomba");
    }

    @Override
    public void camuflarse() {
        System.out.println("Camuflándose");
    }
}


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En Java las excepciones son objetos, y por tanto se pueden modelar, heredar y componer como cualquier otra clase

//Clase usuario
public class Usuario {
    private String id;
    private String nombre;

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

//Creamos excepcion que hereda de Usuario

public class UsuarioNoEncontradoException extends RuntimeException {

    private final Usuario usuario;

    // Constructor básico
    EncontradoException(Usuario usuario, String mensaje) {    public UsuarioNoEncontradoException(Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Constructor con causa subyacente
    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario, causa);
        this.usuario = usuario;
    }

    // Constructor completo: mensaje + causa
    public UsuarioNoEncontradoException(Usuario usuario, String mensaje, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
``
        super("Usuario no encontrado: " + usuario);
        this.usuario = usuario;
    }

    // Constructor con mensaje personalizado


//Por tanto, podemos hacer
public class ServicioUsuarios {

    public Usuario buscarUsuario(String id) {
        Usuario u = null; // imaginemos que viene de BD

        if (u == null) {
            throw new UsuarioNoEncontradoException(
                new Usuario(id, "Desconocido")
            );
        }

        return u;
    }
}


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
Herencia → relación “es-un” (IS-A)
Composición → relación “tiene-un” (HAS-A)

1-En el ejemplo siguiente, Usuario no es una lista de nada. Por tanto, si heredamos Lista solo por el metodo ordenar, el codigo está mal planteado, aunque funcione.

class Lista {
    void ordenar() { }
}
´´
class Usuario extends Lista {  }

2- Además, si heredamos, un cambio en la madre puede afectar a todas las hijas, poco seguro.
3- Por ultimo, una subclase accede a los atributos protected de la superclase. Si no es necesario, la encapsulacion se compromete.
4- Principio de Sustitución de Liskov (LSP): Si B hereda de A, un objeto B debe poder usarse donde se espere un A sin problemas.

Por ello, siempre que sea posible, composición sobre herencia. Herencia es favorable si B realmente es un tipo especial de A y no se compromete ningun detalle anterior.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Ver 10

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Ver 10


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
OPCION HERENCIA (trabajador/estudiante son personas, y  a las personas se les atribuyen datos)

//Clase superior persona
public class Persona {

    protected String dni;
    protected String nombre;

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
``
//subclases
public class Estudiante extends Persona {

    private String carrera;

    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }

    public String getCarrera() {
        return carrera;
    }
}

public class Trabajador extends Persona {

    private double salario;

    public Trabajador(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }

    public double getSalario() {
        return salario;
    }
}

OPCION COMPOSICION (trabajador/estudiante TIENEN Datos personales)

//Clase datos personales
public class DatosPersonales {

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

//Clases con datos 
public class Estudiante {

    private DatosPersonales datos;
    private String carrera;

    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }

    public String getDni() {
        return datos.getDni();
    }

    public String getNombre() {
        return datos.getNombre();
    }

    public String getCarrera() {
        return carrera;
    }
}

public class Trabajador {

    private DatosPersonales datos;
    private double salario;

    public Trabajador(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }

    public String getDni() {
        return datos.getDni();
    }

    public String getNombre() {
        return datos.getNombre();
    }

    public double getSalario() {
        return salario;
    }
}

//Ademas, debe crearse un objeto del tipo Datos personales
DatosPersonales dp = new DatosPersonales("12345678A", "Ana López");

Estudiante e = new Estudiante(dp, "Ingeniería Informática");
Trabajador t = new Trabajador(dp, 1800.0);