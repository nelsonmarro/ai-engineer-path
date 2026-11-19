# Día 4 — Pydantic y FastAPI

**Meta del día:** validar datos de verdad con Pydantic y exponer esa validación en tu primera API. Es la base de todo lo que harás con salida estructurada desde la semana 2.

**Tiempo:** ~2 h · **Lecturas:** [Pydantic — Models](https://docs.pydantic.dev/latest/concepts/models/) (20 min) y [tutorial de FastAPI](https://fastapi.tiangolo.com/tutorial/) hasta *Request Body* (25 min)

Añade a `playground`: `uv add pydantic fastapi "uvicorn[standard]"`.

---

## Ejercicio 4.1 — Validar el log con Pydantic (35 min)

**Qué hacer:** `validar.py` vuelve a leer `../datos/respuestas.jsonl`, pero ahora con un modelo Pydantic `RegistroLLM` que exija:
- `modelo`: solo `"qwen3:4b"` o `"gemini-flash"`
- `tarea`: solo `resumen`, `extraccion` o `codigo`
- tokens de entrada y salida: enteros **≥ 0**
- `latencia_ms`: entero **> 0**
- `exito`: booleano
- `fecha`: fecha y hora válida

Imprime un reporte: cuántos registros son válidos, y para cada inválido, su `id` y **qué campo falló y por qué**.

**Noción:** Pydantic no solo comprueba tipos: también **convierte** cuando tiene sentido (el texto `"125"` pasa a `125` en modo normal) y **rechaza** con un mensaje preciso cuando no. Esos mensajes son los que en la semana 2 le devolverás al LLM para que corrija su propia salida.

<details><summary>Pista</summary>

`Literal[...]` para valores fijos, `Field(ge=0)` y `Field(gt=0)` para los rangos, `datetime` para la fecha. Valida con `RegistroLLM.model_validate_json(linea)` y atrapa `ValidationError`; su método `.errors()` te da una lista con `loc` (el campo) y `msg` (el motivo).

Después prueba con `model_config = ConfigDict(strict=True)`: ¿qué registros pasan de válidos a inválidos? Anota la diferencia.
</details>

**Listo cuando:** el reporte encuentra los registros malos (en modo normal son 6, más la línea que no es JSON) y explica cada error. En modo estricto debe aparecer 1 más.

---

## Ejercicio 4.2 — El esquema que verá el modelo (10 min)

**Qué hacer:** imprime el **JSON Schema** de `RegistroLLM` y guárdalo en `esquema.json`. Añade a cada campo una `description` en español y vuelve a generarlo.

**Noción:** este esquema es exactamente lo que se le pasa a un LLM para pedirle **salida estructurada**. Las `description` no son documentación decorativa: el modelo las lee y le ayudan a rellenar bien cada campo.

<details><summary>Pista</summary>

`RegistroLLM.model_json_schema()` y `json.dumps(..., indent=2, ensure_ascii=False)`. La descripción va en `Field(description="...")`.
</details>

**Listo cuando:** `esquema.json` tiene las descripciones en español.

---

## Ejercicio 4.3 — Primera API (25 min)

**Qué hacer:** `api.py` con FastAPI:
- `GET /health` → `{"status": "ok"}`
- `POST /eco` → recibe un `RegistroLLM` y lo devuelve tal cual

Arráncala y prueba los dos endpoints desde `/docs`. Manda a `/eco` un registro con tokens negativos y mira la respuesta.

**Noción:** FastAPI usa tus modelos Pydantic para tres cosas a la vez: **validar** la entrada (devuelve 422 si no cumple), **documentar** la API (lo que ves en `/docs`) y **serializar** la salida. Escribes el modelo una vez y sirve para todo.

<details><summary>Pista</summary>

`uv run fastapi dev api.py` (o `uv run uvicorn api:app --reload`). Si el endpoint recibe un parámetro con tipo `RegistroLLM`, FastAPI ya sabe que viene en el cuerpo de la petición.
</details>

**Listo cuando:** `/eco` devuelve 200 con un registro bueno y 422 con uno malo, y el 422 dice qué campo falló.

---

## Ejercicio 4.4 — `POST /validar` (extra, 20 min)

**Qué hacer:** endpoint que recibe una **lista de diccionarios sin validar** y devuelve:

```json
{"validos": 8, "invalidos": [{"id": 7, "errores": ["tokens_entrada: ..."]}]}
```

Defínelo con modelos Pydantic para la respuesta también.

**Noción:** aquí no puedes dejar que FastAPI valide la entrada directamente, porque un solo registro malo rechazaría toda la petición. Recibes datos "crudos" y validas tú uno por uno: es el patrón para procesar lotes, por ejemplo cuando un modelo extrae 50 facturas y quieres quedarte con las buenas.

<details><summary>Pista</summary>

Tipa la entrada como `list[dict[str, Any]]` y dentro llama a `RegistroLLM.model_validate(d)` en un bucle con `try/except`. Para la respuesta, un modelo `ReporteValidacion` con `validos: int` e `invalidos: list[...]`.
</details>

**Listo cuando:** enviando todo el archivo (convertido a lista) desde `/docs`, el reporte coincide con el del ejercicio 4.1.

---

## Para el diario
- ¿Qué cambió al activar el modo estricto? ¿Cuál preferirías para validar lo que devuelve un LLM?
- ¿Qué devuelve FastAPI exactamente cuando la validación falla? ¿Te sirve para mostrarle el error a un usuario?
