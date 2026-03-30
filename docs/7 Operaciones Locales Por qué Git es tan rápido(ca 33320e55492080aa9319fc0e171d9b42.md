# 7. Operaciones Locales: Por qué Git es tan rápido(casi todo ocurre en tu disco).

Si alguna vez has usado sistemas como SVN, habrás notado que a veces hay un "lag" o retraso al ver el historial o hacer un commit. En Git, eso no existe. La razón es técnica y arquitectónica: **Git es un sistema de archivos local primero.**

Aquí tienes la documentación detallada sobre las **Operaciones Locales**.

### 1. El concepto "Offline-First"

En la mayoría de los sistemas antiguos, el "cliente" (tu PC) es tonto y el "servidor" es el inteligente. En Git, **tu PC es el servidor**. Cuando clonas un repositorio, traes toda la base de datos a tu disco duro (dentro de la carpeta `.git`).

**Consecuencia técnica:** Casi todos los comandos que ejecutas no necesitan salir a internet ni consultar una red local. Solo leen archivos de tu propio disco duro (preferiblemente un SSD), lo cual es órdenes de magnitud más rápido que cualquier conexión de red.

---

### 2. ¿Qué operaciones son puramente locales?

Para que entiendas la magnitud de esto, mira lo que Git hace sin necesidad de Wi-Fi:

- **`git log`:** Para mostrarte los commits de hace 3 años, Git no le pregunta a GitHub. Lee su propia base de datos local. Por eso el historial aparece instantáneamente.
- **`git diff`:** Para comparar lo que escribiste hoy con lo que había hace un mes, Git compara dos objetos que ya tiene en tu disco.
- **`git commit`:** Esta es la clave. Hacer un commit en Git es simplemente crear un objeto en tu disco local y mover un puntero. No hay transferencia de archivos a un servidor externo en este paso.
- **`git branch`:** Como veremos más adelante, crear una rama es solo escribir 41 bytes en un archivo local.

### 3. El cuello de botella se elimina

En un sistema centralizado, si 50 programadores intentan hacer un commit al mismo tiempo, el servidor central se ralentiza procesando las peticiones.
En Git, esos 50 programadores hacen sus commits en sus propios discos. El único momento en que "compiten" por velocidad es cuando deciden sincronizar (hacer `push`), pero para entonces, todo el trabajo pesado de computar hashes y snapshots ya se hizo localmente.

---

## Ejemplo para la Comprensión: La Biblioteca Personal

- **Sistemas Centralizados (SVN):** Es como vivir en un pueblo donde solo hay una **Biblioteca Pública**. Si quieres leer un libro (ver un commit antiguo), tienes que caminar hasta allí, pedirlo, y si hay mucha gente, esperar. Si la biblioteca cierra (se cae el servidor), nadie puede leer nada.
- **Git:** Es como si al entrar al pueblo, te dieran una **fotocopiadora mágica** que copia toda la biblioteca y la pone en tu casa. Si quieres leer un libro, solo estiras el brazo. Es instantáneo, no dependes del horario de la biblioteca y puedes leer incluso si hay una tormenta afuera.

---

## Ejercicio Práctico: Prueba de Velocidad "Cero Red"

Si quieres comprobar esto por ti mismo, intenta este ejercicio (simulado o real):

1. Desactiva el Wi-Fi de tu computadora.
2. Entra en un repositorio de Git que tengas.
3. Ejecuta: `git log --graph --oneline --all`
4. Ejecuta: `git checkout -b rama-de-prueba`
5. Modifica un archivo y ejecuta: `git commit -am "Prueba offline"`

**Reflexión técnica:**
¿Recibiste algún error? No. Todo funcionó.
Ahora, intenta imaginar hacer eso en un sistema donde el historial vive en un servidor en la nube. **Imposible.** Esta capacidad de operar localmente es lo que permite que herramientas como **Magit** (en Emacs) o las integraciones de VS Code sean tan fluidas: pueden consultar el estado del repo cientos de veces por segundo sin saturar la red.

---

## Resumen Documentado

- **Casi todo es local:** Solo `fetch`, `pull`, `push` y `clone` requieren red.
- **Rendimiento:** Limitado solo por la velocidad de tu disco (I/O) y CPU.
- **Ventaja:** Permite trabajar en aviones, trenes o lugares con mala conexión sin perder ni un ápice de potencia de control de versiones.

[⬅️ Volver al Inicio](README.md)