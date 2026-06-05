# AGENTS.md

Instrucciones para cualquier agente o asistente que trabaje en este repo.

## Contexto del proyecto

Este repo documenta, con Spec Driven Development, la infraestructura de red para un evento tipo Olimpiada Mexicana de Informatica en campus sede.

La fuente de verdad vive en `docs/`. Antes de proponer o cambiar algo, leer:

1. `docs/README.md`
2. `docs/SDD-WORKFLOW.md`
3. `docs/00-fuente-de-verdad.md`
4. `docs/01-arquitectura-red.md`
5. `docs/02-eventos-escenarios.md`
6. `docs/03-validacion-operacion.md`
7. `docs/04-espacios-fisicos.md`

## Regla SDD

No hacer cambios importantes solo desde memoria o chat. Si una feature cambia conteos, calendario, VLANs, subredes, SSIDs, permisos, espacios, camaras, capacidad o validacion, primero debe quedar especificada en Markdown.

Flujo obligatorio:

1. Crear o actualizar una spec en `docs/features/`.
2. Crear un ADR en `docs/decisions/` si hay decision tecnica relevante.
3. Integrar el cambio en los documentos base afectados.
4. Actualizar validacion en `docs/03-validacion-operacion.md`.
5. Revisar consistencia entre fuente de verdad, arquitectura, escenarios y pruebas.

## Documentos base

| Archivo | Rol |
| --- | --- |
| `docs/00-fuente-de-verdad.md` | Decisiones oficiales, conteos, calendario, requerimientos y supuestos. |
| `docs/01-arquitectura-red.md` | VLANs, subredes, SSIDs, topologia y reglas de acceso. |
| `docs/02-eventos-escenarios.md` | Usuarios, eventos, restricciones y resultados esperados. |
| `docs/03-validacion-operacion.md` | Matriz de permisos, pruebas, checklist y fallos. |
| `docs/04-espacios-fisicos.md` | Inventario de espacios, alternativas fisicas y bosquejos por espacio. |

## Decisiones vigentes

- Formato oficial: Markdown.
- No usar Gherkin ni YAML para la fuente de verdad.
- Conteo oficial: 31 estados regulares + 1 estado sede con contingente doble (8 prepa, 12 secundaria, 16 primaria).
- Capacidad maxima simultanea de competencia: 264 equipos (turno primaria, Dia 2).
- Red A: VLAN 10, `10.50.0.0/23`.
- Red A hibrida: cable + SSID `OMI-Competidores` en VLAN 10 cuando se use WiFi.
- Red T hibrida: cable + SSID `OMI-Jueces` en VLAN 20 cuando se use WiFi.
- Red C: VLAN 80 para hasta 20 camaras adicionales/rentadas inalambricas por SSID `OMI-Camaras`.
- Expansion Red A: Sala Borrego como Plan A para 120 equipos externos; Auditorio Escuela de Ingenieria como Plan B.
- Las tablets no forman parte del inventario de competencia.

## Privacidad

- No guardar datos personales ni sensibles del correo, imagenes o conversaciones.
- Conservar solo informacion operativa necesaria: espacios, capacidades, inventario, nodos, redes y restricciones.

## Estilo

- Escribir en espanol.
- Mantener Markdown claro, con tablas cuando ayuden.
- Usar ASCII salvo que el archivo ya requiera caracteres especiales.
- No borrar cambios existentes del usuario.
- Evitar refactors grandes si el cambio pedido es puntual.

## Validacion recomendada

Despues de cambios en docs, revisar segun aplique:

```sh
rg -n "iPad|iPads|tablet|Samsung" docs
rg -n "/23|264|OMI-Camaras|VLAN 80|OMI-Competidores|OMI-Jueces" docs
find . -maxdepth 3 -type f \( -name "*.feature" -o -name "*.yaml" -o -name "*.yml" \) -print
git status --short
```

Los primeros comandos son checks de consistencia del estado actual; si una feature futura cambia esas decisiones, actualizar tambien este archivo.
