# 12.Colisiones de Hash: ¿Es posible que dos archivos tengan el mismo ID?

Esta es la pregunta que todo ingeniero se hace tarde o temprano: *¿Qué pasa si dos archivos distintos generan exactamente el mismo código de 40 caracteres?* A este fenómeno se le llama **Colisión**.

Aquí tienes la documentación detallada sobre las probabilidades, la realidad técnica y cómo Git maneja este riesgo.

## 1. ¿Qué es una colisión de Hash?

Una colisión ocurre cuando dos entradas de datos diferentes (por ejemplo, una foto de un gato y un archivo de configuración de un servidor) producen exactamente el mismo hash SHA-1. Dado que el SHA-1 tiene un número **finito** de combinaciones (2^160),

y el número de archivos posibles en el universo es **infinito**, matemáticamente las colisiones *tienen* que existir.

### 2. La probabilidad matemática (El mundo de lo imposible)

Para que te hagas una idea de la escala, el número de hashes posibles es aproximadamente:
**1,461,501,637,330,902,918,203,684,832,716,283,019,655,932,542,976**

Linus Torvalds explicó una vez la probabilidad de una colisión accidental de esta forma:

> "Es mucho más probable que todos los miembros de tu equipo de programación sean alcanzados por un rayo **al mismo tiempo** en que ganan la lotería, a que Git confunda dos archivos por accidente".
> 

### 3. El ataque "SHAttered" (2017)

Durante años, la colisión fue solo teórica. Sin embargo, en 2017, investigadores de Google anunciaron **SHAttered**: lograron crear dos archivos PDF distintos con el mismo hash SHA-1.

- **El costo:** Requirió una potencia de cómputo inmensa (equivalente a 110 GPUs trabajando un año entero).
- **La respuesta de Git:** Inmediatamente se implementaron parches para detectar "ataques de colisión". Hoy en día, Git utiliza un "detector de colisiones" que analiza si un archivo ha sido diseñado maliciosamente para explotar esta vulnerabilidad.

---

## Documentación Técnica: ¿Qué pasaría en Git si ocurriera?

Si por un azar del destino (o un ataque) logras meter dos archivos con el mismo Hash en un repositorio:

1. **El primero gana:** Git guarda el primer objeto que recibe.
2. **El segundo es ignorado:** Cuando intentes añadir el segundo archivo, Git calculará el Hash, verá que ya existe un objeto con ese nombre en `.git/objects` y asumirá que es el mismo archivo.
3. **Corrupción lógica:** Tu repositorio tendría el nombre del "Archivo B" pero, al abrirlo, verías el contenido del "Archivo A".

---

## Ejemplo para la Comprensión: Los Granos de Arena

Imagina que cada archivo posible en el universo es un grano de arena.

- El número de granos de arena en la Tierra es minúsculo comparado con el número de combinaciones del SHA-1.
- Tendrías que tener un desierto que llenara todo el sistema solar para empezar a tener una probabilidad real de elegir dos granos idénticos por accidente.

En la práctica profesional, **puedes confiar ciegamente en que nunca verás una colisión accidental** en toda tu carrera.

---

## Ejercicio de Reflexión Técnica

**Situación:**
Tu jefe lee una noticia sobre que "SHA-1 ha sido roto" y te pide que dejes de usar Git porque teme que el código de la empresa se mezcle.

**Preguntas para responderle:**

1. ¿Es lo mismo una colisión **accidental** que una **provocada** en un laboratorio?
    - *Respuesta:* No. Las accidentales siguen siendo imposibles. Las provocadas requieren miles de dólares en servidores y archivos modificados quirúrgicamente.
2. ¿Cómo protege Git el repositorio contra estos ataques ahora?
    - *Respuesta:* Git ahora incluye código que detecta las estructuras matemáticas sospechosas típicas de un ataque de colisión (Hardened SHA-1).

---

## El Futuro: SHA-256

Debido a que el SHA-1 ya no se considera "seguro" contra ataques de estados-nación o supercomputadoras, las versiones más recientes de Git ya permiten usar **SHA-256** (que genera hashes mucho más largos y seguros). Sin embargo, la migración de la industria es lenta porque el SHA-1 sigue siendo perfectamente válido para la integridad de datos del día a día.

---

## Resumen Documentado

- **Probabilidad accidental:** Virtualmente cero.
- **Vulnerabilidad:** Existe solo mediante ataques computacionales masivos.
- **Seguridad actual:** Git utiliza versiones "reforzadas" del algoritmo para detectar manipulaciones.

[⬅️ Volver al Inicio](../README.md)