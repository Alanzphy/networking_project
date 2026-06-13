# ADR-003 - Red A hibrida con WiFi optimizado

## Estado

Aceptada

## Contexto

La Red A se habia documentado como cableada para todos los competidores. El levantamiento fisico actualizado muestra APs disponibles en salas y espacios clave, y la expansion de 120 equipos externos se resolvera con laptops rentadas que pueden conectarse por WiFi. Instalar nodos Cat 6a para todos los equipos faltantes elevaria el costo sin ser necesario para la propuesta actual.

## Decision

Adoptar una Red A hibrida: usar Ethernet donde ya exista infraestructura suficiente y usar el SSID `OMI-Competidores`, mapeado a VLAN 10, para equipos portatiles, iMacs, MacBooks y laptops rentadas. Para jueces, habilitar `OMI-Jueces` en VLAN 20 cuando se requiera conectividad inalambrica. Mantener `OMI-Camaras` en la VLAN de camaras vigente para camaras adicionales/rentadas.

## Alternativas consideradas

| Alternativa | Ventaja | Desventaja |
| --- | --- | --- |
| Red A cableada para todos | Mayor previsibilidad fisica por puerto. | Requiere mas nodos, switches, cableado y costo. |
| Red A hibrida | Optimiza recursos existentes y evita instalacion masiva. | Requiere confirmar APs disponibles y configuracion de VLAN por SSID. |
| Todo WiFi | Reduce aun mas cableado. | Aumenta riesgo operativo para equipos que ya tienen conexion cableada estable. |

## Consecuencias

Positivas:

- Reduce el costo base de nodos Cat 6a nuevos a 0.
- Aprovecha APs existentes y laptops rentadas con WiFi.
- Mantiene VLAN 10, `/23`, calendario y reglas de acceso.
- Sala Borrego puede cubrir 120 laptops con 3 APs, considerando 150 equipos aproximados por AP.

Negativas o riesgos:

- Si falta AP fisico o cobertura minima en sitio, se debe agregar un AP adicional.
- La configuracion de SSIDs debe soportar mapeo a VLANs.
- La operacion depende mas del monitoreo de clientes WiFi y saturacion.

## Impacto en documentos

| Documento | Cambio |
| --- | --- |
| `00-fuente-de-verdad.md` | Declara Red A hibrida y APs de 150 equipos aproximados. |
| `01-arquitectura-red.md` | Agrega `OMI-Competidores` y `OMI-Jueces`. |
| `02-eventos-escenarios.md` | Actualiza conexion de competidores y jueces. |
| `03-validacion-operacion.md` | Agrega pruebas de Red A y Red T por WiFi. |
| `04-espacios-fisicos.md` | Integra levantamiento fisico actualizado. |

## Fecha

2026-06-04
