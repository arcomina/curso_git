# 2. BitKeeper: La crisis que dio origen a Git.

### El Escenario: El idilio con BitKeeper (2002-2005)

Antes de 2002, el Kernel de Linux se gestionaba de forma "caótica". Linus Torvalds recibía archivos `.tar.gz` y parches por email. Tras años de resistencia a usar un sistema de control de versiones (odiaba CVS y SVN), Linus aceptó usar **BitKeeper**, un software comercial propiedad de la empresa **BitMover**.

BitKeeper era revolucionario porque era un **DVCS (Distributed Version Control System)**: permitía que cada desarrollador tuviera una copia local del historial, algo que Git heredaría después. Durante tres años, el desarrollo de Linux alcanzó una velocidad nunca antes vista.

### La Crisis: El "TridgeGate" (Abril 2005)

El acuerdo era que la comunidad de Linux podía usar BitKeeper gratis, siempre que no intentaran crear una herramienta competidora o hacer ingeniería inversa.

Sin embargo, **Andrew Tridgell** (el creador de Samba) comenzó a escribir un cliente de código abierto que se comunicaba con los servidores de BitKeeper mediante ingeniería inversa de sus protocolos. Larry McVoy, el CEO de BitMover, consideró esto una violación del acuerdo y tomó una decisión drástica: **revocar la licencia gratuita para todos los desarrolladores del Kernel de Linux.**

### El Ultimátum Técnico

De la noche a la mañana, el proyecto de software libre más importante del mundo se quedó sin su herramienta principal. Linus tenía tres opciones:

1. Volver al envío manual de parches (retroceso de 10 años).
2. Usar un sistema centralizado como SVN (que Linux consideraba basura técnica).
3. **Crear algo nuevo desde cero.**

---

## Documentación Detallada: Las "Leyes" de Linus para el nuevo sistema

Linus se encerró una semana y estableció que el sucesor de BitKeeper no solo debía ser igual, sino superior en tres pilares que hoy definen a Git:

1. **Rendimiento Extremo:** En BitKeeper, algunas operaciones tardaban segundos. Linus quería que en Git fueran instantáneas (milisegundos).
2. **Resistencia a la Corrupción:** Git debía ser paranoico. Si un bit del disco fallaba, el sistema debía detectarlo inmediatamente mediante hashes (SHA-1).
3. **Flujos de trabajo no lineales:** El sistema debía permitir miles de ramas (branches) paralelas y fusiones (merges) masivas sin errores.

---

## Ejemplo para la Comprensión: El problema de la confianza

Imagina que estás en una oficina con 1,000 programadores.

- **En el modelo antiguo (CVS/SVN):** Si el servidor central se rompe, nadie puede trabajar. Si alguien sube código corrupto, todos se infectan.
- **En el modelo de BitKeeper/Git:** Si el servidor central explota, no importa. Tú tienes todo el historial en tu laptop. Puedes seguir trabajando y, más tarde, sincronizarte con tus compañeros.

La crisis de BitKeeper le enseñó a Linus que **no se puede confiar en un software propietario para un proyecto libre**. Git nació para asegurar que nadie pudiera volver a "quitarles la pelota" en mitad del partido.

---

## Ejercicio Práctico de Reflexión Técnica

**Situación:** Trabajas en una empresa que usa una herramienta de diseño "X". Un día, la empresa "X" decide que tu país ya no tiene acceso a sus servidores.

**Analiza según lo aprendido:**

1. ¿Por qué un sistema **distribuido** (como Git) te permitiría seguir trabajando a pesar del bloqueo?
2. ¿Qué ventaja tiene que el historial de tu código esté basado en **hashes criptográficos** (SHA-1) si de repente tienes que mover todo tu código a un servidor nuevo y desconocido?

**Respuesta guía:**

1. Al ser distribuido, tú ya tienes el 100% de los datos en tu máquina. El servidor es solo un "punto de encuentro", no el dueño de la información.
2. Los hashes garantizan que, al mover el código, nadie haya inyectado código malicioso en la transición. Si el hash coincide, el código es idéntico al original.

---

### Resumen Documentado

- **BitKeeper:** El precursor distribuido pero propietario.
- **La crisis:** La pérdida de licencia por conflicto de intereses.
- **La consecuencia:** El nacimiento de Git como una herramienta que garantiza la **soberanía tecnológica** del desarrollador.
- [⬅️ Volver al Inicio](README.md)