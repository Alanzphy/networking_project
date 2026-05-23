# Features SDD

Esta carpeta guarda specs de features o cambios antes de integrarlos a la fuente de verdad principal.

## Como usarla

1. Copiar `TEMPLATE-feature.md`.
2. Nombrar el archivo como `F###-nombre-corto.md`.
3. Completar problema, objetivo, requerimientos, restricciones, escenarios e impacto.
4. Integrar los cambios en los documentos base cuando la feature sea aprobada.

## Ejemplos de features

- `F001-expansion-red-a-112.md`: expansion fisica de Red A con Sala Borrego como Plan A.
- `F002-asignacion-final-espacios.md`: resolver asignacion definitiva de salones y espacios.
- `F003-plan-cableado-temporal.md`: definir cableado para salas sin nodos.
- `F004-monitoreo-camaras.md`: completar monitoreo de camaras rentadas.

## Regla

Una feature no reemplaza a los documentos base. Cuando se aprueba, debe integrarse en `00`, `01`, `02` y/o `03` segun corresponda.
