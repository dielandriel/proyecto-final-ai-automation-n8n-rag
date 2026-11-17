# Diseño de la base de datos del Proyecto Final

- **Documento:** DISENO_BASE_DATOS_PROYECTO_FINAL
- **Versión:** 1.1
- **Fecha:** 16/11/2026
- **Estado:** diseño aprobado; planilla implementada con datos ficticios
- **Soporte:** una única planilla de Google Sheets con tres pestañas
- **Empresa:** Cordal Tecnología S.A. (empresa ficticia, prototipo académico)

Este documento define la estructura de datos del Asistente Inteligente de Procesos Internos. Todos los datos son ficticios. La planilla contiene 20 empleados y 15 solicitudes (3 por cada uno de los cinco procedimientos).

**Cambios de la versión 1.1 (16/11/2026):** se agrega la columna 16 de Consultas, `estado_aviso`, con sus valores permitidos y reglas (secciones 1, 4, 6.5, 7.6 y 8).

## 1. Resumen de las pestañas

| Pestaña | Contenido | Columnas | Filas previstas | Acceso del sistema |
|---|---|---|---|---|
| Empleados | Datos maestros de los empleados ficticios | 11 | 20 | Solo lectura |
| Solicitudes | Trámites de los cinco procedimientos | 12 | 15 | Solo lectura |
| Consultas | Registro de consultas atendidas y derivaciones | 16 | Crece con el uso | Agregar filas; el workflow actualiza solo `estado_aviso` |

## 2. Pestaña Empleados (11 columnas)

No incluye documento de identidad, domicilio, teléfono ni datos salariales.

| # | Columna | Tipo | Finalidad |
|---|---|---|---|
| 1 | `legajo` | Texto (`LEG-0001`) | Clave única del empleado |
| 2 | `nombre` | Texto | Personalizar la respuesta |
| 3 | `apellido` | Texto | Identificación |
| 4 | `correo` | Texto (`@cordal.example`) | Dato ilustrativo; no se usa como destinatario real |
| 5 | `area` | Lista | Contexto para clasificar y derivar |
| 6 | `puesto` | Texto | Contexto del rol |
| 7 | `sede` | Lista | Aplica reglas como el visto bueno del líder de proyecto |
| 8 | `legajo_jefe` | Texto (`LEG-0001`) | Indica quién aprueba; vacío en el nivel más alto |
| 9 | `fecha_ingreso` | Fecha (AAAA-MM-DD) | Detectar ingreso reciente |
| 10 | `saldo_vacaciones` | Número entero | Saldo registrado que el agente puede informar |
| 11 | `activo` | Lista (SÍ, NO) | Rechazar consultas de legajos inactivos |

Ejemplos aprobados:

| legajo | nombre | apellido | correo | area | puesto | sede | legajo_jefe | fecha_ingreso | saldo_vacaciones | activo |
|---|---|---|---|---|---|---|---|---|---|---|
| LEG-0001 | Marta | Quiroga | marta.quiroga@cordal.example | Operaciones | Gerente de Operaciones | Sede Central | | 2015-03-02 | 18 | SÍ |
| LEG-0004 | Julián | Ferreyra | julian.ferreyra@cordal.example | Operaciones | Líder de Proyecto | Sede Central | LEG-0001 | 2018-08-13 | 12 | SÍ |
| LEG-0007 | Carla | Benítez | carla.benitez@cordal.example | Administración y Finanzas | Analista de Rendiciones | Sede Central | LEG-0002 | 2020-02-03 | 9 | SÍ |
| LEG-0009 | Tomás | Aguirre | tomas.aguirre@cordal.example | Operaciones | Técnico de Campo | Sitio de cliente | LEG-0004 | 2022-06-20 | 14 | SÍ |
| LEG-0019 | Lucía | Paredes | lucia.paredes@cordal.example | Sistemas | Analista de Soporte | Remoto | LEG-0003 | 2026-09-07 | 0 | SÍ |

## 3. Pestaña Solicitudes (12 columnas)

Representa los trámites que en los procedimientos se gestionan en el Portal del Empleado, que es un sistema ficticio de referencia.

| # | Columna | Tipo | Finalidad |
|---|---|---|---|
| 1 | `id_solicitud` | Texto | Clave única, con el formato de cada procedimiento |
| 2 | `procedimiento` | Lista | Vincula con el documento del RAG |
| 3 | `legajo` | Texto (`LEG-0001`) | Vincula con Empleados |
| 4 | `fecha_carga` | Fecha (AAAA-MM-DD) | Calcular plazos |
| 5 | `estado` | Lista según el procedimiento | Estado que el agente informa |
| 6 | `fecha_estado` | Fecha (AAAA-MM-DD) | Último cambio de estado |
| 7 | `fecha_inicio` | Fecha (AAAA-MM-DD) | Inicio de vacaciones, ausencia, viaje o préstamo |
| 8 | `fecha_fin` | Fecha (AAAA-MM-DD) | Fin del período; puede quedar vacío |
| 9 | `prioridad` | Lista | Solo tickets de soporte informático |
| 10 | `importe` | Número | Solo rendiciones de gastos |
| 11 | `centro_costo` | Texto | Solo rendiciones y herramientas |
| 12 | `detalle` | Texto breve | Descripción no sensible; en licencias médicas siempre "Aviso de ausencia" |

