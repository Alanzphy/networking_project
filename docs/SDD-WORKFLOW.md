# Workflow SDD

## Objetivo

Este workflow explica como manejar el proyecto con Spec Driven Development aunque no exista contexto previo de chat. Cualquier persona debe poder abrir `docs/`, leer los archivos y continuar una feature sin adivinar decisiones.

## Principios

- La especificacion manda sobre la implementacion.
- Toda feature empieza como documento, no como cambio tecnico directo.
- Cada requerimiento debe tener escenario y validacion.
- Cada restriccion de seguridad debe aparecer en arquitectura y pruebas.
- Las decisiones importantes deben quedar justificadas en `decisions/`.

## Ciclo de una feature

### 1. Crear spec de feature

Crear un archivo en `docs/features/` con nombre:

```text
F###-nombre-corto.md
```

Ejemplos:

```text
F001-plan-b-capacidad.md
F002-camaras-wifi.md
F003-asignacion-final-espacios.md
```

La spec debe usar `docs/features/TEMPLATE-feature.md`.

### 2. Definir impacto

La feature debe declarar si impacta:

- Conteos o calendario.
- VLANs, subredes o SSIDs.
- Reglas de acceso.
- Usuarios, eventos o escenarios.
- Validacion, operacion o fallos.
- Equipamiento fisico o espacios.

### 3. Registrar decision si aplica

Crear un ADR en `docs/decisions/` cuando la feature tome una decision dificil de revertir, por ejemplo:

- Cambiar capacidad maxima.
- Cambiar bloque IP.
- Agregar una VLAN.
- Cambiar el calendario oficial.
- Aceptar BYOD, equipo rentado o formato por olas.

Usar `docs/decisions/TEMPLATE-adr.md`.

### 4. Actualizar documentos base

Actualizar solo los documentos necesarios:

- `00-fuente-de-verdad.md`: decisiones, conteos, supuestos y requerimientos.
- `01-arquitectura-red.md`: diseno tecnico, VLANs, subredes, SSIDs y reglas.
- `02-eventos-escenarios.md`: usuarios, eventos, restricciones y resultados esperados.
- `03-validacion-operacion.md`: pruebas, matriz de permisos, checklist y fallos.

### 5. Validar consistencia

Antes de cerrar una feature, revisar:

- Todo requerimiento tiene al menos un escenario.
- Toda regla de acceso aparece en la matriz de permisos.
- Toda VLAN/SSID aparece en arquitectura y validacion.
- Todo supuesto importante aparece en fuente de verdad.
- No se agregaron datos sensibles.

## Estado de una feature

Cada spec en `features/` debe tener uno de estos estados:

| Estado | Significado |
| --- | --- |
| `Propuesta` | La idea existe, pero no se ha integrado. |
| `Aprobada` | Se decidio implementarla en los documentos base. |
| `Integrada` | Ya fue reflejada en los 4 documentos base necesarios. |
| `Rechazada` | Se documento y no se hara. |
| `Pendiente` | Falta una decision externa. |

## Criterio de cierre

Una feature queda cerrada cuando:

- Su spec tiene estado `Integrada` o `Rechazada`.
- Los documentos base afectados estan actualizados.
- Las pruebas de aceptacion estan en `03-validacion-operacion.md`.
- Las decisiones tecnicas relevantes tienen ADR.
- No quedan contradicciones sin marcar como pendientes.
