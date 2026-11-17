# Índice de evidencias

Capturas auténticas de las interfaces de n8n, Supabase y Google Sheets.

- **Estados:** Pendiente, Aprobada o Rechazada.
- **Criterios de revisión:** integridad, resolución nativa, legibilidad, correspondencia con la prueba, privacidad y autenticidad.
- **Uso en la entrega:** solo las capturas aprobadas se incluyen en el repositorio y se enlazan desde el README.

## A. Imprescindibles

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `01_WF-01_canvas.png` | WF-01 | Canvas completo de la ingesta RAG | — | — | Pendiente | — |
| `02_WF-02_canvas.png` | WF-02 | Canvas completo: chat y webhook, validación previa, respuesta y monitoreo | — | — | Pendiente | — |
| `03_WF-03_canvas.png` | WF-03 | Panorámica del procesamiento | — | — | Pendiente | — |
| `04_WF-04_canvas.png` | WF-04 | Canvas completo del monitoreo de errores | — | — | Pendiente | — |
| `05_ejecucion_CON-205_canvas.png` | WF-03 | Ejecución exitosa con el AI Agent | Prueba 14 A | — | Pendiente | — |
| `06_sheets_consultas_CON-205.png` | Google Sheets | Registro de CON-205 en Consultas | Prueba 14 A | — | Pendiente | — |

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

- Las 14 capturas están pendientes de incorporación.
- No se incluyen imágenes de baja resolución, ampliadas ni reconstruidas para completar el inventario.
- El README enlazará solo las capturas aprobadas.
