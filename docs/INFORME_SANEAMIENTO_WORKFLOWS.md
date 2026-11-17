# Informe de auditoría y saneamiento de los workflows

- **Documento:** INFORME_SANEAMIENTO_WORKFLOWS
- **Fecha:** 17/11/2026
- **Alcance:** exportación, auditoría de seguridad y saneamiento de los cuatro workflows de n8n incluidos en este repositorio.

Este informe no reproduce valores secretos ni identificadores de la instancia original.

## 1. Archivos

| Workflow | Archivo saneado | Nodos (incluye Sticky Notes) | Sticky Notes | Conexiones |
|---|---|---|---|---|
| WF-01 Ingesta documental RAG | `workflows/WF-01_ingesta_documental_rag.json` | 17 | 3 | 13 |
| WF-02 Recepción | `workflows/WF-02_recepcion.json` | 19 | 4 | 16 |
| WF-03 Procesamiento | `workflows/WF-03_procesamiento.json` | 37 | 6 | 36 |
| WF-04 Monitoreo de errores | `workflows/WF-04_monitoreo_errores.json` | 17 | 4 | 15 |

**Exportación**

- Origen: versiones guardadas vigentes en n8n Cloud el 17/11/2026, después de la preparación de la etapa B.
- Método: opción "Export JSON" del menú del workflow en el editor de n8n.
- Los exports originales se conservan como copias privadas de trabajo, fuera del repositorio. No se incluyen en él de ninguna forma.

## 2. Auditoría de seguridad

Se recorrieron de forma estructurada todos los campos de cada export:

- Raíz.
- Metadatos.
- Configuración.
- Parámetros de nodos, incluidos expresiones, código JavaScript y system prompt.
- Credenciales.
- Grupos.
- Datos fijados.

| Búsqueda | Resultado en los exports originales |
|---|---|
| Claves de API, contraseñas, tokens, encabezados de autenticación | No se encontraron valores. El encabezado `X-Cordal-Token` aparece solo como nombre, en Sticky Notes y en un control de contenido; su valor reside en la credencial de n8n y no se exporta |
| Patrones de claves conocidas (Google, OpenAI, GitHub, Slack, JWT) | Ninguno |
| Direcciones de correo personales | Ninguna. Los destinatarios de WF-03 se vaciaron en la etapa B |
| Direcciones `@cordal.example` | Presentes solo en el system prompt y en validaciones de código. Son ficticias y se conservan |
| Referencias a credenciales | 15 referencias a 5 credenciales, con identificador interno y nombre |
| URL de la instancia n8n Cloud, de Google Sheets o de Supabase | Ninguna |
| Identificador de la instancia (`meta.instanceId`) | Presente en los cuatro |
| Identificadores del workflow (`id`, `versionId`) | Presentes en los cuatro |
| Referencias a otros workflows | WF-02 → WF-03 y WF-04 (selector de workflow). WF-04: lista de workflows autorizados y un literal en el código de "Normalizar incidente controlado" |
| Identificador de la planilla de Google Sheets | 6 referencias (WF-02: 1, WF-03: 5) |
| Proyecto de Supabase | No figura en los workflows; está en la credencial. La tabla `documents` y la función `match_documents` se conservan |
| Identificadores de webhook (`webhookId`) | 5: formulario de ingesta, chat, webhook de consultas y los dos nodos de Gmail |
| Datos fijados (`pinData`) | Vacíos en los cuatro |
| Datos de ejecución | No incluidos |
| Estado de publicación (`active`) | `false` en los cuatro |
| Nombres de credenciales con referencias a cuentas o cursos | Presentes (nombres de trabajo de la cuenta de pruebas) |

**Conclusión:** no se encontró ninguna credencial ni dato sensible que impida el saneamiento o afecte el funcionamiento.

## 3. Saneamiento aplicado

Solo sobre las copias de entrega. La configuración real de los workflows en n8n no se modificó.

Se aplicaron tres tipos de cambios, distintos entre sí.

### 3.1 Saneamiento de información propia de la instancia

