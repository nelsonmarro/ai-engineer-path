# Ruta AI Engineer — v2: práctica, con fuentes y certificados oficiales

Plan para pasar de fullstack senior a **AI Engineer**, rehecho con tres reglas:

1. **Fuentes oficiales primero:** el curso o la documentación de quien fabrica la herramienta (Anthropic, Google, Hugging Face, Microsoft, MCP).
2. **Cada semana termina con dos cosas:** algo construido en GitHub y, cuando existe, un certificado o insignia oficial.
3. **Mes 1 a costo $0.** Nada de lo que aparece en las semanas 1–4 requiere tarjeta.

- **Dedicación:** 2 h/día, 5 días por semana (~10 h/semana, ~40 h en el mes 1).
- **Duración total:** 24 semanas. El mes 1 está detallado día por día; los meses 2–6 por semana.
- **Versión anterior:** el plan de 24 semanas original sigue en el historial de git (commit `f040f5e`).

> Verificado el 19 de noviembre de 2026. Precios y catálogos cambian: confirma en la página oficial antes de inscribirte.

---

## Qué significa "certificación oficial" (y qué es gratis)

Hay dos niveles distintos y conviene no confundirlos:

| Nivel | Qué es | Costo | Ejemplos |
|---|---|---|---|
| **A. Certificado o insignia de curso** | Lo emite el propio fabricante al terminar su curso y aprobar el quiz o los ejercicios. No hay examen supervisado. | **Gratis** | Claude Academy (Anthropic), Hugging Face, Kaggle Learn y Google ML Crash Course, Microsoft Learn |
| **B. Certificación con examen supervisado** | Examen vigilado (Pearson VUE), con nota de corte y vigencia. Es lo que una empresa entiende por "certificado". | **De pago** | Microsoft AI-103 (~US$165), Claude Certified Developer/Architect (US$125), Google Generative AI Leader (US$99) |