Ejemplos aprobados:

| id_solicitud | procedimiento | legajo | fecha_carga | estado | fecha_estado | fecha_inicio | fecha_fin | prioridad | importe | centro_costo | detalle |
|---|---|---|---|---|---|---|---|---|---|---|---|
| VAC-2026-0001 | PRO-RH-001 | LEG-0007 | 2026-11-02 | Aprobada | 2026-11-06 | 2026-11-30 | 2026-12-04 | | | | 5 días hábiles |
| LIC-2026-0001 | PRO-RH-002 | LEG-0012 | 2026-11-10 | En revisión de Salud Ocupacional | 2026-11-11 | 2026-11-10 | | | | | Aviso de ausencia |
| RG-2026-0001 | PRO-AF-001 | LEG-0004 | 2026-11-05 | Observada | 2026-11-12 | 2026-11-03 | 2026-11-04 | | 84500 | CC-PRJ-014 | Viaje a sitio de cliente |
| TKT-2026-0001 | PRO-IT-001 | LEG-0015 | 2026-11-12 | En curso | 2026-11-12 | | | Alta | | | No conecta la VPN |
| HM-2026-0001 | PRO-LG-001 | LEG-0009 | 2026-11-04 | Entregado | 2026-11-11 | 2026-11-11 | 2026-11-19 | | | CC-PRJ-014 | Instrumental de medición |

## 4. Pestaña Consultas (16 columnas)

Registro que escribe el sistema por cada consulta recibida. Es la salida funcional del prototipo y la evidencia de las decisiones del AI Agent.

| # | Columna | Tipo | Finalidad |
|---|---|---|---|
| 1 | `id_consulta` | Texto | Clave única generada por el workflow (ver sección 7.1) |
| 2 | `fecha_hora` | Fecha y hora (AAAA-MM-DD HH:MM) | Trazabilidad |
| 3 | `origen` | Lista | Canal de entrada de la consulta |
| 4 | `legajo` | Texto (`LEG-0001`) | Identidad simulada de prueba; vincula con Empleados |
| 5 | `procedimiento` | Lista | Procedimiento al que corresponde la consulta |
| 6 | `categoria` | Lista | Tipo de consulta |
| 7 | `decision` | Lista | Decisión del AI Agent; vacía si el agente no intervino |
| 8 | `estado_procesamiento` | Lista | Resultado técnico de la ejecución |
| 9 | `codigo_error` | Lista de códigos controlados | Solo cuando el estado es ERROR |
| 10 | `area_destino` | Lista | Área recomendada para la atención humana |
| 11 | `urgencia` | Lista | Prioriza la atención humana |
| 12 | `resumen` | Texto breve | Resumen no sensible; nunca el mensaje original |
| 13 | `fuentes` | Texto | Secciones recuperadas por el RAG |
| 14 | `id_solicitud` | Texto | Solicitud relacionada, si existe |
| 15 | `estado_derivacion` | Lista | Seguimiento de la derivación; lo actualiza una persona |
| 16 | `estado_aviso` | Lista | Estado del correo de aviso de la derivación; lo escribe el workflow (ver sección 7.6) |

Ejemplos (los identificadores de ejecución son ilustrativos):

| id_consulta | fecha_hora | origen | legajo | procedimiento | categoria | decision | estado_procesamiento | codigo_error | area_destino | urgencia | resumen | fuentes | id_solicitud | estado_derivacion | estado_aviso |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| CON-1201 | 2026-11-13 10:42 | CHAT | LEG-0009 | PRO-LG-001 | Informativa | RESPONDER | COMPLETADO | | | Normal | Plazo de devolución de instrumental | PRO-LG-001 §7 | HM-2026-0001 | | NO_APLICA |
| CON-1202 | 2026-11-13 11:05 | WEBHOOK | LEG-0007 | SIN_CLASIFICAR | SIN_CLASIFICAR | SOLICITAR_ACLARACION | COMPLETADO | | | Normal | Pide "días" sin indicar si son vacaciones o licencia | | | | NO_APLICA |
| CON-1203 | 2026-11-13 14:20 | CHAT | LEG-0004 | PRO-AF-001 | Requiere decisión humana | DERIVAR | COMPLETADO | | Administración y Finanzas | Normal | Pide excepción a un tope | PRO-AF-001 §10 | RG-2026-0001 | Pendiente | PENDIENTE |
| CON-1204 | 2026-11-13 15:02 | CHAT | LEG-0012 | PRO-RH-002 | Dato sensible | | BLOQUEADO | | Salud Ocupacional | Normal | [contenido no registrado] | | | Pendiente | NO_APLICA |
| CON-1205 | 2026-11-13 16:31 | WEBHOOK | LEG-0015 | SIN_CLASIFICAR | SIN_CLASIFICAR | | ERROR | ERR_LLM_NO_DISPONIBLE | | Normal | [no disponible] | | | | NO_APLICA |

