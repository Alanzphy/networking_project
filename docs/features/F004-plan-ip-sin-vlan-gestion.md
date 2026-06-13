# F004 - Plan IP sin VLAN de gestion

## Estado

Integrada

## Problema

El diseno anterior reservaba una VLAN de gestion y otra VLAN distinta para camaras. El plan fisico en Packet Tracer requiere simplificar la segmentacion, eliminar la VLAN de gestion del evento y usar VLAN 70 para camaras de monitoreo.

## Objetivo

Actualizar el plan de direccionamiento para usar `172.23.8.0/21`, mover camaras a Red C / VLAN 70 y dejar la administracion tecnica fuera de las VLANs de usuario del evento.

## Alcance

Incluye:

- Cambiar el bloque interno del evento a `172.23.8.0/21`.
- Mantener Red A en VLAN 10 con prefijo `/23`.
- Asociar `OMI-Camaras` a VLAN 70.
- Eliminar la VLAN de gestion del diseno del evento.
- Agregar enlace WAN RTFrontera - FCampus en `10.32.100.0/30`.

No incluye:

- Cambiar conteos, calendario, espacios o capacidad maxima.
- Agregar nuevas VLANs para usuarios.
- Definir marca o modelo especifico de router, firewall, switch o AP.

## Requerimientos

| ID | Requerimiento | Prioridad |
| --- | --- | --- |
| RF-F004-01 | Las VLANs activas del evento deben ser 10, 20, 30, 40, 50, 60 y 70. | Alta |
| RF-F004-02 | Las camaras adicionales/rentadas deben conectarse a `OMI-Camaras` en Red C / VLAN 70. | Alta |
| RF-F004-03 | La administracion tecnica no debe exponerse en ninguna VLAN de usuario del evento. | Alta |
| RNF-F004-01 | El nuevo direccionamiento debe conservar capacidad suficiente para 264 competidores simultaneos. | Alta |

## Restricciones

- Red A conserva `/23` para cubrir 264 competidores mas reservas.
- Red C debe permanecer aislada de competidores, invitados, reporteros y entrenadores.
- El enlace WAN `10.32.100.0/30` queda fuera del bloque interno `172.23.8.0/21`.
- No debe quedar ninguna referencia operativa a la VLAN de gestion anterior ni a una VLAN distinta para camaras.

## Escenarios

### Escenario 1: camara se conecta a Red C

Condiciones:

- La camara adicional/rentada tiene WiFi funcional.
- El SSID `OMI-Camaras` esta mapeado a VLAN 70.

Accion:

- La camara se conecta a `OMI-Camaras`.

Resultado esperado:

- La camara recibe IP de `172.23.11.0/27`.
- El video solo es visible desde acceso tecnico autorizado fuera de las VLANs de usuario del evento.
- Redes de usuarios no pueden alcanzar la camara.

### Escenario 2: competidor recibe direccion en Red A

Condiciones:

- El equipo esta conectado por cable o por `OMI-Competidores`.
- DHCP de VLAN 10 esta activo.

Accion:

- El equipo solicita direccion IP.

Resultado esperado:

- El equipo recibe IP de `172.23.8.0/23`.
- Puede acceder al servidor de concurso.
- No puede acceder a Red C ni a redes no autorizadas.

## Impacto en documentos base

| Documento | Cambio requerido |
| --- | --- |
| `00-fuente-de-verdad.md` | Actualizar decisiones, bloque IP, Red C y eliminacion de VLAN de gestion. |
| `01-arquitectura-red.md` | Reemplazar tabla de VLANs, subredes, gateways, reservas y reglas. |
| `02-eventos-escenarios.md` | Quitar la red de gestion y ajustar camaras a VLAN 70. |
| `03-validacion-operacion.md` | Quitar la red de gestion de matriz y pruebas; validar VLAN 70 para camaras. |
| `04-espacios-fisicos.md` | Actualizar leyenda de VLANs y SSID `OMI-Camaras`. |

## Validacion

- Confirmar que no existan referencias operativas a la VLAN de gestion anterior ni a una VLAN distinta para camaras.
- Confirmar que `OMI-Camaras` aparece asociado a VLAN 70.
- Confirmar que Red A usa `172.23.8.0/23`.
- Probar DHCP por VLAN y aislamiento de Red C.

## Decisiones pendientes

- Ninguna.

## Supuestos

- El bloque `172.23.8.0/21` no entra en conflicto con la red institucional.
- La administracion tecnica se realiza por consola, red institucional autorizada o puertos administrativos fuera de VLANs de usuario.
