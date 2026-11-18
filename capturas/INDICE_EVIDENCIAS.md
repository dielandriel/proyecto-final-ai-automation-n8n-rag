# Índice de evidencias

Capturas auténticas de las interfaces de n8n, Supabase y Google Sheets.

- **Estados:** Pendiente, Aprobada o Rechazada.
- **Criterios de revisión:** integridad, resolución nativa, legibilidad, correspondencia con la prueba, privacidad y autenticidad.
- **Uso en la entrega:** solo las capturas aprobadas se incluyen en el repositorio y se enlazan desde el README.

## A. Imprescindibles

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `01_WF-01_canvas.png` | WF-01 | Canvas completo de la ingesta RAG | — | 1315 × 523 | Aprobada en conjunto con 01a–01e | Panorámica contextual: las etiquetas son pequeñas y se leen en las capturas de detalle |
| `02_WF-02_canvas.png` | WF-02 | Canvas completo: chat y webhook, validación previa, respuesta y monitoreo | — | 1162 × 519 | Aprobada en conjunto con 02a–02e | Panorámica contextual: las etiquetas son pequeñas y se leen en las capturas de detalle. Identificadores de WF-03 y WF-04 ocultos |
| `03_WF-03_canvas.png` | WF-03 | Panorámica del procesamiento | — | — | Pendiente | — |
| `04_WF-04_canvas.png` | WF-04 | Canvas completo del monitoreo de errores | — | — | Pendiente | — |
| `05_ejecucion_CON-205_canvas.png` | WF-03 | Ejecución exitosa con el AI Agent | Prueba 14 A | — | Pendiente | — |
| `06_sheets_consultas_CON-205.png` | Google Sheets | Registro de CON-205 en Consultas | Prueba 14 A | — | Pendiente | — |

## A1. Detalle de WF-01

Complementan la panorámica `01_WF-01_canvas.png`; se aprueban en conjunto con ella.

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `01a_WF-01_nota_ingesta.png` | WF-01 | Sticky Note "WF-01 Ingesta documental RAG": objetivo, entrada con n8n User Auth y rechazo de procedimientos existentes | — | 701 × 552 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `01b_WF-01_nota_control.png` | WF-01 | Sticky Note "Control antes de escribir" | — | 743 × 527 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `01c_WF-01_nota_vectorizacion.png` | WF-01 | Sticky Note "Vectorización": fragmentación, `gemini-embedding-001` (3072 dimensiones), tabla y función de Supabase | — | 1049 × 559 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `01d_WF-01_entrada_validacion.png` | WF-01 | Formulario de carga, lectura de archivos, validación del documento, búsqueda en Supabase y clasificación | — | 1222 × 557 | Aprobada | Nombres de nodos legibles |
| `01e_WF-01_supabase_embeddings.png` | WF-01 | Supabase Vector Store con Embeddings Google Gemini, Default Data Loader, Recursive Character Text Splitter y Resumen de la ingesta | — | 1333 × 575 | Aprobada | Nombres de nodos y conexiones de subnodos legibles |

**Revisión de las capturas de WF-01:**

- **Integridad:** PNG válidos; huellas SHA-256 idénticas a las del manifiesto de origen.
- **Metadatos:** sin bloques de texto ni EXIF; solo perfil de color y densidad (96 ppp).
- **Correspondencia:** los 14 nodos funcionales y las 3 Sticky Notes coinciden con `workflows/WF-01_ingesta_documental_rag.json`.
- **Privacidad:** sin URL de la instancia, correos, credenciales ni datos de ejecuciones. Se ve el nombre del proyecto ("Personal") y el contador del plan de n8n.
- **Resolución:** inferior al objetivo de calidad del proyecto (1920 × 1080). Aceptada por el responsable porque los detalles son legibles a tamaño real; la consigna no fija una resolución mínima.

## A2. Detalle de WF-02

