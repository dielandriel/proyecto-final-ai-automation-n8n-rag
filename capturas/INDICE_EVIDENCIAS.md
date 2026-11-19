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
| `03_WF-03_canvas.png` | WF-03 | Panorámica de WF-03: seis notas, grupos de validación, AI Agent con RAG, registro y aviso, y sus conexiones | — | 1158 × 515 | Aprobada como panorámica contextual (lote 4) | Solo muestra la estructura general: los nombres de nodos no son legibles a este tamaño y se leen en los detalles publicados (`03a`, `03b1`, `03b2`, `03c1`, `03c2`, `03d`, `03d1`, `03d2`, `03n1` a `03n6`). En el borde inferior derecho se ve un ícono flotante del escritorio ajeno a n8n |
| `04_WF-04_canvas.png` | WF-04 | Canvas completo del monitoreo de errores: cuatro Sticky Notes, dos entradas y recorrido hasta el resultado | — | 1160 × 533 | Aprobada como panorámica contextual, en conjunto con 04a, 04b1 y 04b2 (lote 1) | Las cuatro notas se ven, pero su texto **no es legible** a esta escala. Los nombres de nodos se leen en los detalles |
| `05_ejecucion_CON-205_canvas.png` | WF-03 | Ejecución histórica 206 de WF-03 en la vista Executions: Succeeded en 8,863 s, 16/11 19:33:32, recorrido del canvas | Prueba 14 A | 1163 × 532 | Aprobada (lote CON-205 1) | La imagen muestra el número de ejecución, no el id de consulta: el vínculo con CON-205 lo acreditan `05a` y el registro de pruebas. Los nombres de nodos no son legibles a esta escala |
| `06_sheets_consultas_CON-205.png` | Google Sheets | Pestaña Consultas de BASE_DATOS_PROYECTO_FINAL, fila 14 seleccionada (A14 = CON-205): CHAT, LEG-0007, PRO-RH-001, Informativa, RESPONDER, COMPLETADO, sin código de error, urgencia Normal | Prueba 14 A | 1352 × 252 | Aprobada en conjunto con 06b (lote CON-205 2) | **Edición de privacidad:** avatar de la cuenta cubierto con un relleno opaco del color del fondo (x 1303–1348, y 7–49). Algunas columnas (`fecha_hora`, `resumen`) aparecen truncadas por su ancho |
| `06b_sheets_consultas_CON-205_estado_aviso.png` | Google Sheets | Continuación de la misma fila 14 (columnas D a Q; A14 = CON-205 en la barra de fórmulas): fuentes `PRO-RH-001 §7; PRO-RH-001 §13`, sin `id_solicitud` ni `estado_derivacion`, `estado_aviso` NO_APLICA | Prueba 14 A | 1360 × 530 | Aprobada en conjunto con 06 (lote CON-205 2) | **Edición de privacidad:** la misma que en `06`. Las demás filas visibles son registros ficticios de otras pruebas |

**Revisión del lote 2 de CON-205 (`06`, `06b`):**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen. Ninguna coincide con imágenes ya publicadas.
- **Metadatos:** sin bloques de texto, EXIF ni perfiles.
- **Misma fila:** ambas muestran la fila 14 de la pestaña Consultas, seleccionada, con A14 = CON-205 en la barra de fórmulas.
- **Correspondencia con la ejecución 206** (`05a`, `05b1`), sin discrepancias:
  - `id_consulta`, `origen`, `legajo`, `procedimiento`, `categoria` y `decision`.
  - `estado_procesamiento`, `codigo_error`, `area_destino` y `urgencia`.
  - `fuentes` e `id_solicitud`.
- **Coincidencia con el registro de pruebas:** `estado_aviso` NO_APLICA, como en la Prueba 14 A.
- **Edición de privacidad:** solo el avatar de la cuenta. Se verificó que la zona superior derecha (desde x 1290, y 0–60) es un color uniforme en ambas imágenes, sin restos de la fotografía. No se alteraron celdas, valores ni encabezados.
- **Privacidad:** sin correos, URL ni identificadores de la planilla. Los datos visibles son ficticios.
- **Conjunto:** `06` identifica la fila y sus primeras columnas; `06b` muestra la continuación hasta `estado_aviso`. Juntas prueban el registro de CON-205 con decisión RESPONDER, procedimiento PRO-RH-001, estado COMPLETADO y `estado_aviso` NO_APLICA.

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

## A3. WF-03: AI Agent, herramientas y notas explicativas (lote 1)

