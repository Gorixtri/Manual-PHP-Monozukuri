TALLER DE PROGRAMACIÓN II (12 Puntos)
Estudiante: [Tu Nombre y Apellido]
Cédula: [Tu Cédula]
Proyecto Base: Sistema de Cartelera Universitaria (PHP 8.2)

1. ENCAPSULAMIENTO
¿Qué es el encapsulamiento y qué problema resuelve?
El encapsulamiento es el principio de la Programación Orientada a Objetos (POO) que consiste en ocultar los datos internos (atributos) de un objeto y permitir su acceso o modificación solo a través de métodos controlados.

Problema que resuelve: Resuelve la falta de integridad de los datos. Sin encapsulamiento, cualquier parte del código podría alterar los datos de un objeto de forma accidental o maliciosa (por ejemplo, asignar un año negativo a una película o un nombre vacío), rompiendo la lógica del sistema. También oculta la complejidad interna para que el programador que use la clase no tenga que saber cómo funciona por dentro, solo cómo usarla.

Tabla: Modificadores de Acceso en PHP

Modificador ¿Desde dónde se puede acceder?  Ejemplo en nuestro proyecto
public  Desde cualquier lugar (dentro de la clase, clases hijas y desde fuera del objeto).  El método obtenerDescripcion() es público para que la vista HTML pueda llamarlo.
private Solo desde dentro de la misma clase que lo define. Ni siquiera las clases hijas pueden acceder directamente a él.   El atributo $titulo es privado (o readonly) para evitar que alguien cambie el nombre de la película después de crearla.
protected   Desde dentro de la clase y desde las clases hijas (herencia), pero no desde fuera.  Un atributo $precioBase en una clase Entrada sería protected para que la clase hija EntradaVIP pueda usar ese precio y sumarle impuestos.
¿Qué son los getters y los setters? ¿Por qué no basta con poner todos los atributos como public?

Getters: Son métodos públicos (ej. getTitulo()) que sirven para leer el valor de un atributo privado.

Setters: Son métodos públicos (ej. setTitulo($nuevoTitulo)) que sirven para asignar o modificar el valor de un atributo privado, permitiendo validar el dato antes de guardarlo.

¿Por qué no usar todo public? Porque si un atributo es public, pierdes el control. Cualquiera podría hacer $pelicula->anio = "Hola"; (rompiendo el tipo de dato) o $pelicula->precio = -50;. Usando setters, puedes poner reglas: "Si el año es menor a 1890, lanza un error". El encapsulamiento protege la lógica del negocio.

En nuestro sistema (Cartelera), ¿cuáles serían los atributos públicos y privados?

Privados: $titulo, $anio, $genero y $nivelExplosiones. Deben ser privados para que nadie modifique la identidad de la película desde el HTML.

Públicos: Los métodos como obtenerDescripcion(), getTitulo() o getAnio(). Son públicos porque la vista (el mesero) necesita interactuar con ellos para mostrar la información en la web.

2. ABSTRACCIÓN
¿Qué es la abstracción? Den un ejemplo.
La abstracción es el proceso de enfocarse en lo esencial de un objeto del mundo real, ignorando los detalles irrelevantes para el sistema que estamos construyendo. Es quedarse con el "qué es" y no con el "cómo se hace".

Ejemplo: Si modelamos una Pelicula para un sistema de cartelera, abstraemos sus atributos esenciales: Título, Año y Género. Ignoramos (abstraemos) detalles como el color de los zapatos del director, cuántos cafés se tomaron en el rodaje o el peso del rollo de película, porque no son relevantes para el sistema.

Lista de atributos esenciales para 2 clases de nuestro proyecto:

Clase Pelicula:

Título (string)

Año de estreno (int)

Género (string)

Clase Actor:

Nombre artístico (string)

Edad (int)

Nacionalidad (string)

¿En qué se diferencian la abstracción y el encapsulamiento?

Abstracción: Es el proceso mental de diseño. Consiste en decidir qué datos y comportamientos va a tener la clase (quedarse con lo importante).

Encapsulamiento: Es la técnica de implementación en el código. Consiste en cómo protegemos esos datos que ya decidimos abstraer, usando modificadores de acceso (private, public) y métodos para evitar que se corrompan.

3. HERENCIA
¿Qué es la herencia? ¿Qué hereda una clase hija de su clase padre y qué no puede usar?
Es el mecanismo por el cual una clase (hija) adquiere las propiedades y métodos de otra clase (padre), fomentando la reutilización del código.

¿Qué hereda? Hereda todos los atributos y métodos que sean public y protected, así como los métodos private (aunque no puede acceder directamente a ellos, hereda su comportamiento).

