# 5. Integridad de datos: Por qué es casi imposible
	corromper un repo de Git.

Este es uno de los pilares que obsesionaban a Linus Torvalds. Para él, un sistema de control de versiones que pudiera perder datos o permitir cambios silenciosos sin que nadie se diera cuenta era, simplemente, basura técnica.

Aquí tienes la documentación detallada sobre la **Integridad de Datos en Git**.

### 1. La filosofía: "En Git nada se pierde, todo se identifica"

En la mayoría de los sistemas de archivos tradicionales, si un bit se "voltea" (debido a un fallo de disco o un error de red), el sistema puede que no lo note. Git fue diseñado bajo el principio de **desconfianza absoluta**. Todo lo que entra en la base de datos de Git es identificado mediante una suma de comprobación (**checksum**) antes de ser almacenado.

### 2. El mecanismo: Checksumming con SHA-1

Git utiliza un algoritmo llamado **SHA-1** (Secure Hash Algorithm 1). Este algoritmo toma el contenido de un archivo (o una estructura de directorios) y genera una cadena de **40 caracteres hexadecimales**.

Ejemplo de un hash de Git: `24b9da6552252987aa493b52f8696cd6d3b00d73`

**¿Por qué esto garantiza la integridad?**

- **Identificación por contenido:** Git no guarda los archivos por su nombre (ej. `foto.jpg`), sino por el hash de su contenido. Si cambias un solo espacio en blanco en un archivo de 1GB, el hash cambiará por completo.
- **Inmutabilidad:** Como el nombre del objeto es el hash de su contenido, es imposible cambiar el contenido de un archivo sin que cambie su nombre. Por lo tanto, no puedes "editar" la historia sin que el ID del commit cambie.

---

### 3. La Cadena de Confianza (El Grafo)

La integridad no solo se aplica a los archivos individuales (blobs), sino a todo el proyecto:

1. Los archivos se agrupan en un **Tree** (un objeto que representa una carpeta). El hash del Tree depende de los hashes de los archivos que contiene.
2. El **Commit** contiene el hash del Tree y el hash del **Commit Padre**.

**Consecuencia técnica:** Si intentas alterar un commit de hace 3 años, el hash de ese commit cambiará. Como el commit siguiente apuntaba al hash original, la conexión se rompe. Para "hackear" la historia, tendrías que recalcular todos los hashes desde ese momento hasta el presente. Esto es lo que hace que Git sea similar a una **Blockchain** primitiva.

---

### 4. Detección de errores de hardware

Git verifica los hashes en operaciones críticas:

- Al hacer un `git pull` o `push` (red).
- Al hacer un `git gc` (mantenimiento de disco).
- Al cambiar de rama.

Si el archivo en disco se corrompe por un fallo físico, Git comparará el contenido actual con su nombre (el hash) y gritará: *"Corrupt object!"*. Sabrás exactamente qué se rompió.

---

## Ejemplo para la Comprensión: El "Sello de Cera"

Imagina que envías una carta en un sobre con un **sello de cera único** que se rompe si alguien intenta abrirlo.

- **Sistemas antiguos:** Son como cartas normales. Alguien podría abrir el sobre, cambiar una palabra y cerrarlo. Tú nunca lo sabrías.
- **Git:** Es como si el contenido de la carta dictara la forma exacta del sello de cera. Si alguien cambia una sola letra, el sello ya no encaja en la cerradura. Es físicamente imposible que la carta alterada parezca la original.

---

## Ejercicio Práctico de Comprensión

**Situación de "Forense Digital":**
Tienes un repositorio con dos commits:

- Commit A (Hash: `aaa111`)
- Commit B (Padre: `aaa111`, Hash: `bbb222`)

Un virus modifica un archivo dentro del **Commit A**.

1. ¿Qué le pasa al hash del contenido de ese archivo?
2. ¿Qué le pasa al hash del Commit A?
3. ¿Por qué el Commit B ahora es "inválido"?

**Respuesta Técnica:**

1. El hash del archivo cambia totalmente.
2. Como el commit se calcula en base al contenido, el hash de A ya no sería `aaa111`, sino algo nuevo (ej. `zzz999`).
3. Porque el Commit B dice explícitamente: "Mi padre es `aaa111`". Al buscar al padre `aaa111` en la base de datos, Git no encontrará nada o verá que el historial ha sido manipulado. **La cadena de integridad se ha roto.**

---

### Resumen Documentado

- **Herramienta:** Hash SHA-1 (40 caracteres).
- **Alcance:** Archivos, directorios y mensajes de commit.
- **Resultado:** Es virtualmente imposible inyectar cambios en el historial sin que Git lo detecte instantáneamente.

[⬅️ Volver al Inicio](README.md)