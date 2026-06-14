# ADR-005 - VLAN 99 de Administracion para controladora

## Estado

Aceptada

## Contexto

El diseno vigente habia eliminado la VLAN de gestion del evento para simplificar el plan IP y evitar exponer administracion a usuarios. Al construir la topologia fisica en Packet Tracer se requiere una controladora, por lo que hace falta un segmento tecnico propio que no pertenezca a las VLANs de usuarios.

## Decision

Agregar VLAN 99 como Administracion tecnica para controladora e infraestructura simulada.

| Segmento | VLAN | Subred | Mascara | Primera IP valida | Ultima IP valida |
| --- | ---: | --- | --- | --- | --- |
| Administracion | 99 | `172.23.11.64/28` | `255.255.255.240` | `172.23.11.65` | `172.23.11.78` |

Reservas recomendadas:

| Uso | IP |
| --- | --- |
| Gateway VLAN 99 | `172.23.11.65` |
| Controladora Packet Tracer | `172.23.11.66` |

La VLAN 99 no se publica como SSID para usuarios y debe quedar bloqueada desde Red A, Red T, Red TI, Red Repos, Red E, Red I y Red C, salvo acceso tecnico autorizado.

## Alternativas consideradas

| Alternativa | Ventaja | Desventaja |
| --- | --- | --- |
| Mantener administracion fuera del plan IP | Conserva el diseno anterior. | La controladora queda sin segmento claro en Packet Tracer. |
| Colocar controladora en Red TI | Usa una red existente. | Mezcla administracion de infraestructura con servidor e impresoras. |
| Agregar VLAN 99 | Aisla la controladora y mantiene reglas claras. | Requiere actualizar el plan IP y pruebas de bloqueo. |

## Consecuencias

Positivas:

- La controladora queda modelada de forma clara en Packet Tracer.
- La administracion tecnica queda separada de usuarios y servicios.
- El bloque `172.23.11.64/28` no se traslapa con las subredes existentes.

Riesgos:

- Si se configura mal, VLAN 99 podria quedar accesible desde redes de usuario.
- Deben actualizarse pruebas y matriz de permisos para confirmar aislamiento.

## Impacto en documentos

| Documento | Cambio |
| --- | --- |
| `00-fuente-de-verdad.md` | Agrega VLAN 99 como Administracion tecnica para controladora. |
| `01-arquitectura-red.md` | Agrega subred, reservas, reglas y criterios de aceptacion. |
| `02-eventos-escenarios.md` | Actualiza escenario de administracion tecnica. |
| `03-validacion-operacion.md` | Agrega pruebas de VLAN 99 y bloqueo desde usuarios. |
| `04-espacios-fisicos.md` | Actualiza leyenda de VLANs. |

## Fecha

2026-06-14