## 5. Relaciones entre las tres pestañas

- `Empleados.legajo` es la clave principal de los empleados.
- `Empleados.legajo_jefe` referencia a otra fila de Empleados e indica quién aprueba.
- `Solicitudes.legajo` referencia a `Empleados.legajo`.
- `Consultas.legajo` referencia a `Empleados.legajo`.
- `Consultas.id_solicitud` referencia a `Solicitudes.id_solicitud` cuando la consulta es sobre un trámite.
- `Solicitudes.procedimiento` y `Consultas.procedimiento` enlazan con los cinco procedimientos de la base documental del RAG.

## 6. Estados y valores permitidos

### 6.1 Identificadores

| Dato | Formato |
|---|---|
| Legajo | `LEG-NNNN` |
| Solicitud de vacaciones | `VAC-AAAA-NNNN` |
| Licencia médica | `LIC-AAAA-NNNN` |
| Rendición de gastos | `RG-AAAA-NNNN` |
| Ticket de soporte informático | `TKT-AAAA-NNNN` |
| Solicitud de herramientas y materiales | `HM-AAAA-NNNN` |
| Consulta | Ver sección 7.1 |

### 6.2 Procedimientos

`PRO-RH-001`, `PRO-RH-002`, `PRO-AF-001`, `PRO-IT-001`, `PRO-LG-001`.

En la pestaña Consultas se admiten además `Fuera de alcance` y `SIN_CLASIFICAR`.

### 6.3 Estados de las solicitudes

Solo se usan los estados definidos en los procedimientos aprobados.

| Procedimiento | Estados permitidos |
|---|---|
| PRO-RH-001 (VAC) | Pendiente de jefe directo, Aprobada |
| PRO-RH-002 (LIC) | Aviso registrado, En revisión de Salud Ocupacional, Justificada, Observada |
| PRO-AF-001 (RG) | Pendiente de jefe directo, Aprobada, Observada, Rechazada |
| PRO-IT-001 (TKT) | Abierto, En curso, Esperando respuesta del usuario, Resuelto |
| PRO-LG-001 (HM) | Pendiente de aprobación, Listo para retirar, Sin stock, Entregado, Devuelto, Devuelto con observaciones |

Prioridad de tickets: Crítica, Alta, Media, Baja.

### 6.4 Valores de la pestaña Empleados

- `sede`: Sede Central, Sitio de cliente, Remoto.
- `activo`: SÍ, NO.

### 6.5 Valores de la pestaña Consultas

| Columna | Valores permitidos |
|---|---|
| `origen` | CHAT, WEBHOOK |
| `categoria` | Informativa, Estado de solicitud, Requiere decisión humana, Incidente de seguridad, Dato sensible, Fuera de alcance, SIN_CLASIFICAR |
| `decision` | RESPONDER, SOLICITAR_ACLARACION, DERIVAR, o vacío |
| `estado_procesamiento` | COMPLETADO, BLOQUEADO, ERROR |
| `codigo_error` | ERR_LLM_NO_DISPONIBLE, ERR_LLM_TIMEOUT, ERR_LLM_SALIDA_INVALIDA, ERR_VECTOR_STORE, ERR_SHEETS, o vacío |
| `area_destino` | Recursos Humanos, Salud Ocupacional, Administración y Finanzas, Mesa de Ayuda IT, Seguridad Informática, Pañol y Logística, o vacío |
| `urgencia` | Normal, Alta |
| `estado_derivacion` | Pendiente, Tomada, Cerrada, o vacío |
| `estado_aviso` | NO_APLICA, PENDIENTE, ENVIADO, ERROR |

## 7. Decisiones definitivas de diseño

### 7.1 Generación del identificador de consulta

- El identificador se genera a partir del ID de ejecución de n8n: `CON-<ID de ejecución>`. Ese ID es único por ejecución, por lo que dos ejecuciones simultáneas no pueden producir el mismo identificador.
- Si una misma ejecución procesa varias consultas, se agrega un identificador único adicional por consulta: `CON-<ID de ejecución>-<identificador único>`.
- Para registros de prueba cargados fuera del workflow puede utilizarse un UUID: `CON-<UUID>`.
- El identificador nunca se calcula leyendo la última fila de la planilla.

