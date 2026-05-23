# ADR-001 - Expansion Red A para 112 equipos externos

## Estado

Aceptada

## Contexto

La OMI requiere 256 equipos simultaneos para primaria en Dia 2. El campus cuenta con 144 equipos fijos en cinco salas de computo, por lo que faltan 112 equipos. El calendario oficial no permite dividir categorias en olas ni agregar turnos.

## Decision

Usar Sala Borrego como Plan A para instalar los 112 equipos externos y conectarlos a Red A / VLAN 10. Usar Auditorio Escuela de Ingenieria como Plan B contingente si Sala Borrego no cumple con energia, mobiliario, conectividad o disponibilidad.

## Alternativas consideradas

| Alternativa | Ventaja | Desventaja |
| --- | --- | --- |
| Sala Borrego | Aforo aproximado suficiente para 112 equipos y separacion de roles operativos. | Requiere validar energia, mesas y cableado temporal. |
| Auditorio Escuela de Ingenieria | Aforo aproximado suficiente y funciona como contingencia. | El mobiliario tipo auditorio puede dificultar montar computadoras. |
| Dividir primaria en olas | Reduce necesidad de equipos simultaneos. | Viola la regla actual de no dividir categorias. |

## Consecuencias

Positivas:

- Mantiene calendario oficial y primaria completa.
- Permite llegar a 256 equipos en Red A.
- Conserva roles no competidores separados en sus espacios.

Negativas o riesgos:

- Requiere 112 equipos externos.
- Requiere switches, energia y cableado temporal adicional.
- Depende de validacion fisica final de Sala Borrego.

## Impacto en documentos

| Documento | Cambio |
| --- | --- |
| `00-fuente-de-verdad.md` | Registra Plan A con Sala Borrego y Plan B con Auditorio Ingenieria. |
| `01-arquitectura-red.md` | Declara Sala Borrego como extension Red A / VLAN 10. |
| `02-eventos-escenarios.md` | Sin cambio requerido. |
| `03-validacion-operacion.md` | Agrega validaciones para expansion Red A. |
| `04-espacios-fisicos.md` | Documenta Plan A seleccionado y Plan B contingente. |

## Fecha

2026-05-23
