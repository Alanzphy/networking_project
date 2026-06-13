# F003 - Red A hibrida con WiFi optimizado

## Estado

Integrada

## Problema

La propuesta anterior asumia que la Red A de competidores debia ser principalmente cableada y que la expansion fisica requeria instalar nodos o cableado adicional para todos los equipos faltantes. El levantamiento fisico actualizado muestra que hay APs disponibles en los espacios clave y que las laptops rentadas pueden operar por WiFi, por lo que una instalacion masiva de nodos Cat 6a no optimiza recursos.

## Objetivo

Definir una Red A hibrida que use cable donde ya existe infraestructura suficiente y WiFi donde conviene evitar instalaciones adicionales, manteniendo VLAN 10, la subred `/23`, la capacidad de 264 competidores y las reglas de seguridad existentes.

## Alcance

Incluye:

- Uso de `OMI-Competidores` en VLAN 10 para equipos de competencia por WiFi.
- Uso de `OMI-Jueces` en VLAN 20 para jueces por WiFi si se requiere.
- Aprovechamiento de APs existentes con capacidad aproximada de 150 equipos por AP.
- Reduccion de nodos Cat 6a nuevos a 0 como base de la propuesta economica.
- Validacion de DHCP, suficiencia de APs y bloqueo entre VLANs.

No incluye:

- Compra o instalacion definitiva de APs nuevos.
- Diseno de radiofrecuencia detallado.
- Certificacion formal de cableado.
- Cambio de calendario.

## Requerimientos

| ID | Requerimiento | Prioridad |
| --- | --- | --- |
| RF-F003-01 | Los competidores deben poder conectarse a Red A por cable o por `OMI-Competidores` segun el espacio. | Alta |
| RF-F003-02 | Sala Borrego debe soportar 120 laptops rentadas por WiFi en Red A / VLAN 10. | Alta |
| RF-F003-03 | Los jueces deben poder usar Red T por cable o por `OMI-Jueces` sin perder acceso a servidor e impresoras. | Media |
| RNF-F003-01 | La Red A hibrida debe conservar las mismas ACLs que la Red A cableada. | Alta |
| RNF-F003-02 | La cantidad de APs disponibles debe revisarse para definir si hacen falta APs adicionales. | Alta |

## Restricciones

- La Red A sigue usando VLAN 10 y `172.23.8.0/23`.
- `OMI-Competidores` no debe compartir VLAN con invitados, reporteros, entrenadores, jueces o camaras.
- `OMI-Jueces` no debe ser accesible para competidores ni usuarios generales.
- Las camaras adicionales/rentadas continuan en `OMI-Camaras` / VLAN 70.
- No se agregan nodos Cat 6a nuevos como base; solo se considerarian si una necesidad fisica puntual lo vuelve necesario.

## Escenarios

### Escenario 1: primaria usa Sala Borrego por WiFi

Condiciones:

- Sala Borrego tiene 3 APs disponibles.
- Cada AP tiene capacidad aproximada de 150 equipos.
- Hay 120 laptops rentadas con WiFi funcional.

Accion:

- El equipo tecnico configura `OMI-Competidores` en VLAN 10 y conecta las laptops rentadas desde Sala Borrego.

Resultado esperado:

- Las 120 laptops reciben IP de Red A.
- Las laptops acceden al servidor de concurso.
- Las laptops quedan bloqueadas hacia jueces, impresoras, invitados, reporteros, entrenadores y camaras.

### Escenario 2: competidor usa laboratorio hibrido

Condiciones:

- El laboratorio tiene equipos cableados y equipos con WiFi.
- El AP del espacio esta disponible para equipos que usen WiFi.

Accion:

- Los equipos con puerto disponible usan Ethernet y los equipos portatiles o sin punto asignado usan `OMI-Competidores`.

Resultado esperado:

- Todos reciben IP de Red A.
- El tipo de conexion no cambia permisos ni prioridad logica.

### Escenario 3: juez usa Red T por WiFi

Condiciones:

- Domo Life tiene 3 APs disponibles.
- Los jueces usan laptops rentadas o equipo con WiFi.

Accion:

- Los jueces se conectan a `OMI-Jueces`, mapeado a VLAN 20.

Resultado esperado:

- Los jueces acceden al servidor local y a impresoras.
- Competidores e invitados no alcanzan Red T.

## Impacto en documentos base

| Documento | Cambio requerido |
| --- | --- |
| `00-fuente-de-verdad.md` | Actualizar RF-01, tabla de redes, supuestos y levantamiento fisico. |
| `01-arquitectura-red.md` | Agregar SSIDs `OMI-Competidores` y `OMI-Jueces`; ajustar infraestructura fisica. |
| `02-eventos-escenarios.md` | Cambiar competidores y jueces a conexion hibrida. |
| `03-validacion-operacion.md` | Agregar pruebas WiFi para Red A y Red T. |
| `04-espacios-fisicos.md` | Registrar APs, tomas, puertos y camaras observadas. |

## Validacion

- Probar DHCP Red A por cable y por `OMI-Competidores`.
- Probar 120 laptops en Sala Borrego con IP de VLAN 10.
- Confirmar que Sala Borrego conserva 3 APs disponibles para 120 laptops; agregar APs solo si falta cobertura minima o AP fisico.
- Probar `OMI-Jueces` con acceso a servidor e impresoras.
- Confirmar que Red A mantiene bloqueos hacia redes no autorizadas.

## Decisiones pendientes

- Levantamiento fisico final de Sala Menlo y Auditorio Escuela de Ingenieria.
- Confirmacion final de tomas de corriente en 12401.

## Supuestos

- Cada AP soporta aproximadamente 150 equipos.
- Las laptops rentadas tienen WiFi funcional.
- TI puede mapear SSIDs a VLANs en los APs existentes.
- La capacidad de AP se usa para estimar si hacen falta APs adicionales.
