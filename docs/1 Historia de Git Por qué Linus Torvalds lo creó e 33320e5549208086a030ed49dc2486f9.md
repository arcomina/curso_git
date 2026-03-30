# 1. Historia de Git: Por qué Linus Torvalds lo creó en 15 días.

Durante los primeros 10 años del Kernel de Linux, el desarrollo era "artesanal". Linus Torvalds recibía parches por correo electrónico, los revisaba manualmente y los integraba. No había un sistema de control de versiones que escalara al nivel de Linux. Herramientas como **CVS** eran rechazadas por Linus porque las consideraba "lentas, corruptibles y diseñadas con una filosofía errónea".

### El Detonante: La Crisis de BitKeeper (2005)

En 2002, el proyecto Linux adoptó **BitKeeper**, un sistema propietario (de pago) que les regaló licencias gratuitas. BitKeeper era revolucionario porque era **distribuido**. Sin embargo, en abril de 2005, un desarrollador de Linux (Andrew Tridgell) intentó hacer ingeniería inversa al protocolo de BitKeeper. La empresa (BitMover) se enfureció y revocó la licencia gratuita para todo el proyecto Linux.

Linus se quedó sin herramientas un viernes. En lugar de volver al correo electrónico, decidió que escribiría su propio sistema.

### El Reto de los 15 Días

Linus Torvalds estableció objetivos que nadie más había logrado combinar:

1. **Velocidad:** Hacer un "patch" debía tomar milisegundos.
2. **Diseño Distribuido:** Nada de un servidor central obligatorio (copias completas en cada nodo).
3. **Salvaguarda contra la corrupción:** El sistema debía saber si un solo bit del historial cambiaba (aquí entra el hashing).

**Cronología de la hazaña:**

- **Día 1 (3 de abril):** Empieza a escribir el código en C.
- **Día 4:** Git ya es capaz de hacer commits de sí mismo (self-hosting).
- **Día 10:** El rendimiento supera a todo lo conocido.
- **Día 15:** Se publica oficialmente y el Kernel de Linux se migra a Git.

---

## Documentación Técnica: Por qué es tan diferente

### A. Snapshots, no Deltas

La mayoría de los sistemas antiguos (como SVN) guardaban una lista de cambios por archivo. Git no. Git toma una "fotografía" (Snapshot) de todo el proyecto. Si un archivo no cambió, Git no lo duplica, solo pone un puntero al archivo anterior que ya tiene guardado.

### B. El Grafo Acíclico Dirigido (DAG)

Git no es una línea de tiempo. Es un **Grafo**. Cada commit apunta a su "padre". Esto permite que las ramas (branches) sean simplemente punteros a un nodo del grafo, haciendo que crear una rama sea instantáneo (solo pesa 41 bytes).

### Ejemplo 1: El concepto de "Snapshot"

Imagina que tienes un libro de 3 páginas:

- **Sistemas antiguos:** Guardan "En la página 2, línea 3, se cambió la palabra 'Hola' por 'Adiós'".
- **Git:** Guarda una copia completa de la página 1, la nueva página 2, y un puntero a la página 3 original. Para Git, el contenido es lo que importa, no el cambio.

### Ejemplo 2: Velocidad de Ramificación

En otros sistemas, crear una rama significaba copiar toda la carpeta del proyecto a otra ubicación en el servidor.
En Git, crear una rama es como poner un "post-it" en una página de un libro. No copias el libro, solo marcas dónde estás.

## Ejercicio Práctico de Comprensión

Para que entiendas la filosofía de "integridad" y "snapshots" de Linus, analiza este ejercicio mental:

**Situación:** Tienes un archivo llamado `notas.txt` con el texto "Versión 1". Haces un commit. Luego cambias el texto a "Versión 2" y haces otro commit.

**Preguntas para reflexionar:**

1. Si un hacker entra en tu servidor y cambia una sola letra del primer commit ("Versión 1" por "Versión X"), ¿cómo sabe Git que el historial fue manipulado?
    - *Respuesta:* Porque el ID del commit (SHA-1) se calcula en base al contenido. Si el contenido cambia, el ID ya no coincide con el puntero del siguiente commit. El grafo se rompe.
2. ¿Por qué Linus prefirió que Git fuera "difícil de usar" al principio pero "imposible de romper"?
    - *Respuesta:* Porque para el Kernel de Linux, la integridad del código es más importante que la comodidad del programador.

### Resumen Documentado

- **Nombre:** "Git" (argot británico para "imbécil" o "persona desagradable"). Linus dijo: "Soy un egocéntrico, así que nombro mis proyectos como yo: Linux, y ahora Git".
- **Lenguaje:** C (para máxima velocidad y acceso a bajo nivel del sistema de archivos).
- **Legado:** Hoy el 90% del software del mundo se gestiona con Git.