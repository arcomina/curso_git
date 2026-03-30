# 11. El Algoritmo SHA-1: Cómo se generan los hashes de 40 caracteres.

Si Git fuera un cuerpo humano, el **SHA-1** sería su ADN. Es el mecanismo matemático que permite que todo lo que hemos hablado (integridad, inmutabilidad y snapshots) funcione en la práctica.

Aquí tienes la documentación detallada sobre cómo Git utiliza la criptografía para identificar archivos.

### 1. ¿Qué es un Hash SHA-1?

**SHA-1** (Secure Hash Algorithm 1) es una función criptográfica de resumen. Su trabajo es tomar una entrada de cualquier tamaño (un archivo de texto, una imagen de 4GB o un simple "Hola") y transformarla en una cadena fija de **160 bits**, que Git representa como **40 caracteres hexadecimales** (números del 0-9 y letras de la a-f).

### 2. Las 4 Propiedades Mágicas del SHA-1 en Git

Para que Git confíe su vida en este algoritmo, se cumplen estas reglas:

- **Determinismo:** Si pasas el mismo archivo por el algoritmo un millón de veces, el hash siempre será exactamente el mismo.
- **Efecto Avalancha:** Si cambias un solo bit (un punto por una coma) en un archivo gigante, el hash resultante será completamente diferente. No se parece en nada al anterior.
- **Irreversibilidad:** No puedes tomar el hash `24b9da...` y "reconstruir" el archivo original. Es un camino de una sola vía.
- **Resistencia a colisiones:** Es estadísticamente casi imposible que dos contenidos diferentes produzcan el mismo hash.

### 3. ¿Cómo genera Git el Hash? (Documentación Técnica)

Git no solo hace el hash de tu archivo; le añade una "cabecera" para saber qué está guardando. El cálculo real que hace Git es:

```python
sha1("tipo" + " " + "tamaño" + "\0" + "contenido")
```

1. **Tipo:** Puede ser `blob`, `tree`, `commit` o `tag`.
2. **Tamaño:** El peso del archivo en bytes.
3. **\0:** Un byte nulo (separador).
4. **Contenido:** Los datos reales del archivo.

---

## Ejemplo para la Comprensión: La Huella Dactilar Digital

Imagina que tienes dos gemelos idénticos. A simple vista, parecen iguales (mismo nombre de archivo, mismo tamaño).

- Sin Git, podrías confundirlos.
- Con SHA-1, Git les pide su "huella dactilar". Aunque sean casi iguales, sus huellas son únicas.

Si un gemelo se corta un dedo (cambias un carácter), su huella cambia por completo. Git ya no lo reconoce como el mismo "objeto" y crea uno nuevo.

---

## Ejercicio Práctico: Genera un Hash manualmente

Si estás en una terminal (Linux, Mac o Git Bash en Windows), puedes ver a Git en acción calculando el hash de la palabra "test":

Bash

```python
echo "test" | git hash-object --stdin
```

**Resultado:** `4b825dc642cb6eb9a060e54bf8d69288fbee4904`

**Reto de comprensión:**

1. Crea un archivo llamado `hola.txt` con el texto "Hola mundo".
2. Calcula su hash.
3. Añade un espacio al final del texto y vuelve a calcular el hash.
4. ¿El nuevo hash empieza por los mismos caracteres o cambió radicalmente?

**Respuesta Técnica:** Cambiará radicalmente. Esto es lo que permite que Git detecte cambios minúsculos en proyectos de millones de líneas de código en milisegundos.

---

## El Rol del Hash en la Carpeta `.git`

Cuando Git genera el hash, por ejemplo: `d670460b4b4aece5915caf5c68d12f560a9fe3e4`, guarda el archivo en:

```python
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4
```

- Usa los **2 primeros caracteres** como nombre de carpeta.
- Usa los **38 restantes** como nombre de archivo.
- Esto lo hace para no saturar una sola carpeta con miles de archivos, mejorando la velocidad de lectura del sistema operativo.

---

## Resumen Documentado

- **Longitud:** 40 caracteres hexadecimales.
- **Función:** Identificador único universal del contenido.
- **Seguridad:** Garantiza que el historial no ha sido alterado.