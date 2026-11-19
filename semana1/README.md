# Semana 1 — Bases y entorno

**Objetivo:** al final de la semana tienes el entorno listo, entiendes qué pasa dentro de un LLM y tienes una plantilla FastAPI que habla con dos modelos (Ollama y Gemini) y que reutilizarás todo el plan.

**Credenciales:** insignia *MLCC: Large language models* (Google) + certificado *Python* (Kaggle).

## Cómo usar estas guías

Cada día tiene ejercicios con tres partes:

- **Qué hacer:** el enunciado.
- **Noción:** la idea que necesitas para resolverlo. No es la solución.
- **Pista:** plegada. Ábrela solo si llevas más de 15 minutos atascado.

Cada ejercicio termina con un **"Listo cuando"**: si se cumple, pasas al siguiente. Si no terminas un ejercicio marcado *(extra)*, no pasa nada.

## Dónde va el código

```
semana1/
├── README.md                ← esta página
├── dia1-entorno.md
├── dia2-como-funciona-un-llm.md
├── dia3-python-para-ia.md
├── dia4-pydantic-y-fastapi.md
├── dia5-entregable-template.md
├── datos/
│   └── respuestas.jsonl     ← datos de práctica para los días 3 y 4
└── playground/              ← lo creas tú el día 1: aquí van los scripts de los días 1–4
```

El entregable del día 5 (`fastapi-llm-template`) va en **su propio repositorio** de GitHub, porque lo vas a clonar en cada proyecto.

## Plan de la semana

| Día | Tema | Ejercicios | Credencial |
|---|---|---|---|
| 1 | [Entorno](dia1-entorno.md) | uv, Ollama, Gemini, primer script comparativo | — |
| 2 | [Cómo funciona un LLM](dia2-como-funciona-un-llm.md) | Tokens, temperatura, alucinaciones | Insignia Google |
| 3 | [Python para IA](dia3-python-para-ia.md) | JSONL, dataclasses, async | Certificado Kaggle |
| 4 | [Pydantic y FastAPI](dia4-pydantic-y-fastapi.md) | Validar datos, JSON Schema, primera API | — |
| 5 | [Entregable](dia5-entregable-template.md) | `fastapi-llm-template` con tests y Docker | — |

## Checklist

- [ ] Día 1 — entorno funcionando y `comparar.py` imprime la tabla
- [ ] Día 2 — insignia de Google + tabla de tokens en el diario
- [ ] Día 3 — certificado de Kaggle + `analizar.py` + `paralelo.py`
- [ ] Día 4 — `validar.py` + API con `/eco` y `/validar`
- [ ] Día 5 — `fastapi-llm-template` en GitHub, tests en verde, imagen Docker que arranca
- [ ] Certificados guardados en `../certificados/`
- [ ] *(opcional, fin de semana)* [Kaggle: Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning)

## Al cerrar cada día

1. Commit con lo que hiciste (aunque esté a medias).
2. 3–5 líneas en `../diario.md`. Cada guía trae preguntas para el diario: respóndelas con tus palabras, sin copiar.
