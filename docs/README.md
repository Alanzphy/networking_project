# Documentacion SDD - Red OMI

Este directorio es la fuente de verdad del proyecto de infraestructura de red para la OMI. La documentacion esta organizada para trabajar con Spec Driven Development: primero se define la especificacion, despues se ajusta la arquitectura y al final se valida contra pruebas.

## Como leer el proyecto

| Archivo | Proposito |
| --- | --- |
| `../AGENTS.md` | Instrucciones para agentes nuevos que trabajen el repo. |
| `00-fuente-de-verdad.md` | Decisiones oficiales, conteos, calendario, requerimientos y supuestos. |
| `01-arquitectura-red.md` | VLANs, subredes, topologia logica, SSIDs y reglas de acceso. |
| `02-eventos-escenarios.md` | Usuarios, eventos, escenarios, restricciones y resultados esperados. |
| `03-validacion-operacion.md` | Matriz de permisos, pruebas, checklist operativo y fallos esperados. |
| `SDD-WORKFLOW.md` | Proceso para proponer, revisar e integrar nuevas features. |

## Carpetas de trabajo

| Carpeta | Uso |
| --- | --- |
| `features/` | Specs de nuevas features o cambios antes de modificar los documentos base. |
| `decisions/` | Decisiones tecnicas importantes que deben quedar justificadas. |

## Regla principal

Ningun cambio importante debe vivir solo en una conversacion. Si una decision afecta conteos, redes, VLANs, permisos, calendario, capacidad o validacion, debe quedar en Markdown dentro de `docs/`.

## Flujo corto

1. Crear una spec en `features/` usando `features/TEMPLATE-feature.md`.
2. Revisar si la feature cambia una decision tecnica; si si, crear un ADR en `decisions/`.
3. Actualizar los documentos base que correspondan.
4. Actualizar pruebas y criterios de aceptacion en `03-validacion-operacion.md`.
5. Verificar que no haya contradicciones entre fuente de verdad, arquitectura, escenarios y validacion.

## Politicas del proyecto

- No guardar datos personales ni sensibles.
- No crear Gherkin ni YAML; el formato oficial es Markdown.
- Mantener la Red A en `/23` mientras el maximo simultaneo siga siendo 256 equipos.
- Documentar supuestos cuando falte una decision final.
- Marcar como pendiente cualquier inconsistencia que no deba resolverse todavia.
