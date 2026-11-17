# Instalación y configuración

Guía para reproducir el prototipo en otra cuenta, sin las cuentas ni las credenciales del autor.

Los pasos se basan en la configuración exportada y en el informe de saneamiento. La importación de los JSON saneados en otra instancia no fue probada: ver la sección 9.

## 1. Requisitos

| Componente | Uso | Notas |
|---|---|---|
| n8n (Cloud o autoalojado reciente) | Ejecuta los cuatro workflows | Desarrollado en n8n Cloud, noviembre de 2026. Los nodos usan versiones recientes (por ejemplo, AI Agent 3.1, Google Sheets 4.7, Gmail 2.2, Chat Trigger 1.5, Webhook 2.2) |
| Clave de API de Google Gemini | Modelo del agente (`models/gemini-3.5-flash-lite`) y embeddings (`models/gemini-embedding-001`) | En el nivel gratuito, el proveedor puede usar el contenido para mejorar sus productos: usar solo datos ficticios |
| Proyecto de Supabase con pgvector | Vector Store de los procedimientos | El plan gratuito alcanza; pausa los proyectos con poca actividad |
| Cuenta de Google con Google Sheets | Planilla de datos (Empleados, Solicitudes y Consultas) | OAuth2 desde n8n |
| Cuenta de Gmail (opcional) | Avisos de derivación y alertas de monitoreo | En la entrega el envío está deshabilitado |

## 2. Supabase

1. Crear un proyecto.
2. Ejecutar `docs/supabase/esquema_supabase.sql` en el editor SQL. El script crea:
   - La extensión pgvector.
   - La tabla `documents` con `embedding vector(3072)`.
   - La función `match_documents`, con similitud coseno.
   - RLS activado sin políticas y permisos solo para el rol de servicio.
3. Ejecutar las consultas de verificación del final del script.

La dimensión de 3072 es obligatoria: el nodo Embeddings Google Gemini de n8n no permite reducirla, y la ingesta y la búsqueda deben usar el mismo modelo.

## 3. Planilla de Google Sheets

Crear una planilla con tres pestañas y estos encabezados en la fila 1, en este orden. El detalle de tipos y valores permitidos está en `docs/DISENO_BASE_DATOS_PROYECTO_FINAL.md`.

| Pestaña | Columnas |
|---|---|
| Empleados (11) | `legajo`, `nombre`, `apellido`, `correo`, `area`, `puesto`, `sede`, `legajo_jefe`, `fecha_ingreso`, `saldo_vacaciones`, `activo` |
| Solicitudes (12) | `id_solicitud`, `procedimiento`, `legajo`, `fecha_carga`, `estado`, `fecha_estado`, `fecha_inicio`, `fecha_fin`, `prioridad`, `importe`, `centro_costo`, `detalle` |
| Consultas (16) | `id_consulta`, `fecha_hora`, `origen`, `legajo`, `procedimiento`, `categoria`, `decision`, `estado_procesamiento`, `codigo_error`, `area_destino`, `urgencia`, `resumen`, `fuentes`, `id_solicitud`, `estado_derivacion`, `estado_aviso` |

Reglas:

- **Nombres de pestañas:** deben ser exactamente Empleados, Solicitudes y Consultas.
- **Empleados y Solicitudes:** cargarlas con datos ficticios. Se puede partir de la copia demostrativa `datos/BASE_DATOS_PROYECTO_FINAL_demo.xlsx` (importándola en Google Sheets), que tiene las tres pestañas con sus encabezados.
- **Consultas:** empieza vacía (solo encabezados). La completa el workflow. En la copia demostrativa contiene registros de las pruebas (CON-170 a CON-230): borrarlos y dejar solo los encabezados antes de usarla.
- **Formato de los valores:** fechas como texto `AAAA-MM-DD`, legajos `LEG-NNNN` y `activo` con los valores `SÍ` o `NO`.

## 4. Credenciales en n8n

Crear estas credenciales. Los JSON saneados las referencian solo por tipo y nombre.

| Nombre sugerido | Tipo en n8n | Configuración |
|---|---|---|
| Google Gemini API | Google Gemini (PaLM) API | Clave de API de Gemini |
| Supabase API | Supabase API | URL del proyecto y clave `service_role` |
| Google Sheets OAuth2 | Google Sheets OAuth2 API | Cuenta con acceso a la planilla |
| Gmail OAuth2 | Gmail OAuth2 | Opcional; cuenta emisora de pruebas |
| Header Auth - Webhook WF-02 | Header Auth | Nombre `X-Cordal-Token` y un valor secreto propio, generado de forma aleatoria |