Evidencias parciales de WF-03. Complementan la panorámica contextual `03_WF-03_canvas.png` (lote 4). La configuración RAG se documenta en la sección B (lote 5). La validación y el registro (lote 2) y el aviso de derivación (lote 3) se documentan en la sección B.

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `03b1_WF-03_agente.png` | WF-03 | Grupo "AI Agent con RAG" y nodo "Agente de procesos internos" con sus conectores de modelo, memoria, herramientas y Output Parser, y salidas Success y Error | — | 967 × 527 | Aprobada | Las etiquetas de los conectores aparecen recortadas por la interfaz de n8n; los subnodos se identifican en `03b2` |
| `03b2_WF-03_herramientas.png` | WF-03 | Subnodos del agente: Google Gemini Chat Model, `buscar_procedimientos` con Embeddings Google Gemini, `consultar_mis_solicitudes` y Salida estructurada | — | 1032 × 535 | Aprobada | Nombres de nodos y tipos de conexión legibles |
| `03n1_WF-03_nota_general.png` | WF-03 | Sticky Note "WF-03 Procesamiento": función del subworkflow y límites del modelo | — | 777 × 543 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `03n2_WF-03_nota_controles.png` | WF-03 | Sticky Note "Controles previos al agente (sin IA)" | — | 814 × 504 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `03n3_WF-03_nota_agente_rag.png` | WF-03 | Sticky Note "AI Agent con RAG": modelo, herramientas, decisiones y validación | — | 1037 × 535 | Aprobada | Texto idéntico a la nota del JSON exportado |
| `03n4_WF-03_nota_registro.png` | WF-03 | Sticky Note "Registro en Consultas" | — | 1158 × 512 | Aprobada | Texto idéntico a la nota del JSON exportado. En el borde inferior derecho se ve parcialmente un ícono flotante del escritorio, ajeno a n8n, sin información |
| `03n5_WF-03_nota_aviso.png` | WF-03 | Sticky Note "Aviso de derivación por Gmail": condiciones del aviso y envío deshabilitado en la entrega | — | 1110 × 534 | Aprobada | Texto idéntico a la nota del JSON exportado. Mismo ícono flotante, apenas visible en el borde |
| `03n6_WF-03_nota_respuesta.png` | WF-03 | Sticky Note "Respuesta a WF-02" y credenciales a reconfigurar | — | 794 × 522 | Aprobada | Texto idéntico a la nota del JSON exportado |

**Revisión del lote 1 de WF-03:**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen.
- **Metadatos:** sin bloques de texto ni EXIF; solo perfil de color y densidad.
- **Correspondencia:** las seis Sticky Notes coinciden textualmente con `workflows/WF-03_procesamiento.json`, y los nombres del agente y sus cinco subnodos existen en el export.
- **Ediciones:** ninguna declarada ni detectada.
- **Privacidad:** sin URL de la instancia, correos, credenciales, identificadores ni datos de ejecuciones.
- **Resolución:** inferior al objetivo de calidad del proyecto (1920 × 1080). Legibles a tamaño real; la consigna no fija una resolución mínima.

## A4. Detalle de WF-04

Complementan la panorámica `04_WF-04_canvas.png`; se aprueban en conjunto con ella.

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `04a_WF-04_recepcion_clasificacion.png` | WF-04 | Entradas Falla no prevista (Error Trigger) e Incidente controlado, sus normalizaciones, Clasificar y validar incidente y Es incidente (true/false) | — | 1002 × 542 | Aprobada (lote 1) | Nombres de nodos legibles |
| `04b1_WF-04_deduplicacion_preparacion.png` | WF-04 | Evitar duplicados, Incidente nuevo (true/false), Configuración de alerta y Preparar alerta | — | 786 × 529 | Aprobada con limitación (lote 1) | El nodo **Evitar duplicados está parcialmente recortado** en el borde izquierdo: esta imagen sola no lo demuestra completo. Su ubicación se ve en la panorámica |
| `04b2_WF-04_envio_resultado.png` | WF-04 | Preparar alerta, Enviar alerta (true/false), Enviar alerta por Gmail (`send: message`) y Resultado del monitoreo | — | 823 × 514 | Aprobada (lote 1) | Nombres de nodos legibles |

