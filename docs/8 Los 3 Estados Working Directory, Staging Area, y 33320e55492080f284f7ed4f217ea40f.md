# 8. Los 3 Estados: Working Directory, Staging Area, y Repository.

Si hay un concepto que causa confusión en los principiantes pero que otorga un poder absoluto a los expertos, es este. Entender los **3 estados** es entender el "proceso de fabricación" de un commit.

En otros sistemas, tú cambias un archivo y lo envías al servidor. En Git, hay capas intermedias que te permiten ser mucho más preciso.

Para que Git gestione tu proyecto, tus archivos deben pasar por una "aduana" antes de ser grabados permanentemente. Estos son los tres lugares (o estados) donde puede residir tu código:

### 1. Working Directory (Directorio de Trabajo)

Es lo que ves en tu carpeta de archivos. Son los archivos que estás editando actualmente en tu editor de código (VS Code, Vim, etc.).

- **Estado técnico:** "Modified" (Modificado). Git sabe que cambiaste el archivo, pero aún no ha guardado ese cambio en su base de datos. Si tu PC se apaga ahora, Git no podrá recuperar esos cambios.

### 2. Staging Area (Área de Preparación / El Index)

Es una zona intermedia, un "limbo" antes del commit. Aquí es donde preparas lo que quieres que vaya en la siguiente foto (snapshot).

- **Estado técnico:** "Staged" (Preparado). Has marcado el archivo para que forme parte de tu próximo commit usando el comando `git add`.
- **¿Por qué existe?** Porque te permite seleccionar **qué** cambios quieres guardar. Puedes haber modificado 10 archivos, pero solo querer hacer un commit de 2 que arreglan un bug específico.

### 3. Repository / Git Directory (El Almacén)

Es donde Git guarda permanentemente los snapshots en la carpeta oculta `.git`.

- **Estado técnico:** "Committed" (Confirmado). Los datos están almacenados de forma segura en tu base de datos local. Tienen un Hash (ID) único y son inmutables.

---

## Documentación Técnica: El flujo de los datos

El ciclo de vida estándar de un cambio en Git sigue este orden:

1. **Modificas** un archivo en tu **Working Directory**.
2. **Preparas** el archivo (`git add`) para que pase al **Staging Area**. Git calcula el hash del contenido en este momento.
3. **Confirmas** los cambios (`git commit`). Git toma todo lo que está en el Staging Area y crea un objeto Commit permanente en el **Repository**.

---

## Ejemplo para la Comprensión: La sesión de fotos

Imagina que eres un fotógrafo de modelos:

- **Working Directory:** Es el camerino. Los modelos se están cambiando, maquillando y moviendo. Hay caos. Tú ves el movimiento, pero no has disparado la cámara.
- **Staging Area:** Es el escenario de la foto. Llamas a dos modelos y les pides que posen en una posición específica. Todavía no has disparado, pero ya elegiste quién sale y cómo.
- **Repository:** ¡CLIC! Disparas la cámara. La foto ya está en la tarjeta de memoria. No importa si los modelos se mueven ahora o se van a casa; esa foto (commit) es eterna y muestra exactamente lo que estaba en el escenario en ese instante.

---

## Ejercicio Práctico: Control de precisión

**Situación:**
Has estado trabajando toda la mañana.

1. Arreglaste un error en el archivo `login.py`.
2. Empezaste a escribir una nueva funcionalidad en `perfil.py`, pero aún no la terminas.

**El reto:** Quieres guardar el arreglo del error (login.py) pero **no** quieres que la funcionalidad incompleta (perfil.py) se guarde todavía.

**Comandos a ejecutar:**

1. `git add login.py` (Esto mueve solo el arreglo al **Staging Area**).
2. `git status` (Verás que `login.py` está en verde/staged y `perfil.py` está en rojo/modified).
3. `git commit -m "Arreglado error en el login"` (Solo se guarda el login).

**Reflexión:** En un sistema antiguo (SVN), habrías tenido que guardar ambos o esperar a terminar todo. Git te permite ser un "curador" de tu propio historial.

---

## Resumen de estados para tu ruta:

- **Modified:** Cambios realizados pero no rastreados para el próximo commit.
- **Staged:** Cambios marcados para ser incluidos en la próxima fotografía.
- **Committed:** Cambios guardados de forma segura en la base de datos local.
  
[⬅️ Volver al Inicio](../README.md)