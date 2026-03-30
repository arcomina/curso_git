# 3. Sistemas de Control de Versiones Centralizados (CVCS): Cómo funcionan SVN y Perforce.

Para entender por qué Git es como es, debemos analizar a sus "padres" y rivales: los **Sistemas de Control de Versiones Centralizados (CVCS)**. Durante décadas, este fue el estándar de la industria, y nombres como **Subversion (SVN)** y **Perforce** aún resuenan en muchas empresas.

### ¿Qué es un CVCS?

En un sistema centralizado, existe **un único servidor** que actúa como la "fuente de la verdad". Este servidor contiene la base de datos con todas las versiones de cada archivo que se ha modificado.

Los desarrolladores no tienen el historial del proyecto en sus computadoras; solo tienen una **"Working Copy"** (copia de trabajo), que es una foto del estado actual del código en un momento específico.

---

### Cómo funcionan SVN y Perforce (El flujo de trabajo)

1. **Check-out (Obtención):** El programador le pide al servidor la última versión del código. El servidor le envía los archivos actuales.
2. **Edit (Edición):** El programador modifica los archivos localmente.
3. **Commit / Check-in (Entrega):** El programador envía sus cambios al servidor. El servidor crea una nueva versión (ej. de la v5 a la v6) y la guarda en su base de datos central.

### El caso de Subversion (SVN):

SVN fue el rey antes de Git. Se basa en **directorios**. Si quieres crear una rama (branch), SVN físicamente crea una copia de la carpeta en el servidor. Es un sistema basado en "Diferencias" (Deltas): solo guarda qué líneas cambiaron respecto a la versión anterior.

### El caso de Perforce (Helix Core):

Perforce es extremadamente potente para archivos binarios gigantes (común en la industria de videojuegos como Epic Games o Ubisoft). A diferencia de SVN, Perforce suele usar un modelo de **"Locking"** (Bloqueo): si tú estás editando un archivo, el servidor lo bloquea para que nadie más pueda tocarlo hasta que tú termines.

---

### La Documentación de los "Puntos de Dolor" (Por qué Linus los odiaba)

Aunque son fáciles de entender, los CVCS tienen debilidades críticas que Git resolvió:

- **Punto Único de Fallo:** Si el disco duro del servidor central se corrompe y no hay un backup perfecto, **se pierde toda la historia del proyecto**. Los programadores solo tienen la última versión, pero nadie sabe qué pasó hace un mes.
- **Dependencia Total de la Red:** ¿Quieres ver quién cambió una línea de código hace un año? Tienes que estar conectado a internet. ¿Quieres hacer un commit para salvar tu trabajo antes de irte a casa pero el servidor de la oficina está caído? No puedes.
- **Lentitud en Ramas:** En SVN, crear una rama para probar una idea loca es costoso y lento porque implica operaciones en el servidor. Esto hace que los programadores tengan miedo de crear ramas.

---

### Ejercicio de Comprensión: "El Gran Apagón"

**Escenario:**
Trabajas en una empresa que usa **SVN**. De repente, un rayo cae en el centro de datos y el servidor central queda calcinado. No hay copias de seguridad de la base de datos de versiones.

**Preguntas para reflexionar:**

1. Tú tienes el código en tu laptop y tu compañero también. ¿Pueden reconstruir el historial de mensajes de commit y versiones de hace dos años?
2. Si intentas hacer un "update" para ver qué hizo tu compañero ayer, ¿qué sucede?

**Respuesta Técnica:**

1. **No.** Solo tienen la "foto" actual. El historial (quién, cuándo y por qué) vivía solo en el servidor y ha muerto con él.
2. El sistema fallará. No hay "diálogo" posible porque el único interlocutor (el servidor) no existe.

---

### Resumen para tu ruta:

- **SVN/Perforce:** Centralizados. El servidor es el dueño.
- **Modelo:** Cliente-Servidor puro.
- **Riesgo:** Si el servidor cae, el historial muere y la productividad se detiene.

[⬅️ Volver al Inicio](../README.md)