**Revisión del lote 1 de WF-04:**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen. Ninguna coincide con imágenes ya publicadas.
- **Metadatos:** sin bloques de texto ni EXIF.
- **Correspondencia:** los 13 nodos funcionales, las cuatro Sticky Notes y las conexiones (incluidas las ramas true y false de Es incidente, Incidente nuevo y Enviar alerta) coinciden con `workflows/WF-04_monitoreo_errores.json`.
- **Ediciones:** ninguna.
- **Privacidad:** sin correos, destinatarios, URL de la instancia ni valores de credenciales. No se abrieron configuraciones.
- **Limitaciones:**
  - El texto de las cuatro Sticky Notes no es legible en la panorámica. Su contenido consta en el JSON exportado; para mostrarlo en imágenes harían falta capturas adicionales.
  - En `04b1`, el nodo Evitar duplicados aparece recortado a la izquierda.
  - Las capturas no acreditan por sí mismas que el envío de Gmail esté deshabilitado; eso consta en el JSON y en la documentación.

## B. Detalle de WF-03 y de la ejecución

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `03a_WF-03_validacion_empleado.png` | WF-03 | Grupo "Validación del empleado": Buscar empleado, Validar empleado y Empleado habilitado (salidas true y false), seguido de Leer solicitudes propias; controles sin IA | — | 1111 × 503 | Aprobada (lote 2) | Reemplaza el nombre previsto `03a_WF-03_controles_previos.png`. La descripción del grupo aparece truncada por la interfaz; el texto completo está en el JSON |
| `03b_WF-03_ai_agent_herramientas.png` | WF-03 | AI Agent, Gemini, `buscar_procedimientos` con embeddings, `consultar_mis_solicitudes` y Structured Output Parser | — | — | Cubierta por `03b1` y `03b2` (sección A3) | No se generará un archivo con este nombre |
| `03c_WF-03_registro_consultas.png` | WF-03 | Registro en Google Sheets sin duplicados | — | — | Cubierta por `03c1` y `03c2` | No se generará un archivo con este nombre |
| `03c1_WF-03_registro_preparacion_y_control.png` | WF-03 | Grupo "Registro en Consultas": Preparar registro, Buscar consulta registrada, Decidir inserción y Consulta nueva (control de duplicados) | — | 1105 × 535 | Aprobada (lote 2) | "Registrar consulta" aparece en el borde; se ve completo en `03c2` |
| `03c2_WF-03_registro_confirmacion.png` | WF-03 | Bifurcación de Consulta nueva: Registrar consulta (append), Confirmar registro y Resultado del registro; la rama false omite la escritura | — | 979 × 528 | Aprobada (lote 2) | Complementa `03c1` |
| `03d_WF-03_aviso_derivacion.png` | WF-03 | Panorámica del grupo "Aviso de derivación" completo: nueve nodos y sus conexiones | — | 1094 × 534 | Aprobada en conjunto con 03d1 y 03d2 (lote 3) | Muestra la estructura; las etiquetas pequeñas se leen en los detalles. No acredita por sí sola el estado de Gmail: el envío deshabilitado y los destinatarios vacíos constan en el JSON y en la documentación |
| `03d1_WF-03_preparacion_aviso.png` | WF-03 | Detalle izquierdo: Configuración del aviso, Preparar aviso, Enviar aviso (true/false), Enviar correo de aviso (`send: message`), Resultado del aviso, Registrar estado del aviso y Actualizar estado del aviso | — | 1018 × 521 | Aprobada en conjunto (lote 3) | Cobertura parcial del grupo: el sector derecho continúa en `03d2` |
| `03d2_WF-03_confirmacion_aviso.png` | WF-03 | Detalle derecho: Resultado del aviso, Registrar estado del aviso (true/false), Actualizar estado del aviso (`update: sheet`), Confirmar estado del aviso y Respuesta final | — | 914 × 511 | Aprobada en conjunto (lote 3) | Cobertura parcial del grupo: el sector izquierdo está en `03d1` |
| `03e_WF-03_rag_buscar_procedimientos.png` | WF-03 / Supabase | Configuración del Vector Store como herramienta | — | — | Cubierta por `03e1` y `03e2` (lote 5) | No se generará un archivo con este nombre |
| `03e1_WF-03_rag_operacion_y_tabla.png` | WF-03 / Supabase | Panel Parameters de `buscar_procedimientos`: modo "Retrieve Documents (As Tool for AI Agent)", credencial referenciada, descripción de la herramienta y tabla `documents` | — | 1084 × 509 | Aprobada en conjunto con 03e2 (lote 5) | La credencial figura con su nombre en la instancia ("Supabase account"); el JSON entregado usa el nombre genérico "Supabase API". No se muestra ningún secreto |
| `03e2_WF-03_rag_parametros_busqueda.png` | WF-03 / Supabase | Tabla `documents`, Limit 5, Include Metadata activo, Rerank Results desactivado y Query Name `match_documents` | — | 1079 × 488 | Aprobada en conjunto con 03e1 (lote 5) | Documenta configuración, no una ejecución RAG |
| `05a_ejecucion_CON-205_validar_decision.png` | WF-03 | Nodo "Validar decisión" de la ejecución 206: salida del agente y del nodo con `id_consulta` CON-205, origen CHAT, decisión RESPONDER, PRO-RH-001, categoría Informativa, urgencia Normal y la respuesta con citas | Prueba 14 A | 1339 × 561 | Aprobada (lote CON-205 1) | Se ve el aviso de migración a n8n 3.0 de la interfaz, sin datos. LEG-0007 es un legajo ficticio |
| `05b1_ejecucion_CON-205_rag_control.png` | WF-03 | Continuación de la salida JSON de "Validar decisión": fuentes `PRO-RH-001 §7; PRO-RH-001 §13`, estado COMPLETADO, sin código de error ni advertencias, `control.rag.usado` = true y `cantidad_busquedas` = 1 | Prueba 14 A | 1317 × 494 | Aprobada en conjunto con 05b2 (lote CON-205 1) | Parte del mismo JSON guardado |
| `05b2_ejecucion_CON-205_rag_fragmentos.png` | WF-03 | Trazabilidad RAG: la consulta enviada a `buscar_procedimientos` y los cinco fragmentos recuperados, todos de PRO-RH-001 (§7, §13, §4, §6 y §0) | Prueba 14 A | 1326 × 480 | Aprobada en conjunto con 05b1 (lote CON-205 1) | Las secciones citadas en la respuesta (§7 y §13) están entre los fragmentos recuperados |