Complementan la panorámica `02_WF-02_canvas.png`; se aprueban en conjunto con ella.

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución final | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `02a_WF-02_notas_recepcion_validacion.png` | WF-02 | Sticky Notes "WF-02 Recepción" y "Validación previa (sin IA)": dos entradas, autenticación y controles antes del modelo | — | 954 × 515 | Aprobada | Texto idéntico a las notas del JSON exportado. Borde inferior recortado |
| `02b_WF-02_nota_procesamiento.png` | WF-02 | Sticky Note "Procesamiento": llamada a WF-03 y respuesta fija ante falla | — | 537 × 496 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `02c_WF-02_nota_respuesta_monitoreo.png` | WF-02 | Sticky Note "Respuesta y monitoreo": salida por canal y reporte de incidentes a WF-04 | — | 906 × 452 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `02d_WF-02_entradas_validacion.png` | WF-02 | Chat y webhook, normalización, validación previa, ruta de la consulta (válida, bloqueada, rechazada), llamada a WF-03 y registro de bloqueo | — | 1087 × 514 | Aprobada | Nombres de nodos y salidas legibles. Identificador de WF-03 oculto |
| `02e_WF-02_respuesta_monitoreo.png` | WF-02 | Respuesta final, detección y reporte de incidentes a WF-04, y respuesta por webhook o por chat | — | 947 × 510 | Aprobada | Nombres de nodos legibles. Identificador de WF-04 oculto. Borde inferior recortado |

**Revisión de las capturas de WF-02:**

- **Integridad:** PNG válidos; huellas SHA-256 idénticas a las del manifiesto de origen.
- **Metadatos:** sin bloques de texto, EXIF ni perfiles; solo los datos de imagen.
- **Correspondencia:** los 15 nodos funcionales y las 4 Sticky Notes coinciden con `workflows/WF-02_recepcion.json`.
- **Ediciones de privacidad declaradas en el manifiesto:**
  - En `02`, `02d` y `02e` se cubrió con un rectángulo opaco el texto bajo "Procesar en WF-03" y "Reportar incidente a WF-04", que muestra el identificador interno del workflow enlazado. Se comprobó que los rectángulos son uniformes y no dejan texto legible.
  - En `02a` y `02e` se recortó el borde inferior, que mostraba un identificador de sesión del chat.
  - No se modificaron nombres de nodos, conexiones, notas ni resultados.
- **Privacidad:** sin URL de la instancia, correos, credenciales ni datos de ejecuciones.
- **Resolución:** inferior al objetivo de calidad del proyecto (1920 × 1080). Los detalles son legibles a tamaño real; la consigna no fija una resolución mínima.

## B. Detalle de WF-03 y de la ejecución

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `03a_WF-03_controles_previos.png` | WF-03 | Validación del legajo y control de acceso, sin IA | — | — | Pendiente | — |
| `03b_WF-03_ai_agent_herramientas.png` | WF-03 | AI Agent, Gemini, `buscar_procedimientos` con embeddings, `consultar_mis_solicitudes` y Structured Output Parser | — | — | Pendiente | — |
| `03c_WF-03_registro_consultas.png` | WF-03 | Registro en Google Sheets sin duplicados | — | — | Pendiente | — |
| `03d_WF-03_aviso_derivacion.png` | WF-03 | Aviso de derivación por Gmail y `estado_aviso` | — | — | Pendiente | — |
| `03e_WF-03_rag_buscar_procedimientos.png` | WF-03 / Supabase | Configuración del Vector Store como herramienta | — | — | Pendiente | — |
| `05a_ejecucion_CON-205_validar_decision.png` | WF-03 | Decisión RESPONDER, respuesta con citas, fuentes y fragmentos recuperados | Prueba 14 A | — | Pendiente | Puede dividirse en `05a` y `05b` |

## C. Complementarias

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `07_supabase_documents.png` | Supabase | 70 fragmentos en la tabla `documents` | Prueba 4 | — | Pendiente | — |
| `08_WF-01_ejecucion_ingesta.png` | WF-01 | Ingesta de 5 documentos y 70 fragmentos | Prueba 4 | — | Pendiente | — |

## Estado

- **WF-01:** 6 capturas aprobadas (panorámica `01` y detalles `01a` a `01e`).
- **WF-02:** 6 capturas aprobadas (panorámica `02` y detalles `02a` a `02e`).
- **Pendientes:** las demás evidencias previstas, entre ellas los canvas de WF-03 y WF-04 y la ejecución exitosa CON-205 que exige la consigna.
- No se incluyen imágenes ampliadas ni reconstruidas para completar el inventario.
- El README enlaza solo las capturas aprobadas.
