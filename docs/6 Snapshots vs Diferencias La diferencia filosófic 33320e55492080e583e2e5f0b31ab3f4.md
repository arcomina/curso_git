# 6. Snapshots vs. Diferencias: La diferencia filosófica entre Git y otras herramientas.

Entender este concepto es lo que separa a un usuario que "copia comandos" de un **arquitecto de Git**. Aquí es donde la mayoría de la gente se confunde porque intentan entender Git usando la lógica de los sistemas antiguos.

### 1. El modelo antiguo: Almacenamiento basado en Deltas (Diferencias)

La mayoría de los sistemas de control de versiones anteriores (CVS, Subversion, Perforce) ven la información como una lista de archivos y los **cambios** realizados en cada uno a lo largo del tiempo.

- **¿Cómo funciona?** Guardan el archivo original (Versión 1) y luego guardan solo los "deltas" (lo que se sumó o restó).
- **El problema técnico:** Para que el sistema te muestre cómo se ve el archivo en la Versión 5, tiene que ir a la Versión 1 y aplicar matemáticamente el cambio 2, luego el 3, luego el 4 y finalmente el 5. Esto consume tiempo y procesamiento, especialmente en archivos grandes.

---

### 2. El modelo de Git: Almacenamiento basado en Snapshots (Instantáneas)

Git no piensa en "cambios de líneas". Git piensa en **"Cómo se ve todo mi proyecto en este momento"**.

Cada vez que haces un commit, Git toma una **foto instantánea (Snapshot)** de todos tus archivos en ese estado exacto y guarda una referencia a esa foto.

- **¿Cómo funciona?** * Si un archivo **ha cambiado**, Git guarda una copia completa del nuevo archivo (comprimida).
    - Si un archivo **NO ha cambiado**, Git no lo duplica; simplemente crea un enlace (puntero) hacia el archivo anterior que ya tiene guardado.

---

### 3. Documentación Técnica: Las ventajas del Snapshot

1. **Velocidad de Recuperación:** Si le pides a Git la Versión 5, no tiene que calcular nada. Simplemente busca el snapshot 5 y te entrega los archivos. Es casi instantáneo.
2. **Facilidad para Ramas (Branching):** Como Git tiene snapshots, cambiar de una rama a otra es solo cambiar el puntero de un snapshot a otro.
3. **Integridad:** Es mucho más difícil que se corrompa la historia. En el sistema de deltas, si el archivo de la "Versión 2" se daña, todas las versiones siguientes (3, 4, 5...) quedan arruinadas porque dependen de ese eslabón. En Git, cada snapshot es una entidad más independiente.

---

## Ejemplo para la Comprensión: El Álbum de Fotos vs. La Lista de Compras

- **Deltas (Otros sistemas):** Es como una lista de compras que editas cada día. "Día 1: Compré Leche. Día 2: Taché leche y puse Pan. Día 3: Añadí Huevos". Para saber qué tienes el Día 3, tienes que leer toda la lista desde el inicio y hacer la cuenta mental.
- **Snapshots (Git):** Es como si cada día le tomaras una **fotografía a tu mesa de la cocina**. Si el Día 3 quieres saber qué tenías, solo miras la foto del Día 3. No te importa qué compraste el Día 1; la foto te da la realidad actual de un vistazo.

---

## Ejercicio Práctico de Comprensión Técnica

**Situación:**
Tienes un proyecto con 100 fotos de alta resolución (10MB cada una). Solo editas una foto para corregir el color y haces un commit.

1. **En un sistema de Deltas:** El sistema buscará los píxeles que cambiaron y guardará un archivo pequeño con esa diferencia. El repositorio pesará 1000MB + el pequeño cambio.
2. **En Git:** Git guardará las 99 fotos originales (usando punteros, no ocupan espacio extra) y guardará la nueva foto editada completa (10MB). El repositorio pesará 1000MB + 10MB.

**Pregunta de reflexión:** Si Git guarda "copias completas", ¿por qué los repositorios de Git suelen ser **más pequeños** en disco que los de SVN?

**Respuesta Técnica:** Porque Git utiliza un algoritmo de compresión muy superior (zlib) y un sistema llamado **Packfiles**, que empaqueta los objetos de forma tan eficiente que, aunque filosóficamente son snapshots, físicamente ocupan el mínimo espacio posible en el disco.

---

### Resumen Documentado

- **Otros sistemas:** Almacenan cambios (Deltas). Priorizan el ahorro de espacio teórico sobre la velocidad.
- **Git:** Almacena fotos del estado actual (Snapshots). Prioriza la velocidad, la integridad y la facilidad de ramificación.

[⬅️ Volver al Inicio](README.md)