**Revisión del lote 2 de WF-03 (`03a`, `03c1`, `03c2`):**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen.
- **Metadatos:** sin bloques de texto ni EXIF; solo perfil de color y densidad.
- **Correspondencia:** los 11 nodos visibles, los nombres y descripciones de los grupos "Validación del empleado" y "Registro en Consultas" y las conexiones mostradas (incluidas las ramas true y false) coinciden con `workflows/WF-03_procesamiento.json`.
- **Ediciones:** ninguna declarada ni detectada.
- **Privacidad:** sin URL de la instancia, correos, credenciales, identificadores ni datos de ejecuciones. En `03a` y `03c1` se ve parcialmente, en el borde inferior derecho, el mismo ícono flotante del escritorio ajeno a n8n.
- **Resolución:** de 979 × 528 a 1111 × 503; nombres de nodos legibles a tamaño real.

**Revisión del lote 3 de WF-03 (`03d`, `03d1`, `03d2`):**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen.
- **Metadatos:** sin bloques de texto ni EXIF; solo perfil de color y densidad.
- **Correspondencia:** los nueve nodos del grupo "Aviso de derivación" y sus conexiones, incluidas las ramas true y false de "Enviar aviso" y "Registrar estado del aviso", coinciden con `workflows/WF-03_procesamiento.json`.
- **Cobertura:** la panorámica muestra el grupo completo; cada detalle cubre una parte. El aviso queda documentado por el conjunto de las tres imágenes.
- **Ediciones:** ninguna declarada ni detectada.
- **Privacidad:** sin destinatarios de correo, URL de la instancia, credenciales ni datos de ejecuciones. No se abrió la configuración de los nodos del aviso.
- **Resolución:** de 914 × 511 a 1094 × 534; nombres de nodos legibles en los detalles.

**Revisión del lote 4 de WF-03 (`03`):**

- **Archivos recibidos:** cuatro. `03d`, `03d1` y `03d2` tienen la misma huella SHA-256 que las ya publicadas en el lote 3, por lo que no se volvieron a subir. Solo se incorporó la panorámica `03`.
- **Integridad:** PNG válido, sin errores de CRC; dimensiones y huella idénticas a las del manifiesto.
- **Metadatos:** sin bloques de texto ni EXIF.
- **Correspondencia:** se distinguen las seis Sticky Notes y los cuatro grupos de `workflows/WF-03_procesamiento.json` (validación del empleado, AI Agent con RAG, registro en Consultas y aviso de derivación), en la disposición del canvas.
- **Ediciones:** ninguna.
- **Privacidad:** sin URL, correos, credenciales ni datos de ejecuciones.
- **Uso:** panorámica contextual; no se usa como evidencia de nombres o parámetros.

