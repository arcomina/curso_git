# 10. Concepto de Inmutabilidad: Por qué los objetos en Git no se modifican, se crean nuevos.

Una de las piezas más elegantes de la ingeniería de Git. Si vienes de usar carpetas normales donde "guardas" un archivo encima de otro, este concepto te volará la cabeza: **En Git, nada se sobrescribe.**

Aquí tienes la documentación detallada sobre el **Concepto de Inmutabilidad**.

### 1. ¿Qué es la Inmutabilidad?

En informática, algo **inmutable** es algo que no puede cambiar después de ser creado. En Git, una vez que un objeto (un archivo, una carpeta o un commit) se guarda en la base de datos (la carpeta `.git/objects`), ese objeto es **eterno e inalterable**.

Si modificas una sola coma en un archivo, Git **no edita** el objeto anterior. En su lugar, crea un **objeto completamente nuevo** con un nuevo nombre (un nuevo Hash SHA-1).

---

### 2. ¿Por qué Git funciona así? (Documentación Técnica)

Esta decisión de diseño de Linus Torvalds tiene tres razones fundamentales:

- **Integridad del Historial:** Si pudieras modificar un commit pasado, podrías alterar la historia del proyecto (por ejemplo, borrar un error que cometiste o insertar código malicioso) sin que nadie se diera cuenta. Al ser inmutable, cualquier cambio genera un nuevo ID, lo que delata la manipulación.
- **Seguridad en la Concurrencia:** Como los archivos viejos nunca cambian, es muy seguro que varios programadores trabajen a la vez. No hay riesgo de que alguien "borre" lo que tú estás leyendo, porque lo que tú lees es un objeto fijo en el tiempo.
- **Eficiencia en la Recuperación:** Git puede saltar entre versiones de hace 5 años y versiones de hoy instantáneamente porque todas coexisten en la base de datos como objetos independientes. No tiene que "deshacer" cambios para volver atrás.

---

### 3. El mecanismo: El direccionamiento por contenido

Git no guarda archivos por su nombre (`index.html`), sino por su **Hash**.

1. Si el contenido es "Hola", el Hash es `abc123`.
2. Si cambias el contenido a "Hola mundo", el Hash es `xyz789`.

Como el nombre del archivo *es* el contenido, para "cambiar" el archivo tendrías que cambiar su nombre. Por definición, si el nombre cambia, **es un objeto diferente**. El objeto `abc123` sigue existiendo intacto en tu disco.

---

## Ejemplo para la Comprensión: El Libro de Actas vs. La Pizarra

- **Sistemas No Inmutables (La Pizarra):** Escribes algo, te equivocas, pasas el borrador y escribes encima. La información original desapareció para siempre. Solo queda lo último.
- **Git (El Libro de Actas):** Si te equivocas en una página, **no puedes arrancarla ni borrarla**. Tienes que pasar a la siguiente página y escribir la corrección. Si alguien quiere saber qué escribiste originalmente, solo tiene que volver a la página anterior. El libro solo crece, nunca se encoge ni se tacha.

---

## Ejercicio Práctico de Comprensión Técnica

**Situación:**
Tienes un archivo `config.txt` que pesa 1KB. Haces 1000 cambios pequeños y 1000 commits.

**Preguntas para reflexionar:**

1. Si Git fuera puramente inmutable y guardara 1000 copias de 1KB, ¿el repo pesaría 1MB? (En teoría sí).
2. ¿Por qué entonces los repos de Git no pesan gigabytes tras miles de commits?

**Respuesta Técnica:**
Aquí es donde entra la "magia" de Git. Aunque filosóficamente es inmutable y crea objetos nuevos, Git tiene un proceso llamado **Garbage Collection (git gc)**. Periódicamente, Git analiza esos 1000 objetos inmutables, se da cuenta de que son casi idénticos, y los empaqueta en un **Packfile**, donde guarda uno completo y los demás como "deltas" comprimidos.

**Conclusión:** Git se comporta como inmutable para el usuario (seguridad), pero se comporta como un sistema de diferencias para el disco duro (eficiencia).

---

## Resumen Documentado

- **Regla de oro:** Los objetos existentes en `.git/objects` no se tocan.
- **Consecuencia:** Puedes hacer "deshacer" (undo) de casi cualquier cosa en Git, porque el estado anterior sigue guardado en alguna parte del disco.
- **Hash:** Es la garantía de que el objeto es el que dice ser.

[⬅️ Volver al Inicio](../README.md)