# ADR-001 - Expansion Red A para 120 equipos externos

## Estado

Aceptada

## Contexto

La OMI requiere 264 equipos simultaneos para primaria en Dia 2. El campus cuenta con 144 equipos fijos en cinco salas de computo, por lo que faltan 120 equipos. El calendario oficial no permite dividir categorias en olas ni agregar turnos.

## Decision

Usar Sala Borrego como Plan A para instalar los 120 equipos externos y conectarlos a Red A / VLAN 10. Usar Auditorio Escuela de Ingenieria como Plan B contingente si Sala Borrego no esta disponible.

## Alternativas consideradas

| Alternativa | Ventaja | Desventaja |
| --- | --- | --- |
| Sala Borrego | Aforo aproximado suficiente para 120 equipos, 3 APs existentes y separacion de roles operativos. | Si falta AP fisico o cobertura minima, requiere colocar AP adicional. |
| Auditorio Escuela de Ingenieria | Aforo aproximado suficiente y funciona como contingencia. | El mobiliario tipo auditorio puede dificultar montar computadoras. |
| Dividir primaria en olas | Reduce necesidad de equipos simultaneos. | Viola la regla actual de no dividir categorias. |

## Consecuencias

Positivas:

- Mantiene calendario oficial y primaria completa.
- Permite llegar a 264 equipos en Red A.
- Conserva roles no competidores separados en sus espacios.

Negativas o riesgos:

- Requiere 120 equipos externos.
- Requiere 3 APs disponibles en Sala Borrego o colocar AP adicional si falta cobertura minima.
- Depende de disponibilidad final de Sala Borrego.

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