**Revisión del lote 5 de WF-03 (`03e1`, `03e2`):**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen.
- **Metadatos:** sin bloques de texto ni EXIF.
- **Correspondencia con `workflows/WF-03_procesamiento.json`** (nodo `buscar_procedimientos`, Supabase Vector Store 1.4):
  - Modo `retrieve-as-tool`, tabla `documents`, `topK` 5 y `queryName` `match_documents`.
  - Include Metadata y Rerank Results con sus valores por defecto (activo y desactivado).
  - La descripción visible coincide con `toolDescription`.
- **Ediciones:** ninguna.
- **Privacidad:**
  - Sin tokens, contraseñas, URL ni correos.
  - El panel de entrada muestra solo nombres de campos (esquema), sin valores.
  - El nombre visible de la credencial no revela el secreto.
- **Alcance:** documentan la configuración, no una ejecución RAG exitosa, que sigue pendiente como evidencia independiente (CON-205).

**Revisión del lote 1 de la ejecución CON-205 (`05`, `05a`, `05b1`, `05b2`):**

- **Integridad:** PNG válidos, sin errores de CRC; dimensiones y huellas SHA-256 idénticas a las del manifiesto de origen. Ninguna coincide con imágenes ya publicadas.
- **Metadatos:** sin bloques de texto ni EXIF.
- **Ediciones:** ninguna.
- **Correspondencia con `docs/REGISTRO_PRUEBAS_PASO_17.md`** (Prueba 14 A):
  - Ejecución 205 de WF-02 → ejecución 206 de WF-03 → CON-205.
  - Decisión RESPONDER, PRO-RH-001, categoría Informativa.
  - El texto de la respuesta coincide con el del registro.
- **Evidencia RAG:** una búsqueda (`cantidad_busquedas` = 1) y cinco fragmentos recuperados, todos de PRO-RH-001.
- **Alcance:** las imágenes muestran la salida guardada de una ejecución histórica; no se ejecutó ninguna prueba nueva. No se abrieron los nodos del aviso de esa ejecución.
- **Privacidad:** sin URL de la instancia, correos, destinatarios, tokens ni credenciales. Los datos visibles (legajo y consulta) son ficticios.
- **Resolución:** de 1163 × 532 a 1339 × 561; el contenido relevante se lee en `05a`, `05b1` y `05b2`.

## C. Complementarias

| Archivo | Workflow o sistema | Qué demuestra | Prueba | Resolución original | Estado | Observaciones |
|---|---|---|---|---|---|---|
| `07_supabase_documents.png` | Supabase | 70 fragmentos en la tabla `documents` | Prueba 4 | — | Pendiente | — |
| `08_WF-01_ejecucion_ingesta.png` | WF-01 | Ingesta de 5 documentos y 70 fragmentos | Prueba 4 | — | Pendiente | — |

## Estado

- **WF-01:** 6 capturas aprobadas (panorámica `01` y detalles `01a` a `01e`).
- **WF-02:** 6 capturas aprobadas (panorámica `02` y detalles `02a` a `02e`).
- **WF-03:** 17 capturas aprobadas. La documentación visual de la configuración está completa.
- **Ejecución exitosa CON-205:** 4 capturas aprobadas (`05`, `05a`, `05b1`, `05b2`): ejecución 206 exitosa, decisión RESPONDER y trazabilidad RAG.
  - Lote 1: agente, herramientas y seis notas.
  - Lote 2: validación `03a` y registro `03c1` y `03c2`.
  - Lote 3: aviso de derivación, con la panorámica `03d` y los detalles `03d1` y `03d2`.
  - Lote 4: panorámica contextual `03`.
  - Lote 5: configuración RAG `03e1` y `03e2`.
- **Registro de CON-205 en Google Sheets:** 2 capturas aprobadas en conjunto (`06`, `06b`).
- **WF-04:** 4 capturas aprobadas en conjunto (panorámica `04` y detalles `04a`, `04b1` y `04b2`), con las limitaciones de legibilidad indicadas en la sección A4.
- **Pendientes de decisión:** complementarias (`07`, `08`). Opcionales: capturas legibles de las notas de WF-04 y una toma completa de Evitar duplicados.
- No se incluyen imágenes ampliadas ni reconstruidas para completar el inventario.
- El README enlaza solo las capturas aprobadas.