¿Qué NO puede usar? Una clase hija no puede acceder directamente a los atributos o métodos declarados como private en la clase padre. Para usarlos, el padre debe exponerlos mediante getters/setters públicos.

Diferencia entre relación "es un" y "tiene un" (con ejemplos):

"Es un" (Herencia): Es una relación de especialización. La clase hija es un tipo de la clase padre.

Ejemplo 1: Un Auto es un Vehículo.

Ejemplo 2: Un Perro es un Animal.

Ejemplo 3: Una PeliculaDeAccion es una Pelicula.

"Tiene un" (Composición/Agregación): Es una relación de pertenencia. Una clase contiene o tiene otra clase.

Ejemplo 1: Un Auto tiene un Motor.

Ejemplo 2: Una Pelicula tiene un Director.

Ejemplo 3: Un Estudiante tiene una Mochila.

¿Para qué sirven extends y parent::__construct() en PHP o Python?

extends: Es la palabra clave que se usa para declarar que una clase heredará de otra. (Ej: class PeliculaDeAccion extends Pelicula).

parent::__construct(): Se usa dentro del constructor de la clase hija para llamar al constructor de la clase padre. Sirve para no tener que volver a escribir el código que inicializa los atributos comunes (título, año, etc.) y asegurar que el padre se inicialice correctamente antes de añadir los nuevos atributos de la hija.

2 Ventajas y 1 Riesgo de usar Herencia:

Ventaja 1 (Reutilización): Evitas duplicar código. Escribes el método una vez en el padre y todas las hijas lo usan.

Ventaja 2 (Mantenimiento): Si hay un error en la lógica del padre, lo corriges en un solo archivo y se arregla en todas las clases hijas automáticamente.

Riesgo (Acoplamiento fuerte): Si se abusa de la herencia y se crean jerarquías profundas, un cambio en la clase padre puede romper (o afectar) de forma inesperada a todas las clases hijas, haciendo el código frágil y difícil de mantener.
<?php
declare(strict_types=1);

// ==========================================
// CLASE PADRE
// ==========================================
class Pelicula 
{
    // 1. Atributos privados (Encapsulamiento)
    private string $titulo;
    private int $anio;
    private string $genero;

    public function __construct(string $titulo, int $anio, string $genero) 
    {
        $this->titulo = $titulo;
        $this->anio = $anio;
        $this->genero = $genero;
    }

    // 2. Getters y Setters (Acceso controlado)
    public function getTitulo(): string {
        return $this->titulo;
    }

    public function setTitulo(string $titulo): void {
        // Validación en el setter
        if (empty($titulo)) {
            throw new Exception("El título no puede estar vacío.");
        }
        $this->titulo = $titulo;
    }

    public function getAnio(): int {
        return $this->anio;
    }

    public function setAnio(int $anio): void {
        $this->anio = $anio;
    }

    public function getGenero(): string {
        return $this->genero;
    }

    public function obtenerDescripcion(): string {
        return "La película {$this->titulo} se estrenó en {$this->anio}.";
    }
}

// ==========================================
// CLASE HIJA (Herencia)
// ==========================================
class PeliculaDeAccion extends Pelicula 
{
    // Atributo exclusivo de la clase hija
    private int $nivelExplosiones;

    public function __construct(string $titulo, int $anio, int $nivelExplosiones) 
    {
        // Llamamos al constructor del padre (Herencia)
        parent::__construct($titulo, $anio, "Acción");
        $this->nivelExplosiones = $nivelExplosiones;
    }

    // Getter y Setter para el nuevo atributo
    public function getNivelExplosiones(): int {
        return $this->nivelExplosiones;
    }

    public function setNivelExplosiones(int $nivel): void {
        $this->nivelExplosiones = $nivel;
    }

    // Polimorfismo: Sobrescribimos el método
    public function obtenerDescripcion(): string {
        return parent::obtenerDescripcion() . " ¡Nivel de explosiones: {$this->nivelExplosiones}/10!";
    }
}

// ==========================================
// PRUEBA DEL PROYECTO
// ==========================================
$miPelicula = new Pelicula("Matrix", 1999, "Ciencia Ficción");
echo $miPelicula->obtenerDescripcion() . "<br>";

// Usando la clase hija
$miPeliculaAccion = new PeliculaDeAccion("John Wick", 2014, 10);
echo $miPeliculaAccion->obtenerDescripcion() . "<br>";

// Demostración de Setter (Encapsulamiento)
$miPelicula->setTitulo("Matrix Recargado");
echo "Nuevo título: " . $miPelicula->getTitulo();
?>