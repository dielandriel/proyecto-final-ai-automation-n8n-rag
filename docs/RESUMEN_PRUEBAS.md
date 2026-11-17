# Resumen de las pruebas

Síntesis de `docs/REGISTRO_PRUEBAS_PASO_17.md`, que contiene el detalle de cada prueba: consultas, respuestas, verificaciones y hallazgos. Todas las pruebas usaron datos ficticios. Los números de ejecución corresponden a la instancia de n8n del autor.

En el registro completo, los identificadores de workflows, credenciales y mensajes de esa instancia se reemplazaron por descripciones entre corchetes.

## 1. Pruebas aprobadas

| Prueba | Alcance | Resultado |
|---|---|---|
| 1 | Chat Trigger → subworkflow → respuesta en el chat (workflows de prueba) | Aprobada |
| 2 | Dimensión de los embeddings de Gemini en n8n | Aprobada: el nodo nativo solo genera 3072 dimensiones |
| 3 | Supabase como Vector Store (inserción y búsqueda) | Aprobada |
| 4 | WF-01: carga de 5 procedimientos (70 fragmentos), búsquedas semánticas y rechazo de procedimientos existentes | Aprobada |
| 5 | Núcleo de WF-03: AI Agent con RAG, cinco tipos de consulta | Aprobada |
| 6 | Seguridad del agente (5 intentos de manipulación) y regresión | Aprobada |
| 7 | Normalización de la salida y redacción sin afirmar acciones no ejecutadas | Aprobada |
| 8 | Google Sheets en lectura, validación del legajo y control de acceso a solicitudes | Aprobada |
| 9 | Minimización de datos enviados al modelo y fallas de lectura de Sheets | Aprobada |
| 10 | Registro en Consultas, prevención de duplicados y fallas de escritura | Aprobada |
| 11 | Preparación del aviso de derivación, sin envío | Aprobada |
| 12 | Columna `estado_aviso` y migración de la planilla | Aprobada |
| 13 | Primer y único envío real del aviso por Gmail | Aprobada: un correo recibido |
| 14 | WF-02 integrado con WF-03: chat, webhook con Header Auth, bloqueo por salud, error de integración | Aprobada y cerrada |
| 15 | WF-04: simulaciones S1 a S3b y falla automática real con Error Trigger (R1) | Aprobada |
| 15 bis | Incidente controlado WF-02 → WF-04 de punta a punta | Aprobada |

## 2. Casos funcionales cubiertos

| Caso | Resultado observado | Pruebas |
|---|---|---|
| Consulta informativa con RAG | RESPONDER con cita de la sección del procedimiento | 5, 7, 10, 14 |
| Estado de una solicitud propia | RESPONDER con estado y fecha registrados | 8, 9, 14 |
| Solicitud de otro empleado | Denegación fija antes del agente, sin revelar si existe | 8, 9, 14 |
| Consulta ambigua | SOLICITAR_ACLARACION, procedimiento y categoría SIN_CLASIFICAR | 5, 7, 10 |
| Excepción que requiere decisión humana | DERIVAR a Administración y Finanzas | 5, 7, 10 |
| Incidente de seguridad informática | DERIVAR a Seguridad Informática con urgencia Alta; aviso real enviado en la Prueba 13 | 5, 13 |
| Tema fuera de alcance | RESPONDER sin tratar el tema | 5, 6 |
| Intento de manipulación del agente | Mantiene el rol, no aprueba, no inventa datos | 6 |
| Posible información clínica | Bloqueo en WF-02 antes del modelo; fila con texto fijo | 14 |
| Mensaje sin legajo | Respuesta fija en WF-02; sin registro | 14 |
| Legajo inexistente o inactivo | Respuesta fija sin agente; fila BLOQUEADO | 8, 10, 14 |
| Reenvío del mismo `id_consulta` | YA_REGISTRADA, sin fila duplicada | 10 |
| Webhook sin token o con token incorrecto | Rechazado (403) | 14 |
| Webhook con credencial válida | 200 y JSON de 7 campos; campos internos enviados por el cliente ignorados | 14 |

## 3. Manejo de errores comprobado

| Falla | Cómo se probó | Resultado |
|---|---|---|
| Límite de solicitudes de Gemini | Ocurrió en condiciones reales con 10 consultas casi simultáneas | `ERR_LLM_NO_DISPONIBLE`, respuesta fija; las consultas repetidas de a una resultaron correctas |
| Salida del agente inválida | Ocurrió en una ejecución real | `ERR_LLM_SALIDA_INVALIDA`, respuesta fija; se ajustó el prompt |
| Lectura de Empleados o Solicitudes | Pestaña inexistente, configurada temporalmente | `ERR_SHEETS`, sin agente |
| Búsqueda o escritura en Consultas | Pestaña inexistente, configurada temporalmente | `ERR_SHEETS`, `registro` = NO_REGISTRADA, sin escritura parcial |
| WF-03 inexistente | Referencia temporal a un workflow inexistente | Respuesta fija; WF-02 reporta `INTEGRACION_WF03` a WF-04; una sola alerta, sin Gmail |
| Falla no prevista | Workflow temporal con Stop and Error llamado por su URL de producción | WF-04 activado por Error Trigger; alerta `FALLA_NO_PREVISTA` sin datos sensibles |
| Mensaje de error contaminado con datos personales | Datos fijados en WF-04 | Ningún dato copiado a la alerta |

## 4. No probado o probado parcialmente

- **Vector Store no disponible (`ERR_VECTOR_STORE`) y tiempo de espera del modelo (`ERR_LLM_TIMEOUT`):** implementados, pero no provocados.
- **Falla real de Gmail:** su clasificación (ERROR_ENVIO o VERIFICACION_MANUAL) se probó solo con pruebas unitarias, y su reporte a WF-04 no se probó de punta a punta.
- **Hosted Chat publicado:** las pruebas usaron el chat del editor.
- **Deduplicación de WF-04 entre ejecuciones de producción:** no se probó.
- **WF-04 como workflow de errores de WF-02 y WF-03:** no está configurado (pendiente de producción).
- **Importación de los JSON saneados en otra instancia:** no se realizó.

## 5. Incidente durante las pruebas

En la Prueba 14, una respuesta de prueba del webhook devolvió los encabezados recibidos, entre ellos el valor del token de autenticación. Se aplicaron estas medidas:

- Rotación del token.
- Eliminación de las ejecuciones afectadas.
- Respuesta del webhook construida campo por campo.
- WF-02 configurado para no guardar ejecuciones.

El incidente quedó remediado y documentado en el registro completo.

## 6. Observaciones de calidad

- **Fuentes citadas:** con el mismo mensaje, las fuentes citadas y la cantidad de búsquedas pueden variar entre ejecuciones. Las decisiones se mantuvieron.
- **Falso positivo del sanitizador de resúmenes:** reemplazó un resumen legítimo que contenía la palabra "certificado".
- **Tiempo de respuesta:** las consultas con agente y registro tardaron aproximadamente entre 10 y 17 segundos.
