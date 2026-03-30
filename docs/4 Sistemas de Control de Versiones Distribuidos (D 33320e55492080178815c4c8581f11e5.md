# 4. Sistemas de Control de Versiones Distribuidos (DVCS): La arquitectura de Git.

Llegamos al punto donde la arquitectura rompe con lo tradicional. Si el modelo centralizado (CVCS) era un "dictador" que guardaba toda la verdad, el **Sistema de Control de Versiones Distribuido (DVCS)** es una "democracia" donde cada ciudadano tiene las leyes completas en su bolsillo.

### 1. ¿Qué significa "Distribuido"?

En un sistema como Git, **Mercurial** o **Bazaar**, cuando haces un "clone", no estás simplemente descargando la última versión de los archivos (como en SVN). Estás descargando **la base de datos completa**.

Cada desarrollador tiene una copia local que contiene:

- Cada archivo del proyecto.
- Cada cambio realizado en la historia (commits).
- Cada rama y cada etiqueta (tags).

---

### 2. La Arquitectura Interna de Git

A diferencia de otros DVCS, la arquitectura de Git se basa en un **Grafo Acíclico Dirigido (DAG)**. Esto suena complejo, pero técnicamente significa que:

- **Grafo:** Es una red de puntos (commits) conectados.
- **Acíclico:** No hay círculos; el tiempo siempre avanza o se ramifica, pero un commit no puede ser su propio abuelo.
- **Dirigido:** Cada commit sabe quién es su padre (o padres, en caso de un merge).

### Los Componentes Clave:

1. **El Repositorio Local:** Una carpeta oculta llamada `.git` que vive en tu disco duro. Es tu servidor personal.
2. **Los Objetos de Git:** Git transforma tus archivos en objetos (blobs, trees, commits) identificados por un código único (Hash).
3. **Los Remotos:** Son simplemente "espejos" del proyecto en otros servidores (como GitHub o el servidor de un compañero) con los que puedes intercambiar datos.

---

### 3. ¿Por qué esta arquitectura es superior? (Documentación Técnica)

- **Independencia de Red:** El 99% de tus comandos (`git commit`, `git log`, `git diff`, `git branch`) no tocan internet. Son instantáneos porque consultan tu propia base de datos local.
- **Integridad Total:** Como cada nodo tiene la historia completa, si el servidor central explota, cualquiera de los 100 programadores del equipo puede subir su copia local y restaurar el servidor al 100% sin perder ni un solo mensaje de commit.
- **Colaboración Flexible:** Puedes compartir cambios con un compañero directamente (punto a punto) sin pasar por un servidor central.

---

### Ejemplo para la Comprensión: "El Backup Humano"

Imagina un equipo de 3 personas: Ana, Bob y Carlos.

1. Ana crea el proyecto y lo sube a GitHub.
2. Bob y Carlos hacen un `git clone`.
3. De repente, GitHub es hackeado y borran todo el repositorio.

**En un sistema centralizado (SVN):** Ana, Bob y Carlos tienen el código actual, pero han perdido toda la historia de por qué se hicieron los cambios el año pasado. El historial ha muerto.
**En Git (DVCS):** Ana simplemente ejecuta un comando para "empujar" (push) su copia local a un nuevo servidor. Como su copia incluía todo el historial desde el día 1, el proyecto se recupera íntegramente en segundos.

---

### Ejercicio Práctico y Ejercicio de Reflexión

**Ejercicio Mental: El concepto de "Clone"**
Si un repositorio de Git pesa 500MB en el servidor, y tú haces un `git clone`:

1. ¿Cuánto pesará la carpeta `.git` en tu computadora? (Aproximadamente los mismos 500MB).
2. ¿Podrías ver el código de hace 3 años estando en una cabaña en la montaña sin Wi-Fi? (Sí, porque la base de datos de 3 años atrás está en esos 500MB locales).

**Ejercicio de Comandos (Simulado):**
Si quieres enviar tus cambios a un servidor, usas `git push`. Si quieres traer los cambios de otros, usas `git pull`.

- *Reflexión:* ¿Por qué en SVN no existen estos comandos y solo existe "Update" y "Commit"?
    - *Respuesta:* Porque en SVN solo hay un servidor. En Git, como tú también eres un "servidor", necesitas comandos específicos para sincronizar dos bases de datos completas.

---

### Resumen de la Arquitectura

- **Cada cliente es un servidor:** No hay jerarquía técnica, solo administrativa.
- **Basado en el Grafo (DAG):** Permite ramificaciones complejas y fusiones seguras.
- **Optimizado para la seguridad:** La historia es inmutable y redundante por diseño.

[⬅️ Volver al Inicio](../README.md)