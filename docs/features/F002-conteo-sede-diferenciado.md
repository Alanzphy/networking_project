# F002 - Conteo diferenciado para estado sede

## Estado

Integrada

## Problema

El conteo anterior usaba 32 estados parejos con el mismo numero de competidores por categoria. El estado sede tiene un contingente mayor por ser anfitrion, por lo que el modelo de 32 estados iguales subestimaba la capacidad real requerida.

## Objetivo

Actualizar el conteo oficial a 31 estados regulares con contingente normal y 1 estado sede con contingente doble, manteniendo 32 estados totales.

## Conteo anterior vs nuevo

| Categoria | Modelo anterior (32 iguales) | Nuevo modelo (31 + sede) |
| --- | ---: | ---: |
| Preparatoria | 32 x 4 = 128 | 31 x 4 + 8 = 132 |
| Secundaria | 32 x 6 = 192 | 31 x 6 + 12 = 198 |
| Primaria | 32 x 8 = 256 | 31 x 8 + 16 = 264 |
| Total | 576 | 594 |
| Max simultaneo | 256 | 264 |
| Equipos externos | 112 | 120 |

## Alcance

Incluye:

- Actualizacion del conteo oficial en la fuente de verdad.
- Actualizacion del maximo simultaneo de 256 a 264.
- Actualizacion de equipos externos de 112 a 120.
- Propagacion del cambio a arquitectura, escenarios, validacion y espacios fisicos.

No incluye:

- Cambio de calendario, olas o formato de competencia.
- Cambio de subredes (Red A con /23 sigue siendo suficiente).
- Cambio de VLAN o SSIDs.

## Requerimientos

| ID | Requerimiento | Prioridad |
| --- | --- | --- |
| RF-F002-01 | El conteo debe registrar 31 estados con contingente normal y 1 estado sede con contingente doble. | Alta |
| RF-F002-02 | Red A debe soportar al menos 264 equipos simultaneos. | Alta |
| RF-F002-03 | La expansion fisica debe cubrir 120 equipos externos. | Alta |
| RNF-F002-01 | La subred /23 de Red A sigue siendo valida (510 hosts utiles >= 264). | Alta |

## Restricciones

- El total de estados permanece en 32.
- El calendario de dos dias no cambia.
- No hay olas dentro de una categoria.
- Los equipos externos siguen en Red A / VLAN 10.

## Impacto en documentos base

| Documento | Cambio requerido |
| --- | --- |
| `00-fuente-de-verdad.md` | Actualizar tabla de conteo, calendario, decisiones y criterios de aceptacion. |
| `01-arquitectura-red.md` | Actualizar objetivo (256->264), criterios de aceptacion y referencias a 112 equipos externos. |
| `02-eventos-escenarios.md` | Actualizar tabla de usuarios y escenarios con nuevas cantidades. |
| `03-validacion-operacion.md` | Actualizar checklist, pruebas y validacion por turno. |
| `04-espacios-fisicos.md` | Actualizar tabla de capacidad, verificacion y justificacion de planes. |
| `AGENTS.md` | Actualizar decisiones vigentes. |

## Validacion

- Confirmar `144 + 120 = 264` equipos en Red A.
- Confirmar que el scope DHCP de Red A soporta mas de 264 clientes.
- Confirmar que /23 (510 hosts utiles) cubre 264 + reservas.
- Probar DHCP Red A y acceso al servidor desde equipos externos en Sala Borrego.
