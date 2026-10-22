# Ruta AI Engineer — 24 semanas

Plan de estudio para pasar de fullstack senior a **AI Engineer**: construir productos confiables sobre modelos de lenguaje (LLMs), sin entrenar modelos desde cero.

- **Dedicación:** 2 h/día, 5 días a la semana (~10 h/semana). Los fines de semana son colchón para ponerte al día, no obligación.
- **Duración:** 24 semanas (~6 meses), en 6 fases.
- **Punto de partida:** 7 años de fullstack, Python básico.
- **Regla de oro:** 30 % estudiar, 70 % construir. Si una semana solo viste videos, no avanzaste.
- **Costo:** todo es gratuito. Las APIs de modelos sí cobran, así que el plan usa modelos locales con **Ollama** y la capa gratuita de la **API de Gemini** por defecto. Al final hay una sección aparte con libros de pago, para cuando decidas invertir (a partir del mes 4).

---

## Cómo usar este plan

1. Marca las casillas `- [ ]` → `- [x]` a medida que avanzas (GitHub las muestra como checkboxes).
2. Crea **un repositorio por proyecto** dentro de esta carpeta o en tu GitHub. Los 4 proyectos son tu portafolio.
3. Lleva un archivo `diario.md`: 3–5 líneas al final de cada sesión con lo que aprendiste y lo que te bloqueó. Te sirve para repasar y luego para escribir posts.
4. Si te atrasas, **recorta teoría, nunca el entregable de la semana**.

### Estructura de una sesión de 2 horas
| Tiempo | Actividad |
|---|---|
| 0:00–0:40 | Estudio (video, curso, lectura) |
| 0:40–1:50 | Construir / programar |
| 1:50–2:00 | Diario + commit |

### Stack de trabajo
- **Python moderno:** `uv` (gestor de paquetes), type hints, `async`, Pydantic, FastAPI, pytest.
- **Modelos:** Ollama (local, gratis) + API de Gemini (capa gratuita). Escribe tu código detrás de una interfaz para poder cambiar a Claude u OpenAI sin reescribir nada.
- **Datos:** PostgreSQL + pgvector (ya conoces SQL, no necesitas una base vectorial aparte).
- **Tus fortalezas:** Go o C# para el servidor MCP del Proyecto 3. Hay pocos ejemplos en esos lenguajes y eso te diferencia.

---

## Mapa general

| Fase | Semanas | Tema | Entregable |
|---|---|---|---|
| 0 | 1–2 | Python para producción | Plantilla de proyecto FastAPI + tests |
| 1 | 3–5 | Cómo funcionan los LLMs por dentro | Notebook con un mini-GPT entrenado |
| 2 | 6–9 | Construir con APIs de modelos | **Proyecto 1:** extractor de datos estructurados |
| 3 | 10–13 | Embeddings y RAG | **Proyecto 2:** RAG sobre documentos reales |
| 4 | 14–16 | Evals (la habilidad que más te diferencia) | Suite de evals para el Proyecto 2 + informe |
| 5 | 17–20 | Agentes, tool use y MCP | **Proyecto 3:** servidor MCP en Go/C# + agente |
| 6 | 21–24 | Producción: costos, seguridad, observabilidad | **Proyecto 4:** capstone desplegado + artículo |

---

## Fase 0 — Python para producción (semanas 1–2)

Ya sabes programar; aquí solo aprendes "el Python de los equipos de IA".