No escribir claves ni tokens en los workflows, en el código ni en documentos.

## 5. Importación de los workflows

Importar desde `workflows/` (menú "Import from File"), en este orden, y anotar el identificador que n8n asigna a cada uno:

1. `WF-03_procesamiento.json`
2. `WF-04_monitoreo_errores.json`
3. `WF-02_recepcion.json`
4. `WF-01_ingesta_documental_rag.json`

No publicar ningún workflow hasta terminar la configuración.

## 6. Configuración después de importar

| Workflow | Nodo o ajuste | Acción |
|---|---|---|
| Todos | Nodos con credenciales | Asignar la credencial correspondiente (sección 4) |
| WF-02 | "Registrar bloqueo" | Reemplazar `REEMPLAZAR_ID_PLANILLA` por el identificador de la planilla (o seleccionarla de la lista) |
| WF-02 | "Procesar en WF-03" | Seleccionar el workflow WF-03 importado (reemplaza `REEMPLAZARIDWF03`) |
| WF-02 | "Reportar incidente a WF-04" | Seleccionar el workflow WF-04 importado (reemplaza `REEMPLAZARIDWF04`) |
| WF-03 | "Buscar empleado", "Leer solicitudes propias", "Buscar consulta registrada", "Registrar consulta", "Actualizar estado del aviso" | Reemplazar `REEMPLAZAR_ID_PLANILLA` por el identificador de la planilla |
| WF-04 | Settings → "This workflow can be called by" | Reemplazar `REEMPLAZARIDWF02` por el identificador del WF-02 importado |
| WF-04 | Código de "Normalizar incidente controlado" | Reemplazar el literal `REEMPLAZARIDWF02` por el identificador del WF-02 importado. Es el valor que se informa como identificador de WF-02 en las alertas. Si no se reemplaza, el flujo funciona, pero la alerta muestra el marcador |
| WF-03 (opcional) | Settings → "This workflow can be called by" | Restringir a WF-02 (pendiente de producción) |

Los marcadores `REEMPLAZARID…` no son identificadores reales: se introdujeron en el saneamiento (ver `docs/INFORME_SANEAMIENTO_WORKFLOWS.md`).

## 7. Carga de la base documental

1. Abrir WF-01 y ejecutar el formulario "Formulario de carga" (requiere sesión en n8n).
2. Cargar los cinco archivos de `procedimientos/`.
3. Resultado esperado: 5 documentos válidos y 70 fragmentos insertados (14 por procedimiento).
4. Verificar en Supabase con las consultas del script.

Un procedimiento ya cargado se rechaza sin modificar la base. Para recargarlo hay que borrar antes sus fragmentos en Supabase.

## 8. Uso

- **Chat:** en WF-02, usar el chat del editor o, una vez publicado, el Hosted Chat (con n8n User Auth). Escribir el legajo al inicio, por ejemplo: `LEG-0007 ¿Con cuánta anticipación tengo que pedir 5 días de vacaciones?`.
- **Webhook:** `POST /webhook/cordal-consultas` (o `/webhook-test/...` en modo de prueba), con el encabezado `X-Cordal-Token` y el cuerpo JSON `{"mensaje": "LEG-0007 ..."}`. La respuesta es un JSON de 7 campos.
- **Avisos y alertas:** para habilitar Gmail, configurar la casilla en "Configuración del aviso" (WF-03) y "Configuración de alerta" (WF-04). Hay que completar la lista de destinatarios autorizados y el destinatario, y poner el indicador de envío en `true`. No usar direcciones `@cordal.example`.
- **Workflow de errores:** para que WF-04 reciba las fallas no previstas:
  1. Publicar WF-04.
  2. Configurarlo como "Error workflow" en WF-02 y WF-03.
  3. Agregar WF-03 a la lista de workflows que pueden llamarlo.

Recomendaciones:

- Ejecutar las consultas de a una: el nivel gratuito de Gemini puede rechazar varias consultas simultáneas por límite de solicitudes.
- Al probar el webhook, iniciar la escucha con "Execute workflow" sobre el flujo completo y no desde el panel del nodo, para no devolver los encabezados recibidos.

## 9. Limitaciones de la reproducción

- **Importación:** los JSON se validaron estructuralmente, pero no se importaron ni ejecutaron en otra instancia.
- **Script de Supabase:** reproduce la estructura verificada, pero no es el script original ni se ejecutó sobre un proyecto nuevo.
- **Versión de n8n:** una versión anterior podría no reconocer algunas versiones de nodos ni los grupos de nodos.
- **Modelo:** la disponibilidad de `gemini-3.5-flash-lite` depende de la cuenta de Gemini.
