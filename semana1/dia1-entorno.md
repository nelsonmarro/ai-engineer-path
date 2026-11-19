# Día 1 — Entorno

**Meta del día:** dos modelos respondiéndote desde Python, uno local y uno en la nube, y una tabla que los compara.

**Tiempo:** ~2 h · **Lectura:** [uv — Getting started](https://docs.astral.sh/uv/getting-started/) (15 min)

---

## Ejercicio 1.1 — Proyecto con uv (15 min)

**Qué hacer:** dentro de `semana1/`, crea un proyecto llamado `playground` con `uv`. Añade las dependencias `httpx`, `python-dotenv` y `google-genai`.

**Noción:** `uv` reemplaza a `pip` + `venv` + `pip-tools` en una sola herramienta. Crea el `.venv`, escribe las dependencias en `pyproject.toml` y fija las versiones exactas en `uv.lock`. Nunca activas el entorno a mano: ejecutas todo con `uv run`.

<details><summary>Pista</summary>

`uv init <nombre>`, luego `cd` y `uv add <paquetes>`. Mira qué archivos aparecieron y abre `pyproject.toml`.
</details>

**Listo cuando:** `uv run python -c "import httpx, google.genai; print('ok')"` imprime `ok`.

---

## Ejercicio 1.2 — Ollama por terminal (20 min)

**Qué hacer:**
1. Instala Ollama y descarga un modelo pequeño (3–4B parámetros, p. ej. `qwen3:4b` o `llama3.2:3b`).
2. Chatea con él en la terminal.
3. Llama a la **API HTTP** de Ollama con `curl`, sin streaming.

**Noción:** Ollama es un **servidor** que corre en `localhost:11434`. La terminal (`ollama run`) es solo un cliente más. Tu código hablará con ese servidor por HTTP, igual que con cualquier API.

<details><summary>Pista</summary>

El endpoint es `POST /api/chat`. El cuerpo lleva `model`, `messages` (lista de `{role, content}`) y `"stream": false`. Busca el ejemplo en la [documentación de la API de Ollama](https://github.com/ollama/ollama/blob/main/docs/api.md).
</details>

**Listo cuando:** `curl` te devuelve un JSON y encuentras dentro el texto de la respuesta.

---

## Ejercicio 1.3 — `hola_ollama.py` (25 min)

**Qué hacer:** script que manda una pregunta a Ollama con `httpx` e imprime:
- el texto de la respuesta
- tokens de entrada y tokens de salida
- tiempo total en milisegundos

**Noción:** la respuesta de Ollama no trae solo texto: trae **métricas**. Medir tokens y latencia en cada llamada es un hábito de AI engineer, porque es lo que luego se convierte en costo y experiencia de usuario.

<details><summary>Pista</summary>

En el JSON busca `prompt_eval_count`, `eval_count` y `total_duration`. Cuidado: `total_duration` viene en **nanosegundos**. Pon un `timeout` alto en `httpx` (la primera llamada carga el modelo en memoria y tarda).
</details>

**Listo cuando:** el script imprime las 4 cosas sin errores.

---

## Ejercicio 1.4 — `hola_gemini.py` (25 min)

**Qué hacer:**
1. Crea una clave en [Google AI Studio](https://ai.google.dev).
2. Guárdala en `playground/.env` como `GEMINI_API_KEY`. Confirma que `.env` **no** aparece en `git status`.
3. Script que lista los modelos disponibles, elige uno de la familia *Flash* y le hace la misma pregunta que a Ollama, imprimiendo lo mismo que en 1.3.

**Noción:** la clave nunca va en el código ni en git. Se lee del entorno. El SDK oficial (`google-genai`) la toma sola de la variable `GEMINI_API_KEY`. Los nombres de modelos cambian a menudo: lístalos desde el código en vez de copiarlos de un tutorial viejo.

<details><summary>Pista</summary>

`from google import genai`, `client = genai.Client()`. Luego `client.models.list()` y `client.models.generate_content(model=..., contents=...)`. Los tokens están en `response.usage_metadata`. Para la latencia, mide tú con `time.perf_counter()`. Carga el `.env` con `load_dotenv()` antes de crear el cliente.
</details>

**Listo cuando:** el script imprime respuesta, tokens y latencia, y tu clave no está en ningún archivo versionado.

---

## Ejercicio 1.5 — `comparar.py` (30 min)

**Qué hacer:** script que manda **las mismas 3 preguntas** a los dos modelos e imprime una tabla:

```
pregunta | modelo | tokens_in | tokens_out | latencia_ms | primeras 60 letras
```

Usa una pregunta factual, una de razonamiento y una que pida código.

**Noción:** es tu primera comparación de modelos. Aquí empieza la idea que más usarás en el plan: **no se elige un modelo por opinión, se elige midiendo** con tus propias tareas.

<details><summary>Pista</summary>

Reorganiza 1.3 y 1.4 en dos funciones con la **misma firma**, p. ej. `preguntar_ollama(texto) -> dict` y `preguntar_gemini(texto) -> dict`, que devuelvan las mismas claves. Así la tabla no sabe qué modelo hay detrás. Esa idea se convierte en la interfaz `LLMClient` del día 5.
</details>

**Listo cuando:** la tabla sale con 6 filas.

---

## Para el diario
- ¿Cuál fue más rápido y por cuánto? ¿Te sorprendió?
- ¿Cuentan igual los tokens los dos modelos para la misma pregunta? ¿Por qué crees que no?
- ¿Qué te costó más del entorno?