### Semana 1 — Herramientas y tipado
- [ ] [Documentación de uv](https://docs.astral.sh/uv/): crear proyecto, dependencias, entornos
- [ ] [Pydantic — Concepts](https://docs.pydantic.dev/latest/concepts/models/): modelos, validación, serialización (lo usarás para *todas* las salidas de los LLMs)
- [ ] Type hints, `dataclasses`, `async/await` en Python (artículos gratuitos de [Real Python](https://realpython.com/))
- [ ] **Construir:** script que lee un CSV, lo valida con Pydantic y reporta errores

### Semana 2 — APIs y tests
- [ ] [Tutorial oficial de FastAPI](https://fastapi.tiangolo.com/tutorial/) (hasta "Dependencies" y "Testing")
- [ ] pytest básico + fixtures
- [ ] Instalar [Ollama](https://ollama.com) y correr un modelo pequeño (p. ej. Llama o Qwen de 3–8B)
- [ ] Crear una clave gratuita en [Google AI Studio](https://ai.google.dev) (API de Gemini)
- [ ] **Entregable:** plantilla `fastapi-llm-template` con un endpoint `/chat` que llama a Ollama, config por variables de entorno, tests y Dockerfile. La reutilizarás en todos los proyectos.

---

## Fase 1 — Cómo funcionan los LLMs (semanas 3–5)

No vas a entrenar modelos, pero entender tokens, atención y por qué alucinan te hace tomar mejores decisiones de ingeniería.

### Semana 3 — Intuición
- [ ] [3Blue1Brown — Neural Networks](https://www.3blue1brown.com/topics/neural-networks), capítulos 1–4 (redes, gradiente, backprop)
- [ ] 3Blue1Brown, capítulos 5–7 (transformers, atención, cómo guardan "hechos")
- [ ] Karpathy, *Intro to Large Language Models* (1 h) — [canal de YouTube](https://www.youtube.com/@AndrejKarpathy)
- [ ] **Construir:** juega con [Tiktokenizer](https://tiktokenizer.vercel.app): ¿cuántos tokens cuesta el español vs. el inglés? ¿y el código? Anota hallazgos en tu diario.

### Semana 4 — LLMs en profundidad
- [ ] Karpathy, *Deep Dive into LLMs like ChatGPT* (3,5 h; divídelo en 3 sesiones): pre-entrenamiento, post-entrenamiento, RLHF, alucinaciones, modelos de razonamiento
- [ ] [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) (Jay Alammar)
- [ ] **Construir:** notebook que llama al mismo prompt con temperatura 0, 0.7 y 1.5, 10 veces cada uno, y mide la variación. Primer contacto con la naturaleza probabilística.

### Semana 5 — Construir un GPT pequeño
- [ ] [Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html): *micrograd* (opcional si vas corto de tiempo)
- [ ] Zero to Hero: *Let's build GPT: from scratch, in code, spelled out* — programa junto al video
- [ ] Zero to Hero: *Let's build the GPT Tokenizer* (al menos la primera mitad)
- [ ] **Entregable:** notebook con tu mini-GPT entrenado sobre un texto en español (p. ej. una novela de dominio público) + 1 página explicando con tus palabras qué es un token, un embedding y la atención.

> Libros gratuitos de consulta para esta fase (no para leer completos): [Understanding Deep Learning](https://udlbook.github.io/udlbook/) de Simon Prince (PDF gratis, capítulo 12 = transformers) y [Dive into Deep Learning](https://d2l.ai).

---

## Fase 2 — Construir con APIs de modelos (semanas 6–9)

### Semana 6 — Prompt engineering serio
- [ ] [Tutorial interactivo de prompt engineering de Anthropic](https://github.com/anthropics/prompt-eng-interactive-tutorial) (capítulos 1–9; los conceptos sirven para cualquier modelo)
- [ ] Whitepaper *Prompt Engineering* de Google (gratis en Kaggle, búscalo por nombre)
- [ ] **Construir:** 5 prompts para la misma tarea (clasificar tickets de soporte), compáralos en 20 ejemplos escritos por ti. Anota cuál ganó y por qué.

### Semana 7 — Salida estructurada y tool calling
- [ ] [Anthropic Academy](https://anthropic.skilljar.com) → curso **Building with the Claude API** (gratis, con certificado). Puedes seguirlo conceptualmente aunque uses otro proveedor.
- [ ] [Repositorio anthropics/courses](https://github.com/anthropics/courses): notebooks de *Tool use*
- [ ] [OpenAI Cookbook](https://cookbook.openai.com): ejemplos de structured outputs y function calling
- [ ] **Construir:** función que recibe texto libre y devuelve un objeto Pydantic validado; si la validación falla, reintenta enviándole el error al modelo.

### Semanas 8–9 — Proyecto 1
- [ ] [Microsoft — Generative AI for Beginners](https://github.com/microsoft/generative-ai-for-beginners): lecciones de *Building chat apps* y *Securing AI applications* (hay [versión .NET](https://github.com/microsoft/Generative-AI-for-beginners-dotnet) si quieres ver los mismos conceptos en C#)
- [ ] Streaming de respuestas (SSE en FastAPI)

**Proyecto 1 — Extractor de documentos.** Una API que recibe facturas, contratos o CVs (PDF o texto) y devuelve JSON estructurado y validado.
- [ ] Esquema Pydantic + extracción con reintentos
- [ ] Soporte para al menos 2 proveedores de modelo detrás de una interfaz común
- [ ] Registro de tokens usados y tiempo por petición
- [ ] 30 documentos de prueba con su resultado esperado y un script que mide el % de campos correctos
- [ ] README explicando decisiones (por qué ese esquema, qué falla y por qué)

---

## Fase 3 — Embeddings y RAG (semanas 10–13)

### Semana 10 — Embeddings y búsqueda vectorial
- [ ] [Hugging Face LLM Course](https://huggingface.co/learn/llm-course): capítulos 1–2 (transformers, tokenizers, cómo usar modelos de embeddings)
- [ ] [pgvector — README](https://github.com/pgvector/pgvector): índices HNSW vs IVFFlat, distancias
- [ ] **Construir:** genera embeddings de 1.000 frases con un modelo local, guárdalos en Postgres y haz búsqueda semántica. Compara con `ILIKE` y full-text search de Postgres.

### Semana 11 — RAG bien hecho
- [ ] [Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) (Anthropic): chunking, búsqueda híbrida (BM25 + vectores), reranking
- [ ] [DeepLearning.AI](https://www.deeplearning.ai/short-courses/) → *Building and Evaluating Advanced RAG* (verifica que siga gratis al inscribirte)
- [ ] [Patterns for Building LLM-based Systems & Products](https://eugeneyan.com/writing/llm-patterns/) (Eugene Yan) — lectura clave, vuelve a ella varias veces

### Semanas 12–13 — Proyecto 2
**Proyecto 2 — Asistente RAG sobre documentos reales.** Elige un corpus público que conozcas y te importe: normativa o leyes públicas, documentación técnica de un framework, manuales de tu empresa (si te dejan).
- [ ] Ingesta: parseo de PDFs/HTML, chunking con metadatos (fuente, sección, página)
- [ ] Búsqueda híbrida (pgvector + full-text de Postgres) y reranking
- [ ] Respuestas con **citas** a la fuente exacta; si no hay contexto suficiente, el sistema dice "no sé"
- [ ] API FastAPI + interfaz mínima (React, que ya dominas)
- [ ] Versión v1 congelada en un tag de git (la mejorarás con evals en la fase 4)

---

## Fase 4 — Evals (semanas 14–16)

Esta es la fase que más te separa de la gente que solo hace demos. Un AI engineer sabe **demostrar con datos** que su sistema funciona y que un cambio lo mejoró.

### Semana 14 — Análisis de errores
- [ ] [Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/) (Hamel Husain)
- [ ] [A Field Guide to Rapidly Improving AI Products](https://hamel.dev/blog/posts/field-guide/)
- [ ] Video: [Intro to Error Analysis With Just Spreadsheets](https://www.youtube.com/watch?v=BsWxPI9UM4c)
- [ ] **Construir:** corre 50 preguntas reales contra tu RAG v1, revisa cada respuesta a mano en una hoja de cálculo y agrupa los fallos en categorías (recuperó mal, alucinó, citó mal, etc.)

### Semana 15 — Evals automatizadas
- [ ] [Using LLM-as-a-Judge for Evaluation](https://hamel.dev/blog/posts/llm-judge/)
- [ ] [The LLM Evals FAQ](https://hamel.dev/blog/posts/evals-faq/) (léelo por partes, es largo y muy bueno)
- [ ] Herramientas open source, prueba una: [promptfoo](https://www.promptfoo.dev) o [Inspect](https://inspect.aisi.org.uk)
- [ ] **Construir:** dataset de evaluación (≥100 casos), métricas de recuperación (recall@k) y de respuesta (fidelidad a las fuentes con LLM-as-judge, validado contra tus etiquetas manuales)

### Semana 16 — Iterar con datos
- [ ] DeepLearning.AI → *Evaluating AI Agents* (útil también para la fase 5)
- [ ] **Entregable:** RAG v2. Aplica 2–3 mejoras basadas en tu análisis de errores y publica en el README una tabla **v1 vs v2** con las métricas. Esa tabla vale más que cualquier certificado.
- [ ] Integra las evals en CI (GitHub Actions): si una métrica baja, el PR falla

---

## Fase 5 — Agentes, tool use y MCP (semanas 17–20)

### Semana 17 — Cuándo (y cuándo no) usar agentes
- [ ] [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) (Anthropic) — la mejor introducción: flujos de trabajo vs. agentes
- [ ] [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [ ] [Writing Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents)
- [ ] **Construir:** un agente *sin framework* (bucle de tool calling a mano, ~150 líneas) con 3 herramientas. Entender el bucle antes de usar librerías.

### Semana 18 — MCP
- [ ] Anthropic Academy → **Introduction to Model Context Protocol** y **MCP: Advanced Topics** (gratis)
- [ ] [Documentación oficial de MCP](https://modelcontextprotocol.io)
- [ ] [Hugging Face MCP Course](https://huggingface.co/learn/mcp-course) (unidades 1–2)
- [ ] SDK oficial de tu lenguaje: [Go SDK](https://github.com/modelcontextprotocol/go-sdk) o [C# SDK](https://csharp.sdk.modelcontextprotocol.io)

### Semana 19 — Frameworks y patrones multi-agente
- [ ] [Hugging Face Agents Course](https://huggingface.co/learn/agents-course) (unidades 1–2)
- [ ] [Microsoft — AI Agents for Beginners](https://github.com/microsoft/ai-agents-for-beginners): lecciones de planificación, multi-agente y memoria
- [ ] Prueba un framework ligero: [Pydantic AI](https://ai.pydantic.dev) (encaja con todo lo que ya usas)
- [ ] Opcional: whitepapers del *5-Day AI Agents Intensive* de Google y Kaggle (gratis, búscalos en Kaggle)

### Semana 20 — Proyecto 3
**Proyecto 3 — Servidor MCP en Go o C# + agente.** Algo útil de verdad, idealmente ligado a tu trabajo. Ideas: un servidor MCP que da acceso de solo lectura a una base de datos con esquema documentado; uno que consulta tu sistema de tickets o tu API interna.
- [ ] Servidor MCP con 3–5 herramientas bien descritas, validación de entradas y permisos mínimos
- [ ] Agente en Python que lo usa para resolver tareas de varios pasos
- [ ] 20 tareas de evaluación con criterio de éxito claro (¿completó la tarea?, ¿cuántos pasos?, ¿cuánto costó?)
- [ ] README con diagrama de arquitectura y decisiones (por qué agente y no flujo fijo)

---

## Fase 6 — Producción (semanas 21–24)

### Semana 21 — Lecciones de producción
- [ ] [What We've Learned From A Year of Building with LLMs](https://applied-llms.org) — guía táctica, operativa y estratégica
- [ ] [Building A Generative AI Platform](https://huyenchip.com/2024/07/25/genai-platform.html) (Chip Huyen): arquitectura completa de un sistema en producción
- [ ] Caché de prompts, caché semántico, fallbacks entre modelos, timeouts y reintentos

### Semana 22 — Seguridad
- [ ] [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)
- [ ] [Simon Willison sobre prompt injection](https://simonwillison.net/tags/prompt-injection/) (lee los posts sobre la "lethal trifecta")
- [ ] **Construir:** intenta romper tu propio RAG y tu agente con inyección de prompts (p. ej. instrucciones escondidas en un documento indexado). Documenta qué funcionó y añade defensas.

### Semanas 23–24 — Proyecto 4 (capstone)
- [ ] Observabilidad con [Langfuse](https://langfuse.com) (open source, puedes alojarlo tú mismo con Docker): trazas, costos y latencia por petición

**Proyecto 4 — Llevar el Proyecto 2 o el 3 a producción.**
- [ ] Desplegado con Docker en un servicio con capa gratuita
- [ ] Autenticación, rate limiting y límite de gasto por usuario
- [ ] Trazas y dashboard de costos/latencia
- [ ] Evals corriendo en CI
- [ ] Defensas contra prompt injection documentadas
- [ ] **Artículo técnico** (Substack, dev.to o LinkedIn) contando el proyecto: problema, arquitectura, qué falló, métricas antes/después

---

## Mantenerse al día (15 min, 2–3 veces por semana)
- [Blog de Simon Willison](https://simonwillison.net) — lo más útil del día a día, con criterio
- [Blog de ingeniería de Anthropic](https://www.anthropic.com/engineering)
- [Hamel Husain](https://hamel.dev) y [Eugene Yan](https://eugeneyan.com) — práctica de AI engineering
- [Latent Space](https://www.latent.space) — podcast y newsletter de "AI Engineer"
- [Canal AI Engineer en YouTube](https://www.youtube.com/@aiDotEngineer) — charlas de la conferencia AI Engineer

## Para profundizar después de la semana 24 (opcional, gratis)
- [LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) (Sebastian Raschka): el código del libro es gratuito en GitHub
- [Stanford CS336: Language Modeling from Scratch](https://stanford-cs336.github.io): si te pica la curiosidad por el lado de modelos
- Fine-tuning: capítulos de fine-tuning del Hugging Face LLM Course (LoRA, cuándo conviene y cuándo no)

## Libros de pago (para cuando revises el presupuesto, mes 4 en adelante)
Ordenados por prioridad. Ninguno es necesario para completar el plan.
1. **AI Engineering** — Chip Huyen (O'Reilly, 2025). El libro de referencia del rol; cubre evals, RAG, agentes, fine-tuning y producción.
2. **Build a Large Language Model (From Scratch)** — Sebastian Raschka. Complementa la fase 1 si quieres más profundidad.
3. **Designing Data-Intensive Applications** — Martin Kleppmann. No es de IA, pero es la base de sistemas que todo AI engineer en producción necesita.

---

## Progreso por fases
- [ ] Fase 0 — Python para producción
- [ ] Fase 1 — Cómo funcionan los LLMs
- [ ] Fase 2 — APIs + Proyecto 1
- [ ] Fase 3 — RAG + Proyecto 2
- [ ] Fase 4 — Evals
- [ ] Fase 5 — Agentes y MCP + Proyecto 3
- [ ] Fase 6 — Producción + Proyecto 4