### 7.2 Consultas sin clasificar

- `procedimiento` y `categoria` llevan el valor `SIN_CLASIFICAR` cuando no se pudo determinar su clasificación. No quedan vacíos.
- `decision` queda vacía cuando el AI Agent no intervino, por ejemplo ante un bloqueo previo al modelo o un error técnico anterior a su ejecución.

### 7.3 Separación entre decisión y resultado técnico

- `decision` registra únicamente lo que decidió el AI Agent: RESPONDER, SOLICITAR_ACLARACION o DERIVAR.
- `estado_procesamiento` registra el resultado técnico: COMPLETADO, BLOQUEADO o ERROR.
- La decisión del agente es una clasificación y una recomendación de ruta de atención. No es una aprobación ni una decisión oficial de las áreas responsables.

### 7.4 Errores técnicos

- Las fallas se registran con un código controlado en `codigo_error`, sin el mensaje original del empleado ni el texto del error.
- Ante un error técnico previo a la clasificación no se inventa un área de destino: `area_destino` queda vacía, `procedimiento` y `categoria` quedan en `SIN_CLASIFICAR` y `resumen` lleva el texto fijo "[no disponible]".
- `estado_derivacion` solo se completa cuando existe un `area_destino`. Los errores sin área se identifican filtrando `estado_procesamiento` = ERROR.

### 7.5 Bloqueo previo al modelo

- Cuando la validación previa detecta posible información clínica, el contenido no se envía al modelo y `resumen` lleva el texto fijo "[contenido no registrado]".
- En ese caso, la clasificación (`PRO-RH-002`, "Dato sensible", Salud Ocupacional) la asigna la regla de validación, no el AI Agent; por eso `decision` queda vacía y `estado_procesamiento` es BLOQUEADO.

### 7.6 Estado del aviso de derivación

| Valor | Significado |
|---|---|
| NO_APLICA | La consulta no requiere aviso |
| PENDIENTE | Derivación que requiere aviso y todavía no se confirmó su envío |
| ENVIADO | Gmail aceptó el mensaje y devolvió un identificador de mensaje |
| ERROR | Gmail rechazó el envío con un error confirmado |

- Al registrar la consulta, el workflow escribe PENDIENTE solo si `decision` = DERIVAR, `estado_procesamiento` = COMPLETADO y `estado_derivacion` = Pendiente. En cualquier otro caso escribe NO_APLICA.
- ENVIADO y ERROR los escribe el workflow después del envío, actualizando solo esta columna de la fila con el mismo `id_consulta`. El AI Agent no escribe ni decide este valor.
- Si el resultado de Gmail es incierto (tiempo de espera, corte de conexión, error del servidor o respuesta sin identificador), el valor queda en PENDIENTE y el caso se marca para verificación manual. No se asume que el correo no se envió.
- `estado_aviso` y `estado_derivacion` son independientes. ENVIADO no significa entregado, leído ni atendido; la atención del caso se registra solo en `estado_derivacion`.
- No hay reintentos automáticos ni reenvíos de filas en PENDIENTE. La recuperación es manual: primero se revisan el resultado del envío y la carpeta de enviados, y solo después se decide si corresponde reenviar.

## 8. Qué puede consultar y registrar el sistema

- **Consultar (solo lectura):** los datos del empleado identificado por el legajo de la consulta y sus propias solicitudes. No accede a datos de otros empleados.
- **Registrar:** filas nuevas en la pestaña Consultas y, después de un envío, el valor de `estado_aviso` de esa misma fila. Lo hace el workflow, no el AI Agent.
- **No puede:** modificar Empleados ni Solicitudes, cambiar estados de trámites ni actualizar `estado_derivacion`.

## 9. Reglas de seguridad y privacidad

1. Todos los datos son ficticios. No se cargan personas, clientes ni documentos reales.
2. El `legajo` utilizado en las pruebas es una identidad simulada y no constituye autenticación. En una implementación productiva, el acceso a solicitudes individuales deberá requerir autenticación real del empleado.
3. La planilla no contiene diagnósticos, certificados médicos, datos clínicos, contraseñas, documentos de identidad, domicilios, teléfonos ni datos salariales.
4. Las licencias médicas se registran sin motivo: el campo `detalle` es siempre "Aviso de ausencia".
5. La pestaña Consultas no almacena el mensaje original del empleado; solo un resumen breve no sensible o un texto fijo.
6. Los correos `@cordal.example` son ilustrativos y no funcionan como destinatarios reales. Los avisos del prototipo se envían desde una cuenta de pruebas autorizada.
7. El Portal del Empleado es un sistema ficticio de referencia; la pestaña Solicitudes lo representa en el prototipo y el agente no ejecuta trámites.
8. La planilla no debe incluir comentarios de celda, notas de trabajo ni credenciales.
