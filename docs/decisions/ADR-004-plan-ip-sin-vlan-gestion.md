# ADR-004 - Plan IP sin VLAN de gestion

## Estado

Aceptada

## Contexto

El diseno anterior usaba una VLAN de gestion y otra VLAN distinta para camaras. Para el armado fisico en Packet Tracer se definio una tabla de direccionamiento nueva que elimina la VLAN de gestion del evento, mueve camaras a VLAN 70 y usa el bloque interno `172.23.8.0/21`.

## Decision

Eliminar la VLAN de gestion del diseno del evento. Las VLANs activas quedan:

| Segmento | VLAN | Subred |
| --- | ---: | --- |
| Competidores | 10 | `172.23.8.0/23` |
| Jueces | 20 | `172.23.11.32/28` |
| Servidor e impresoras | 30 | `172.23.11.48/29` |
| Reporteros | 40 | `172.23.10.192/26` |
| Entrenadores | 50 | `172.23.10.128/26` |
| Invitados | 60 | `172.23.10.0/25` |
| Camaras | 70 | `172.23.11.0/27` |

El enlace WAN RTFrontera - FCampus usa `10.32.100.0/30`, fuera del bloque interno.

## Alternativas consideradas

| Alternativa | Ventaja | Desventaja |
| --- | --- | --- |
| Mantener una VLAN de gestion separada | Conserva separacion de administracion. | No coincide con la tabla final de direccionamiento y agrega una VLAN que ya no se usara. |
| Eliminar gestion y mover camaras a VLAN 70 | Simplifica Packet Tracer y alinea el plan IP final. | La administracion tecnica debe quedar fuera de las VLANs del evento. |

## Consecuencias

Positivas:

- La tabla de direccionamiento queda alineada con el diseno fisico.
- Se reduce una VLAN del evento.
- `OMI-Camaras` sigue aislada de redes de usuario, ahora en VLAN 70.

Negativas o riesgos:

- La administracion de switches, APs y firewall ya no queda modelada como VLAN del evento.
- Las pruebas deben confirmar que ninguna red de usuario pueda administrar infraestructura.
- La visualizacion de camaras debe realizarse desde acceso tecnico autorizado fuera de las VLANs de usuario; no se reserva IP adicional en Red TI.

## Impacto en documentos

| Documento | Cambio |
| --- | --- |
| `00-fuente-de-verdad.md` | Actualiza decisiones oficiales, bloque IP, Red C y elimina la VLAN de gestion. |
| `01-arquitectura-red.md` | Reemplaza subredes, gateways, SSIDs y reglas de acceso. |
| `02-eventos-escenarios.md` | Quita el evento dependiente de la red de gestion y ajusta camaras. |
| `03-validacion-operacion.md` | Quita la red de gestion de matriz y pruebas; valida Red C / VLAN 70. |
| `04-espacios-fisicos.md` | Actualiza leyendas de VLANs y bosquejos. |

## Fecha

2026-06-12
