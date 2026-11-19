# Día 3 — Python para IA

**Meta del día:** el certificado de Kaggle y el Python que más vas a usar en sistemas con LLMs: leer logs JSONL, tipar resultados y hacer llamadas en paralelo.

**Tiempo:** ~2 h

---

## Ejercicio 3.0 — Curso oficial (50 min)

**Qué hacer:** [Kaggle Learn: Python](https://www.kaggle.com/learn/python). Ya sabes lo básico: **salta directo a los ejercicios** de cada lección y vuelve al tutorial solo si fallas uno.

**Noción:** presta más atención a las lecciones de *Lists*, *Loops and List Comprehensions* y *Strings and Dictionaries*: es lo que usarás para transformar respuestas de modelos.

**Listo cuando:** el certificado aparece en tu perfil de Kaggle. Guarda el PDF en `../certificados/`.

---

## Ejercicio 3.1 — Analizar un log (25 min)

**Qué hacer:** `analizar.py` lee `../datos/respuestas.jsonl` (un registro de llamadas a modelos, una por línea) y muestra **por modelo**:
- número de llamadas
- latencia media y la más alta
- tokens de salida totales
- % de éxito

Usa solo la librería estándar (sin pandas). Las líneas que no puedas leer, sáltalas pero **cuéntalas** y muestra cuántas fueron.

**Noción:** JSONL (un JSON por línea) es el formato estándar de logs, datasets de evaluación y procesos por lotes en IA. Se lee **línea a línea**, nunca con un solo `json.load` del archivo entero. Los datos reales vienen sucios: el archivo tiene líneas rotas a propósito.

<details><summary>Pista</summary>

`for linea in archivo:` + `json.loads(linea)` dentro de un `try/except json.JSONDecodeError`. Para agrupar, `collections.defaultdict(list)` con el modelo como clave. Algunas líneas sí son JSON pero les falta un campo: usa `.get()` o atrapa `KeyError`. Mañana resolverás esto bien con Pydantic.
</details>

**Listo cuando:** el resumen sale por modelo y te dice cuántas líneas descartó. *(Hay al menos 1 línea que no es JSON y varias con datos raros.)*

---

## Ejercicio 3.2 — Tipar el resultado (20 min)

**Qué hacer:** en `modelos.py`:
1. Una `@dataclass` llamada `ResultadoLLM` con los campos que devolvían tus funciones del día 1 (texto, modelo, tokens de entrada y salida, latencia).
2. Una función `costo_usd(resultado, precio_entrada_por_millon, precio_salida_por_millon) -> float`.
3. Cambia `comparar.py` del día 1 para que use `ResultadoLLM` en vez de diccionarios e imprima una columna con el costo estimado (inventa precios para Gemini, p. ej. 0,30 y 2,50 USD por millón; Ollama cuesta 0).

**Noción:** las APIs cobran por **millón de tokens**, y la salida suele costar varias veces más que la entrada. Pasar de `dict` a tipos con nombre hace que el editor y `mypy` te avisen de errores; es el paso previo a Pydantic.

<details><summary>Pista</summary>

`costo = tokens_in / 1_000_000 * precio_in + tokens_out / 1_000_000 * precio_out`. Añade type hints a todo y corre `uv run --with mypy mypy .` para ver si se queja.
</details>

**Listo cuando:** `comparar.py` imprime una columna de costo y `mypy` no reporta errores.

---

## Ejercicio 3.3 — Llamadas en paralelo (25 min)

**Qué hacer:** `paralelo.py` manda **5 preguntas** a Gemini de dos formas y mide el tiempo total de cada una:
1. Secuencial: una detrás de otra.
2. En paralelo con `asyncio`.

Repite con Ollama. Imprime los 4 tiempos.

**Noción:** una llamada a un LLM es casi todo **espera de red**. Con `async`, mientras una espera, las demás avanzan. Es el patrón de cualquier backend con IA (y equivale al `Task.WhenAll` de C# o a goroutines en Go). Con Ollama puede que no ganes nada: un servidor local con una sola GPU/CPU a veces atiende de a una petición. Descubrir eso midiendo es parte del ejercicio.

<details><summary>Pista</summary>

Para Ollama usa `httpx.AsyncClient` y `asyncio.gather(*tareas)`. El SDK de Gemini tiene versión asíncrona en `client.aio.models.generate_content(...)`. Ojo con la capa gratuita de Gemini: si recibes un error **429** es el límite por minuto; espera y reintenta, o baja a 3 preguntas.
</details>

**Listo cuando:** tienes los 4 tiempos y una explicación de por qué Ollama ganó o no ganó en paralelo.

---

## Para el diario
- ¿Qué tipos de errores encontraste en el log? ¿Cuál se te habría colado en producción?
- ¿Cuánto costarían 10.000 llamadas como las de `comparar.py` con los precios que inventaste?
- ¿Qué pasó con Ollama en paralelo y por qué crees que pasó?
