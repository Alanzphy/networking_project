# Features SDD

Esta carpeta guarda specs de features o cambios antes de integrarlos a la fuente de verdad principal.

## Como usarla

1. Copiar `TEMPLATE-feature.md`.
2. Nombrar el archivo como `F###-nombre-corto.md`.
3. Completar problema, objetivo, requerimientos, restricciones, escenarios e impacto.
4. Integrar los cambios en los documentos base cuando la feature sea aprobada.

## Ejemplos de features

- `F001-expansion-red-a-120.md`: expansion fisica de Red A con Sala Borrego como Plan A para 120 equipos externos.
- `F002-asignacion-final-espacios.md`: resolver asignacion definitiva de salones y espacios.
- `F003-red-a-hibrida-wifi.md`: definir Red A hibrida con WiFi optimizado.
- `F004-plan-ip-sin-vlan-gestion.md`: eliminar VLAN de gestion, mover camaras a VLAN 70 y adoptar `172.23.8.0/21`.

## Regla

Una feature no reemplaza a los documentos base. Cuando se aprueba, debe integrarse en `00`, `01`, `02` y/o `03` segun corresponda.