| Elemento | Cambio | Motivo |
|---|---|---|
| Identificador del workflow | Se quitaron `id` y `versionId` de la raíz | Son propios de la instancia; n8n asigna nuevos al importar |
| Identificador de la instancia | Se quitó `meta.instanceId` | Identifica la instancia original |
| Credenciales | Se quitó el identificador interno; se conservó el tipo y se reemplazó el nombre por uno genérico | Las credenciales no viajan con el JSON; deben crearse al importar |
| Webhooks | `webhookId` reemplazados por UUID nuevos | Evita reutilizar las rutas de la instancia original |
| Referencias entre workflows | Reemplazadas por `REEMPLAZARIDWF02`, `REEMPLAZARIDWF03` y `REEMPLAZARIDWF04` | Los identificadores cambian en cada instancia |
| Planilla de Google Sheets | Identificador reemplazado por `REEMPLAZAR_ID_PLANILLA` | Corresponde a una planilla de la cuenta personal de pruebas |

### 3.2 Eliminación de metadatos opcionales

| Elemento | Cambio | Motivo |
|---|---|---|
| `meta.aiBuilderAssisted` y `meta.builderVariant` | Se quitaron; al quedar vacío, se eliminó el bloque `meta` | Metadatos opcionales de procedencia del editor; no son necesarios para la importación ni los exige la consigna |

### 3.3 Ajuste de configuración de seguridad

| Elemento | Cambio | Motivo |
|---|---|---|
| `settings.availableInMCP` | Se fijó explícitamente en `false` en los cuatro JSON; la propiedad se conserva | Decisión de seguridad y mínimo privilegio: la exposición de workflows mediante MCP no es necesaria para el funcionamiento actual y ampliaría la superficie de acceso |

Alcance del ajuste de seguridad:

- Afecta solo a las copias de entrega. No se modificaron los workflows reales de n8n.
- La integración mediante MCP se considera una posible evolución futura (README, sección 15), no una capacidad habilitada ni probada.
- Su implementación requeriría autenticación, autorización, permisos mínimos, control de los workflows expuestos y pruebas de seguridad específicas.

Nombres genéricos de las credenciales:

| Tipo de credencial en n8n | Nombre en los JSON saneados | Usada por |
|---|---|---|
| `googlePalmApi` | Google Gemini API | WF-01, WF-03 |
| `supabaseApi` | Supabase API | WF-01, WF-03 |
| `googleSheetsOAuth2Api` | Google Sheets OAuth2 | WF-02, WF-03 |
| `gmailOAuth2` | Gmail OAuth2 | WF-03, WF-04 |
| `httpHeaderAuth` | Header Auth - Webhook WF-02 | WF-02 |

Se conservaron sin cambios:

- Tipos y versiones de nodos.
- Nombres de nodos e identificadores de nodo.
- Posiciones.
- Conexiones.
- Expresiones.
- Código JavaScript, excepto la sustitución del identificador fijo de WF-02 por el marcador `REEMPLAZARIDWF02` en el nodo "Normalizar incidente controlado" de WF-04.
- System prompt.
- Configuración del AI Agent y sus herramientas.
- Sticky Notes.
- Grupos.
- Configuración de conservación de ejecuciones.
- Rutas de los webhooks (`cordal-consultas` e `ingesta-documental-rag`).

## 4. Validaciones realizadas

Validación estructural: cada JSON saneado se comparó con su original. Para comparar los campos reemplazados, en esa comparación se restituyeron sus valores originales.

| Control | WF-01 | WF-02 | WF-03 | WF-04 |
|---|---|---|---|---|
| JSON sintácticamente válido | Sí | Sí | Sí | Sí |
| Cantidad de nodos esperada | 17 | 19 | 37 | 17 |
| Nombres, tipos y versiones de nodos idénticos | Sí | Sí | Sí | Sí |
| Identificadores de nodo idénticos | Sí | Sí | Sí | Sí |
| Parámetros y expresiones idénticos (salvo los marcadores); código idéntico salvo el identificador de WF-02 en WF-04 | Sí | Sí | Sí | Sí |
| Conexiones idénticas y apuntando a nodos existentes | Sí | Sí | Sí | Sí |
| Grupos idénticos | Sí | Sí | Sí | Sí |
| Configuración idéntica (salvo marcadores y `availableInMCP`) | Sí | Sí | Sí | Sí |
| `settings.availableInMCP` = false | Sí | Sí | Sí | Sí |
| Mismos tipos de credencial por nodo, sin identificador | Sí | Sí | Sí | Sí |
| `pinData` vacío | Sí | Sí | Sí | Sí |
| `active` = false | Sí | Sí | Sí | Sí |
| Sin bloque `meta` (sin `instanceId` ni metadatos opcionales del editor) | Sí | Sí | Sí | Sí |
| Sin identificadores de la instancia, credenciales ni cuentas | Sí | Sí | Sí | Sí |
| Correos solo `@cordal.example` | Sí (ninguno) | Sí (ninguno) | Sí | Sí (ninguno) |
| Gmail deshabilitado y sin destinatarios | No usa Gmail | No usa Gmail | Sí | Sí |

