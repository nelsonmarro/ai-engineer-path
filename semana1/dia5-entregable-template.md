# Día 5 — Entregable: `fastapi-llm-template`

**Meta del día:** una plantilla de servicio con LLM que reutilizarás en todos los proyectos del plan. Junta todo lo de la semana: `uv`, los dos modelos, tipos, Pydantic, FastAPI, tests y Docker.

**Tiempo:** ~2 h (si no terminas, el fin de semana es el colchón) · **Dónde:** repositorio nuevo en GitHub, `fastapi-llm-template`

---

## Qué tiene que hacer

- `GET /health` → estado del servicio y qué proveedor está activo
- `POST /chat` → recibe una lista de mensajes y devuelve la respuesta del modelo con tokens y latencia
- El proveedor (`ollama` o `gemini`) se elige con una **variable de entorno**, sin tocar código
- Los tests pasan **sin red y sin modelos**
- Arranca con Docker

## Estructura sugerida

```
fastapi-llm-template/
├── pyproject.toml
├── uv.lock
├── .env.example          ← variables necesarias, sin valores secretos
├── Dockerfile
├── README.md
├── app/
│   ├── main.py           ← crea la app y define los endpoints
│   ├── config.py         ← configuración leída del entorno
│   ├── schemas.py        ← modelos Pydantic de petición y respuesta
│   └── llm/
│       ├── base.py       ← la interfaz LLMClient y ChatResult
│       ├── ollama.py
│       ├── gemini.py
│       └── factory.py    ← devuelve el cliente según la configuración
└── tests/
    └── test_api.py
```

---

## Paso 1 — La interfaz (20 min)

**Qué hacer:** en `app/llm/base.py` define:
- `Message`: rol (`system`, `user` o `assistant`) y contenido
- `ChatResult`: texto, modelo, tokens de entrada y salida, latencia en ms (es tu `ResultadoLLM` del día 3, ahora en Pydantic)
- `LLMClient`: una interfaz con un único método asíncrono `chat(messages) -> ChatResult`

**Noción:** es la idea más importante del día. El resto de la app depende de `LLMClient`, **nunca** de Ollama o Gemini directamente. Así puedes cambiar de proveedor, añadir Claude u OpenAI más adelante, o meter un cliente falso en los tests, sin tocar los endpoints. Es la misma inversión de dependencias que ya usas en C# con interfaces.

<details><summary>Pista</summary>

En Python se hace con `typing.Protocol` (tipado estructural: cualquier clase con ese método cumple, sin heredar) o con `abc.ABC`. `Protocol` se parece más a las interfaces de Go.
</details>

---

## Paso 2 — Dos implementaciones (30 min)

**Qué hacer:** `OllamaClient` y `GeminiClient`, cada uno con su `chat`. Reutiliza tu código del día 1, ahora async.

**Noción:** cada proveedor tiene su propio formato de mensajes y de métricas. La tarea de estas clases es **traducir** de tu formato común (`Message`, `ChatResult`) al del proveedor, y de vuelta. Fuera de estos dos archivos nadie debería saber que existen.

<details><summary>Pista</summary>

En Gemini, los mensajes `system` no van en la lista de contenidos sino en `system_instruction`, y el rol del asistente se llama `model`. Usa `client.aio...` para la versión async. En Ollama, `httpx.AsyncClient` contra `/api/chat`, igual que el día 1.
</details>

---

## Paso 3 — Configuración y fábrica (15 min)

**Qué hacer:**
- `app/config.py` con una clase `Settings` que lea `LLM_PROVIDER`, `OLLAMA_URL`, `OLLAMA_MODEL`, `GEMINI_MODEL` y `GEMINI_API_KEY` del entorno o de `.env`.
- `app/llm/factory.py` con `get_llm_client()` que devuelva el cliente correcto según `LLM_PROVIDER`.
- `.env.example` con todas las variables y valores de ejemplo, **sin tu clave real**.

**Noción:** configuración por entorno (el factor III de las *12-factor apps*). El mismo contenedor funciona en tu máquina con Ollama y en un servidor con Gemini, cambiando solo variables.

<details><summary>Pista</summary>