**Estrategia:** el mes 1 acumula solo nivel A (6–8 credenciales, $0). El nivel B se decide a partir del mes 4, con el portafolio ya hecho. Ver [Certificaciones de pago](#certificaciones-de-pago-decidir-desde-el-mes-4).

Un certificado de nivel A prueba que terminaste el curso, no que sabes construir. Por eso cada uno va pegado a un entregable en código.

---

## Preparación (30 min, antes del día 1)

Crea estas cuentas, todas gratuitas y sin tarjeta:

- [ ] [Claude Academy](https://academy.claude.com) — cursos oficiales de Anthropic (antes "Anthropic Academy")
- [ ] [Hugging Face](https://huggingface.co/join) — cursos, modelos y Spaces
- [ ] [Kaggle](https://www.kaggle.com) — Kaggle Learn (es de Google)
- [ ] [Google AI Studio](https://ai.google.dev) — clave de la API de Gemini, capa gratuita
- [ ] [Microsoft Learn](https://learn.microsoft.com) — con una cuenta Microsoft personal
- [ ] Usa **el mismo nombre completo** en todas: es el que saldrá impreso en los certificados
- [ ] Instala [Ollama](https://ollama.com) y [uv](https://docs.astral.sh/uv/)
- [ ] Crea la carpeta `certificados/` en este repo

### Estructura de una sesión de 2 horas
| Tiempo | Actividad |
|---|---|
| 0:00–0:45 | Curso oficial (video + quiz) |
| 0:45–1:50 | Construir lo de la semana |
| 1:50–2:00 | `diario.md` + commit |

Si te atrasas, **recorta video, nunca el entregable**.

### Modelos sin pagar
- **Ollama** (local) y la **capa gratuita de la API de Gemini**. Los límites gratuitos de Gemini cambian seguido y ya no están en una tabla fija: míralos en AI Studio para tu proyecto.
- La API de Claude es de pago. Los cursos de Claude Academy se pueden seguir y aprobar sin ella; haz los ejercicios de código contra Gemini u Ollama. Traducir el ejemplo a otro proveedor es buena práctica, no un atajo.
- Escribe siempre tu código detrás de una interfaz `LLMClient` para cambiar de proveedor sin reescribir.

---

## MES 1 — Cuatro semanas, $0

| Semana | Tema | Fuente oficial | Entregable | Credencial |
|---|---|---|---|---|
| 1 | Bases: cómo funciona un LLM + Python de producción | Google, Kaggle | `fastapi-llm-template` | Insignia Google + certificado Kaggle |
| 2 | Construir con la API de un modelo | Anthropic | `extractor-estructurado` | Certificado *Building with the Claude API* |
| 3 | MCP | Anthropic, modelcontextprotocol.io | `mcp-server` en Go o C# | 2 certificados de MCP |
| 4 | Agentes | Hugging Face | `agente-a-mano` usando tu servidor MCP | Certificado *Fundamentals* de HF |

### Semana 1 — Bases y entorno

**Credenciales:** insignia *Large language models* (Google) + certificado *Python* (Kaggle).

- [ ] **Día 1 — Entorno.** Proyecto con `uv`, modelo pequeño corriendo en Ollama, primera llamada a Gemini desde Python. Lee [uv: primeros pasos](https://docs.astral.sh/uv/getting-started/).
- [ ] **Día 2 — Cómo funciona un LLM.** [Google Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course), módulo *Large language models*: tokens, transformers, por qué alucinan. Aprueba el quiz → **insignia** en tu perfil de Google Developers.
- [ ] **Día 3 — Python.** [Kaggle Learn: Python](https://www.kaggle.com/learn/python). Ya sabes lo básico: ve directo a los ejercicios. Al completarlos → **certificado**.
- [ ] **Día 4 — Tipado y validación.** [Pydantic: modelos](https://docs.pydantic.dev/latest/concepts/models/) y el [tutorial oficial de FastAPI](https://fastapi.tiangolo.com/tutorial/) hasta *Request Body*.
- [ ] **Día 5 — Entregable.** `fastapi-llm-template`: endpoint `/chat`, interfaz `LLMClient` con dos implementaciones (Ollama y Gemini), configuración por variables de entorno, 3 tests con pytest y Dockerfile. La reutilizas todo el plan.
- [ ] *Opcional, fin de semana:* [Kaggle Learn: Intro to Machine Learning](https://www.kaggle.com/learn/intro-to-machine-learning) → otro certificado.

### Semana 2 — Construir con la API de un modelo

**Credencial:** certificado *Building with the Claude API* (Claude Academy).

- [ ] **Días 1–3 — Curso.** [Claude Academy](https://academy.claude.com) → *Building with the Claude API*. Es el curso más largo del mes: resérvale las tres sesiones. Temas: mensajes, system prompts, salida estructurada, tool use, streaming, evaluación de prompts.
- [ ] **En paralelo:** repite cada ejercicio contra Gemini usando su [documentación oficial](https://ai.google.dev/gemini-api/docs) (structured output y function calling).
- [ ] **Día 4 — Prompts.** [Tutorial interactivo de prompt engineering de Anthropic](https://github.com/anthropics/prompt-eng-interactive-tutorial), capítulos 1–6.
- [ ] **Día 5 — Entregable.** `extractor-estructurado`: recibe texto libre (factura, CV o contrato) y devuelve un objeto Pydantic validado. Si la validación falla, reintenta enviándole el error al modelo. Incluye 15 casos de prueba con resultado esperado y un script que imprime el % de campos correctos.
- [ ] Aprueba la evaluación final → **certificado**.

### Semana 3 — MCP

**Credenciales:** certificados *Introduction to Model Context Protocol* y *Model Context Protocol: Advanced Topics* (Claude Academy).

- [ ] **Día 1 —** *Introduction to Model Context Protocol* → **certificado**.
- [ ] **Día 2 —** [Documentación oficial de MCP](https://modelcontextprotocol.io): arquitectura, tools, resources, prompts. Elige SDK: [Go](https://github.com/modelcontextprotocol/go-sdk) o [C#](https://csharp.sdk.modelcontextprotocol.io).
- [ ] **Días 3–4 — Entregable.** `mcp-server` en Go o C# con 3 herramientas de solo lectura sobre algo que conozcas (una base Postgres con esquema documentado, una API interna o tus propios repos). Validación de entradas y descripciones claras. Pruébalo con el MCP Inspector oficial.
- [ ] **Día 5 —** *Model Context Protocol: Advanced Topics* → **certificado**. Aplica una cosa del curso a tu servidor.

Aquí te diferencias: casi todos los ejemplos de MCP están en Python o TypeScript, y tú lo entregas en Go o C#.

### Semana 4 — Agentes

**Credencial:** certificado *Fundamentals* del Hugging Face Agents Course.

- [ ] **Días 1–2 —** [Hugging Face Agents Course](https://huggingface.co/learn/agents-course), unidades 0 y 1: herramientas, ciclo pensamiento → acción → observación. Aprueba el quiz de la unidad 1 → **certificado Fundamentals**. No tiene fecha límite.
- [ ] **Día 3 —** [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) (Anthropic): cuándo usar un flujo fijo y cuándo un agente.
- [ ] **Días 3–4 — Entregable.** `agente-a-mano`: bucle de tool calling escrito sin framework (~150 líneas) que se conecta a tu `mcp-server` y resuelve 5 tareas de varios pasos. Registra pasos y tokens por tarea.
- [ ] **Día 5 — Cierre del mes.** Sube los PDF a `certificados/`, completa la tabla de abajo, agrégalos a LinkedIn (sección *Licencias y certificaciones*) y escribe 10 líneas en `diario.md`: qué fue fácil, qué no entendiste, qué quieres profundizar.
- [ ] *Opcional:* *Claude Code in Action* (Claude Academy) → otro certificado.

### Credenciales del mes 1

| # | Credencial | Emisor | Semana | Hecho | Enlace |
|---|---|---|---|---|---|
| 1 | Insignia *MLCC: Large language models* | Google | 1 | [ ] | |
| 2 | Certificado *Python* | Kaggle (Google) | 1 | [ ] | |
| 3 | Certificado *Building with the Claude API* | Anthropic | 2 | [ ] | |
| 4 | Certificado *Introduction to MCP* | Anthropic | 3 | [ ] | |
| 5 | Certificado *MCP: Advanced Topics* | Anthropic | 3 | [ ] | |
| 6 | Certificado *Agents Course — Fundamentals* | Hugging Face | 4 | [ ] | |
| 7 | *(opcional)* *Intro to Machine Learning* | Kaggle | 1 | [ ] | |
| 8 | *(opcional)* *Claude Code in Action* | Anthropic | 4 | [ ] | |

**Al terminar el mes 1 tienes:** 6–8 credenciales oficiales, 4 repos pequeños que funcionan y $0 gastados.

---

## MESES 2–6 — Portafolio y certificaciones mayores

Sigue todo gratis salvo que decidas presentarte a un examen.

### Mes 2 (semanas 5–8) — RAG

Este mes pesa más el proyecto que los certificados: casi no hay credenciales oficiales gratuitas de RAG.

- [ ] **Sem 5:** [Hugging Face LLM Course](https://huggingface.co/learn/llm-course), capítulos 1–2 (transformers, tokenizers, embeddings). [pgvector](https://github.com/pgvector/pgvector): índices y distancias. Construir: búsqueda semántica sobre 1.000 frases en Postgres, comparada con full-text.
- [ ] **Sem 6:** [Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) (Anthropic): chunking, búsqueda híbrida, reranking. Documentación de embeddings de Gemini.
- [ ] **Sem 7–8 — Proyecto 1: asistente RAG** sobre un corpus público que te importe. Ingesta con metadatos, búsqueda híbrida, respuestas con citas a la fuente y "no sé" cuando falta contexto. API FastAPI + interfaz mínima en React. Etiqueta `v1` en git.

### Mes 3 (semanas 9–12) — Agentes a fondo

- [ ] **Sem 9–10:** Hugging Face Agents Course, unidades 2–3 (smolagents, LlamaIndex, LangGraph, RAG agéntico).
- [ ] **Sem 11:** unidad 4, reto final → **certificado de finalización** del Agents Course.
- [ ] **Sem 11:** [Hugging Face MCP Course](https://huggingface.co/learn/mcp-course) → **certificado**.
- [ ] **Sem 12:** Claude Academy: *Introduction to agent skills* e *Introduction to subagents* → **2 certificados**.
- [ ] **Proyecto 2:** amplía tu `mcp-server` a 5 herramientas con permisos mínimos y un agente que lo usa. 20 tareas de evaluación con criterio de éxito (¿la completó?, ¿cuántos pasos?, ¿cuánto costó?).

### Mes 4 (semanas 13–16) — Evals, seguridad y producción

Lo que separa a un AI engineer de alguien que hace demos: demostrar con datos que el sistema funciona.

- [ ] **Sem 13:** [Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/). Corre 50 preguntas reales contra tu RAG v1, revísalas a mano y agrupa los fallos.
- [ ] **Sem 14:** evals automatizadas con [promptfoo](https://www.promptfoo.dev) o [Inspect](https://inspect.aisi.org.uk). Dataset de 100 casos, recall@k y fidelidad a las fuentes. Evals en GitHub Actions.
- [ ] **Sem 15:** [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/). Intenta romper tu RAG y tu agente con inyección de prompts y documenta las defensas.
- [ ] **Sem 16:** RAG v2 con tabla **v1 vs v2** en el README. Trazas y costos con [Langfuse](https://langfuse.com) autoalojado.
- [ ] **Revisión de presupuesto:** decide aquí si vas por una certificación de pago.

### Meses 5–6 (semanas 17–24) — Capstone y examen

- [ ] **Sem 17–20 — Proyecto 3 (capstone):** lleva el Proyecto 1 o el 2 a producción. Docker en un servicio con capa gratuita, autenticación, rate limiting, límite de gasto por usuario, evals en CI.
- [ ] **Sem 21–23 — Preparación del examen elegido** (ver abajo) con su ruta oficial gratuita.
- [ ] **Sem 24:** artículo técnico sobre el capstone (problema, arquitectura, qué falló, métricas antes y después) y, si lo decidiste, examen.

---

## Certificaciones de pago (decidir desde el mes 4)

| Certificación | Emisor | Precio aprox. | Para ti |
|---|---|---|---|
| **AI-103 — Azure AI Apps and Agents Developer Associate** | Microsoft | US$165 (varía por país) | **La más recomendable.** Reemplazó a la AI-102 (retirada el 30 de junio de 2026). Se centra en apps y agentes con Microsoft Foundry y encaja con tu experiencia en C#/.NET. Toda la preparación es gratis en Microsoft Learn. |
| **Claude Certified Developer – Foundations** | Anthropic | US$125 | Muy alineada con el mes 1, pero a octubre de 2026 varias fuentes indican que el registro está limitado a empleados de empresas del Claude Partner Network. Confirma antes de planificarla. |
| **Generative AI Leader** | Google Cloud | US$99 | No: está pensada para perfiles no técnicos. |

**Cómo bajar el costo de la AI-103:** Microsoft reparte vales en sus eventos. En el AI Skills Fest de junio de 2026 dio vales del 100 % a quien completó una lista de cursos en Microsoft Learn. Sigue el *Skills Hub Blog* de Microsoft Tech Community y apúntate a los retos cuando salgan.

- Página oficial del examen: [AI-103](https://learn.microsoft.com/credentials/certifications/exams/ai-103/)
- Guía de estudio oficial: [AI-103 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/ai-103)

---

## Mantenerse al día (15 min, 2–3 veces por semana)
- [Blog de ingeniería de Anthropic](https://www.anthropic.com/engineering)
- [Blog de Simon Willison](https://simonwillison.net)
- [Hamel Husain](https://hamel.dev) y [Eugene Yan](https://eugeneyan.com)

## Teoría opcional (cuando te pique la curiosidad)
Lo que el plan anterior traía como fase obligatoria y aquí queda como extra:
- [3Blue1Brown — Neural Networks](https://www.3blue1brown.com/topics/neural-networks)
- [Karpathy — Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html) (construir un mini-GPT)
- Resto del [Google Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course): cada módulo da insignia

---

## Progreso
- [ ] Preparación (cuentas y entorno)
- [ ] Semana 1 — Bases y entorno
- [ ] Semana 2 — API de modelos
- [ ] Semana 3 — MCP
- [ ] Semana 4 — Agentes
- [ ] Mes 2 — RAG + Proyecto 1
- [ ] Mes 3 — Agentes a fondo + Proyecto 2
- [ ] Mes 4 — Evals, seguridad, producción
- [ ] Meses 5–6 — Capstone + examen