## 5. Diferencias entre los originales y los saneados

| Workflow | Diferencias introducidas |
|---|---|
| WF-01 | Sin `id`, `versionId` ni metadatos (`meta`); `availableInMCP` en `false`; 3 referencias de credencial sin identificador y con nombre genérico; 1 `webhookId` nuevo |
| WF-02 | Lo mismo en raíz, metadatos y `availableInMCP`; 2 referencias de credencial; 2 `webhookId` nuevos; referencias a WF-03 y WF-04 reemplazadas por marcadores; 1 identificador de planilla reemplazado |
| WF-03 | Lo mismo en raíz, metadatos y `availableInMCP`; 9 referencias de credencial; 1 `webhookId` nuevo; 5 identificadores de planilla reemplazados |
| WF-04 | Lo mismo en raíz, metadatos y `availableInMCP`; 1 referencia de credencial; 1 `webhookId` nuevo; identificador de WF-02 reemplazado en `settings.callerIds` y en el código de "Normalizar incidente controlado" |

No hay otras diferencias.

## 6. Configuración necesaria después de importar

1. **Importar en este orden:** WF-03, WF-04, WF-02 y WF-01. Anotar el identificador que n8n asigne a cada uno.
2. **Credenciales:** crear las cinco credenciales del cuadro de la sección 3 y asignarlas en cada nodo que las use.
3. **Planilla:**
   - Crear una copia de la planilla de datos ficticios con las pestañas Empleados, Solicitudes y Consultas.
   - Reemplazar `REEMPLAZAR_ID_PLANILLA` por su identificador en WF-02 ("Registrar bloqueo") y en WF-03 ("Buscar empleado", "Leer solicitudes propias", "Buscar consulta registrada", "Registrar consulta" y "Actualizar estado del aviso").
4. **Supabase:** crear la tabla `documents`, la función `match_documents` y la extensión pgvector con vectores de 3072 dimensiones. El detalle está en la documentación del repositorio.
5. **Referencias entre workflows:**
   - En WF-02, seleccionar WF-03 en "Procesar en WF-03" y WF-04 en "Reportar incidente a WF-04".
   - En WF-04:
     - Reemplazar `REEMPLAZARIDWF02` en la configuración "This workflow can be called by" por el identificador de WF-02.
     - Reemplazar el mismo marcador en el código de "Normalizar incidente controlado". Si no se reemplaza, la alerta muestra el marcador como identificador de WF-02 y el flujo sigue funcionando.
6. **Cargar la base documental:** ejecutar WF-01 con los cinco procedimientos.
7. **Gmail:** para habilitar avisos o alertas, cargar una casilla autorizada en "Configuración del aviso" (WF-03) y "Configuración de alerta" (WF-04), y cambiar el indicador de envío a `true`. En la entrega están deshabilitados.
8. **Producción (opcional):** las tareas pendientes de producción están en la arquitectura (sección 17), entre ellas configurar WF-04 como workflow de errores.

## 7. Limitaciones de importación y reproducibilidad

- **Alcance de la validación:**
  - La validación fue estructural: los JSON son válidos y conservan nodos, conexiones, parámetros y código.
  - No se importaron en otra instancia de n8n ni se ejecutaron desde los archivos saneados. No está comprobado que funcionen tras la importación.
- **Credenciales:** tras importar, n8n mostrará las credenciales como no configuradas hasta asignarlas.
- **Versiones de nodos:** los nodos usan las versiones disponibles en n8n Cloud en noviembre de 2026 (por ejemplo, AI Agent 3.1, Google Sheets 4.7, Gmail 2.2). Una instancia con una versión anterior de n8n podría no reconocerlas.
- **Grupos de nodos (`nodeGroups`):** son una función reciente de n8n. Una versión que no la soporte podría ignorarlos.
- **Modelo:** el AI Agent usa `models/gemini-3.5-flash-lite`. Su disponibilidad depende de la cuenta de Gemini.
- **Webhooks:** al regenerar los `webhookId`, las URL del Hosted Chat y de los webhooks serán distintas a las de la instancia original.
- **Datos:** los datos de la planilla y de Supabase no forman parte de los JSON.