`uv add pydantic-settings`. Hereda de `BaseSettings` y usa `SettingsConfigDict(env_file=".env")`. Tipa `llm_provider` como `Literal["ollama", "gemini"]`: si alguien pone otro valor, la app falla al arrancar con un mensaje claro, que es lo que quieres.
</details>

---

## Paso 4 — Endpoints (15 min)

**Qué hacer:** en `app/main.py`, `/health` y `/chat`. `/chat` recibe el cliente con **inyección de dependencias** de FastAPI, no lo crea dentro.

**Noción:** `Depends(get_llm_client)` hace que FastAPI te entregue el cliente en cada petición. La ventaja aparece en el paso 5: en los tests reemplazas esa dependencia por una falsa.

<details><summary>Pista</summary>

Valida la entrada: `messages` no puede venir vacío (`Field(min_length=1)`) y el contenido de cada mensaje tampoco. Si el proveedor falla (timeout, 429), devuelve un **502** con un mensaje claro en lugar de un 500 con stack trace.
</details>

---

## Paso 5 — Tests sin red (20 min)

**Qué hacer:** `tests/test_api.py` con al menos estos tres tests:
1. `/health` responde 200.
2. `/chat` con un `FakeLLMClient` devuelve el texto y los tokens que el falso inventa.
3. `/chat` con `messages` vacío responde 422.

**Noción:** los tests de tu lógica **no deben llamar a un modelo real**: serían lentos, cuestan dinero y no son deterministas (lo viste el día 2 con la temperatura). Se prueba con un cliente falso. Probar la calidad de las respuestas del modelo es otra cosa: son las **evals**, que harás en el mes 4.

<details><summary>Pista</summary>

`uv add --dev pytest`. Usa `fastapi.testclient.TestClient` y `app.dependency_overrides[get_llm_client] = lambda: FakeLLMClient()`. Limpia el override al terminar cada test (un fixture de pytest con `yield` viene bien).
</details>

**Listo cuando:** `uv run pytest` pasa con el Wi-Fi apagado.

---

## Paso 6 — Docker (20 min)

**Qué hacer:** `Dockerfile` que instale dependencias con `uv` y arranque la app con `uvicorn` en el puerto 8000. Pruébala con los dos proveedores.

**Noción:** copia primero `pyproject.toml` y `uv.lock` e instala dependencias, y **después** copia el código. Así Docker cachea la capa de dependencias y reconstruir tras cambiar código tarda segundos.

<details><summary>Pista</summary>

- La [guía oficial de uv para Docker](https://docs.astral.sh/uv/guides/integration/docker/) trae el patrón exacto, incluido cómo copiar el binario de `uv` a la imagen.
- `uv sync --frozen --no-dev` instala justo lo del lock, sin dependencias de desarrollo.
- Pasa las variables con `--env-file .env`, nunca con `COPY .env`. Añade un `.dockerignore` con `.env` y `.venv`.
- **En Linux**, el contenedor no ve el `localhost` de tu máquina: para llegar a Ollama usa `OLLAMA_URL=http://host.docker.internal:11434` y arranca con `--add-host=host.docker.internal:host-gateway`.
</details>

**Listo cuando:** `docker run` levanta el servicio y `/chat` responde con Gemini y con Ollama.

---

## Criterio de terminado

- [ ] `uv run pytest` en verde, sin red
- [ ] `/chat` funciona con `LLM_PROVIDER=ollama` y con `LLM_PROVIDER=gemini`
- [ ] La imagen Docker arranca y responde
- [ ] Ninguna clave en el repositorio (revisa `git log -p | grep -i key` antes del push)
- [ ] README de la plantilla con: qué hace, cómo configurarla, cómo correr tests y Docker, y cómo añadir un proveedor nuevo
- [ ] Repositorio público en GitHub
- [ ] Marcado como *Template repository* en la configuración del repo de GitHub, para crear proyectos nuevos desde él

## Extras (si te sobra tiempo)
- `POST /chat/stream` que devuelva la respuesta token a token con *Server-Sent Events*
- Un test que compruebe que un error del proveedor se convierte en 502
- Logging de tokens y latencia de cada petición (te servirá en el mes 4)

## Para el diario
- ¿Qué tuviste que traducir entre Ollama y Gemini? ¿Qué te dice eso sobre añadir un tercer proveedor?
- ¿Qué parte de la semana te costó más y qué repasarías?
