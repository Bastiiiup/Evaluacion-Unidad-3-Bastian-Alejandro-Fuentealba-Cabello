# Sistema de Asistencia para Sede

## Descripción del Sistema

Este sistema controla el ingreso y salida de personas (empleados y visitantes) a una sede, y gestiona su asistencia a eventos. Su objetivo es mantener un registro claro y centralizado de la presencia de individuos, facilitar la creación de personas y eventos, y tambien se mostrara un registro para ver los datos.

## Patrones de Diseño Aplicados y su Justificación

El proyecto utiliza 4 patrones de diseño para mejorar su estructura y flexibilidad:

### 1. Singleton (Creacional)

* **Por que:** Asegura que solo exista **una única instancia** del `SistemaAsistencia` para gestionar todos los datos. Esto evita confusiones y garantiza la coherencia de la información global (personas, eventos, registros).
* **Como y Donde:** La clase `SistemaAsistencia` (en `sistema`) tiene un constructor privado y un método estático `getInstancia()` que siempre devuelve la misma instancia. En `main.java`, se accede a ella con `SistemaAsistencia.getInstancia()`.

### 2. Prototype (Creacional)

* **Porque:** Permite crear nuevos objetos (`Persona`, `Evento`) copiando un objeto existente (usando una plantilla). Esto es más eficiente que crear un objeto desde cero, ya que se demorarian mas.
* **Cómo y Dónde:** Se usa una interfaz `Clonable` (en `model`) que implementan `Persona` y `Evento` para definir el método `clonar()`. En `main.java`, al crear personas o eventos, se selecciona una plantilla base y se clona: `nuevaPersona = prototipoBase.clonar()`.

### 3. Bridge (Estructural)

* **Por que:** Separa el "que" de las notificaciones (el mensaje) del "cómo" se envían (el medio). Así, puedes cambiar cómo envías una notificación (ej., a consola o a email) sin modificar el contenido del mensaje.
* **Como y Donde:** Las clases `Notificacion` y `NotificacionEvento` (en `notificacion`) definen la abstracción del mensaje. Las clases `Notificador` y `NotificadorConsola` (en `notificacion`) definen la implementación del envío. Una `Notificacion` tiene una referencia a un `Notificador`, permitiendo intercambiar el método de envío.

### 4. Iterator (Comportamiento)

* **Porque:** Proporciona una forma estándar y limpia de recorrer colecciones de objetos (listas de personas, eventos, registros) sin necesidad de saber cómo están almacenados internamente. Esto mantiene el código que muestra los datos simple y desacoplado de la estructura interna.
* **Como y Donde:** Se utiliza implicitamente a través de las listas de Java (`java.util.List`). En `main.java`, al mostrar los listados de personas, eventos o registros, se usan bucles `for-each` que se basan en el Iterator interno de las listas:
    ```java
    for (Persona persona : sistema.getPersonasRegistradas()) {
        System.out.println(persona);
    }
    ```

## Instrucciones de Uso

### Compilación y Ejecución

1.  **Descarga** el proyecto.
2.  **Abre** el proyecto en tu IDE netbeans.
3.  **Ejecuta** la clase `app.main`.

Alternativamente, desde la **línea de comandos**:

1.  Navega a la carpeta raíz del proyecto (`SistemaSede`).
2.  **Compila**:
    ```bash
    javac -d bin src/app/*.java src/model/*.java src/notificacion/*.java src/sistema/*.java
    ```
3.  **Ejecuta**:
    ```bash
    java -cp bin app.main
    ```

El sistema te mostrará un menú interactivo en la consola para empezar a operar.
