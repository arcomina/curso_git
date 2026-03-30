# 13. Seguridad en Git: Firmas GPG y verificación de autores.

Hasta ahora hemos visto cómo Git asegura que los *datos* no cambien (SHA-1), pero ¿cómo aseguramos que la *persona* que dice haber enviado el código es realmente quien dice ser?

En Git, es alarmantemente fácil suplantar a alguien. Cualquiera puede configurar su nombre como "Linus Torvalds" en su terminal. Aquí es donde entra la **Criptografía de Clave Pública**.

### 1. El problema de la suplantación (Impersonation)

Por defecto, Git confía en lo que dice tu configuración local:

Bash

```python
git config --global user.name "Elon Musk"
git config --global user.email "elon@spacex.com"
```

Si haces un commit con esto, en el historial aparecerá que Elon Musk escribió ese código. Git no verifica correos electrónicos por sí solo; es una herramienta basada en la buena fe de los colaboradores.

### 2. La solución: Firmas GPG (GNU Privacy Guard)

Para evitar fraudes en proyectos críticos, se utiliza **GPG**. Es un sistema que permite "sellar" tus commits con una firma digital matemática que nadie puede falsificar.

- **Clave Privada:** Vive solo en tu computadora, protegida por una contraseña. La usas para "firmar" el commit.
- **Clave Pública:** La compartes con el mundo (o la subes a GitHub/GitLab). Se usa para "verificar" que la firma fue hecha con tu clave privada.

---

### 3. Cómo funciona el flujo técnico

Cuando firmas un commit (usando `git commit -S`), Git hace lo siguiente:

1. Toma el hash del commit (SHA-1).
2. Utiliza tu clave privada GPG para cifrar ese hash.
3. Adjunta ese "sello" cifrado al objeto del commit.

Si alguien intenta cambiar el autor o el código, la firma dejará de ser válida porque el hash ya no coincidirá con el sello cifrado.

---

## Ejemplo para la Comprensión: El Sello de Lacre Real

Imagina que eres un Rey enviando órdenes a tus generales:

- **Sin Firma:** Alguien podría escribir una carta diciendo "Soy el Rey, ríndanse" y enviarla. Los generales no saben si es verdad.
- **Con GPG:** El Rey tiene un **anillo único** (Clave Privada). Estampa su sello en cera caliente sobre la carta. Los generales tienen un **molde de yeso** del anillo (Clave Pública). Si el sello encaja perfectamente en el molde, saben que la orden es auténtica. Si el sello está roto o es diferente, la orden se ignora.

---

## Documentación: Estados de Verificación en plataformas (GitHub/GitLab)

Cuando usas firmas GPG, verás etiquetas al lado de tus commits en la web:

| **Etiqueta** | **Significado** |
| --- | --- |
| **Verified** (Verificado) | El commit tiene una firma GPG válida y la clave pública está registrada en la cuenta del usuario. |
| **Unverified** (No verificado) | Hay una firma, pero la clave pública es desconocida o no coincide con el usuario. |
| **Sin etiqueta** | El commit no está firmado (es el estado por defecto). |

---

## Ejercicio Práctico: ¿Cómo empezar a asegurar tu identidad?

Si quieres experimentar con la seguridad (esto no requiere instalar nada ahora, es para tu conocimiento):

1. **Generar una llave:** Se usa el comando `gpg --full-generate-key`.
2. **Obtener el ID de tu llave:** `gpg --list-secret-keys --keyid-format LONG`.
3. **Configurar Git para que firme siempre:**Bash
    
    ```python
    git config --global user.signingkey <TU_ID_DE_LLAVE>
    git config --global commit.gpgsign true
    ```
    

**Pregunta de reflexión:**

Si trabajas en un proyecto de código abierto y recibes una contribución de una "celebridad" del software, pero el commit no tiene la etiqueta de **Verified**, ¿deberías confiar ciegamente en que es él?

*Respuesta:* No. Podría ser cualquier persona con acceso a internet que simplemente cambió su `user.name` en la configuración de Git.

---

## Resumen Documentado

- **Autenticidad:** Asegura que el autor es real.
- **No repudio:** El autor no puede negar haber escrito ese código después de firmarlo.
- **Herramienta estándar:** OpenPGP / GPG.