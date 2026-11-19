# Día 2 — Cómo funciona un LLM

**Meta del día:** la insignia de Google y tres experimentos que te muestren con datos qué es un token, qué hace la temperatura y por qué los modelos alucinan.

**Tiempo:** ~2 h

---

## Ejercicio 2.0 — Curso oficial (50 min)

**Qué hacer:** [Google Machine Learning Crash Course → Large language models](https://developers.google.com/machine-learning/crash-course). Lee el módulo completo y aprueba el quiz final.

**Noción:** fíjate en tres ideas, que son las que vas a comprobar en los ejercicios:
1. Un LLM no ve palabras, ve **tokens**.
2. Lo que hace es **predecir el siguiente token** a partir de una distribución de probabilidad.
3. El **transformer** usa *atención* para decidir qué partes del texto anterior importan para esa predicción.

**Listo cuando:** la insignia aparece en tu perfil de Google Developers. Haz captura y guárdala en `../certificados/`.

---

## Ejercicio 2.1 — ¿Cuánto cuesta cada idioma? (25 min)

**Qué hacer:** `tokens.py`. Para cada uno de estos textos, cuenta los tokens con Gemini y con Ollama:

1. Un párrafo en español (~50 palabras)
2. El mismo párrafo traducido al inglés
3. 10 líneas de código Python
4. 10 líneas de código Go o C#
5. Una frase con emojis
6. Un número largo: `3.14159265358979323846`

Imprime una tabla con palabras, caracteres y tokens de cada uno, y la proporción **tokens / palabra**.

**Noción:** los tokenizers se entrenan con datos donde predomina el inglés, así que el inglés suele salir más "barato". Como las APIs cobran por token y la ventana de contexto se mide en tokens, esto afecta tu costo y lo que cabe en un prompt.

<details><summary>Pista</summary>

Gemini tiene un método para **contar tokens sin generar**: búscalo en `client.models` (algo como `count_tokens`). En Ollama no hay uno directo: manda el texto pidiendo una respuesta mínima y mira `prompt_eval_count`. Ese número incluye un poco de formato extra (la plantilla de chat), así que compara entre textos, no en valor absoluto.
</details>

**Listo cuando:** tienes la tabla y puedes decir cuántos tokens de más cuesta el español frente al inglés en cada modelo.

---

## Ejercicio 2.2 — La temperatura en números (25 min)

**Qué hacer:** `temperatura.py`. Con Ollama, completa la frase *"Describe en una sola palabra el color del cielo al atardecer:"* **10 veces** para cada temperatura: `0`, `0.7` y `1.5`. Para cada una, imprime:
- cuántas respuestas **distintas** salieron
- la respuesta más frecuente y cuántas veces apareció

**Noción:** el modelo calcula una probabilidad para cada token posible. La temperatura **aplana o afila** esa distribución antes de elegir: en 0 elige casi siempre el más probable; con temperatura alta da más chances a los improbables. Por eso el mismo prompt no da siempre la misma respuesta, y por eso en producción usarás temperatura baja para extraer datos y más alta para tareas creativas.

<details><summary>Pista</summary>

La temperatura va en `"options": {"temperature": ...}` del cuerpo de `/api/chat`. Para contar, `collections.Counter` sobre las respuestas normalizadas (`.strip().lower()`, sin punto final).
</details>

**Listo cuando:** la tabla muestra que la variedad crece con la temperatura. Si con 0 todavía ves variación, anótalo: es una pregunta para el diario.

---

## Ejercicio 2.3 — Provocar una alucinación (20 min)

**Qué hacer:** `alucinacion.py`. Haz a los dos modelos 3 preguntas que **no pueden** saber, por ejemplo:
- un premio inventado ("¿Quién ganó el Premio Ortega de Ingeniería de Software en 2019?")
- un paper que no existe
- un dato de tu empresa

Corre cada pregunta **dos veces**: sin instrucciones y con un *system prompt* que diga que, si no tiene la información, responda exactamente `NO_SE`. Imprime cuántas veces inventó en cada modo.

**Noción:** el modelo no "busca" la respuesta, **genera texto plausible**. Si la pregunta suena a algo que tendría respuesta, produce una que suena bien. Una instrucción explícita reduce el problema pero no lo elimina. Darle el contexto correcto (lo que hará el RAG en el mes 2) lo reduce mucho más.

<details><summary>Pista</summary>

En Ollama, el system prompt es un mensaje más con `role: "system"` al inicio de `messages`. En Gemini va en la configuración (`system_instruction`). Pedir una palabra clave exacta como `NO_SE` hace que puedas contarla con código en vez de leer cada respuesta.
</details>

**Listo cuando:** tienes una tabla con inventó / dijo NO_SE por modelo y modo.

---

## Para el diario
Escribe con tus palabras un párrafo corto para cada concepto, como si se lo explicaras a un compañero fullstack:
- token
- temperatura
- por qué un LLM alucina
- qué hace la atención *(usa lo que viste en el curso)*
