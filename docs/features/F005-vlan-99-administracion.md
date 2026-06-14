# F005 - VLAN 99 de Administracion para controladora

## Estado

Integrada

## Problema

El armado fisico en Packet Tracer requiere colocar una controladora para la administracion tecnica de la infraestructura simulada. El diseno vigente no reservaba una VLAN de administracion dentro del plan IP, lo que dejaba a la controladora sin segmento propio y podia inducir a ubicarla en una red de usuarios.

## Objetivo

Agregar una VLAN de Administracion tecnica con alcance limitado para Packet Tracer y controladora, sin mezclarla con competidores, jueces, invitados, reporteros, entrenadores, servidor/impresoras ni camaras.

## Alcance

Incluye:

- Agregar VLAN 99 como Administracion tecnica.
- Asignar la subred `172.23.11.64/28`.
- Reservar el bloque `172.23.11.64 - 172.23.11.79`.
- Usar mascara `255.255.255.240`.
- Sugerir gateway `172.23.11.65`.
- Sugerir controladora Packet Tracer en `172.23.11.66`.
- Bloquear el acceso desde redes de usuario hacia VLAN 99.

No incluye:

- Cambiar conteos, calendario, espacios o capacidad maxima.
- Cambiar Red A, Red T, Red TI, Red C ni los SSIDs existentes.
- Publicar un SSID de administracion.
- Permitir administracion desde redes de usuarios.

## Requerimientos

| ID | Requerimiento | Prioridad |
| --- | --- | --- |
| RF-F005-01 | La VLAN 99 debe existir como segmento de Administracion tecnica para controladora e infraestructura simulada. | Alta |
| RF-F005-02 | La controladora debe usar una IP de `172.23.11.64/28`, preferentemente `172.23.11.66`. | Alta |
| RF-F005-03 | Las redes de usuarios no deben poder acceder a VLAN 99. | Alta |
| RNF-F005-01 | La VLAN 99 no debe afectar la continuidad operativa ni el aislamiento de roles del evento. | Alta |

## Restricciones

- VLAN 99 no es una red para competidores, jueces, invitados, entrenadores, reporteros ni camaras.
- VLAN 99 no reemplaza a Red TI; Red TI se conserva para servidor e impresoras.
- No se debe publicar un SSID de administracion para usuarios.
- El acceso administrativo debe quedar limitado a equipo tecnico autorizado.

## Escenarios

### Escenario 1: controladora usa VLAN 99

Condiciones:

- VLAN 99 esta creada en el core/firewall o switch capa 3.
- La controladora esta conectada a un puerto de administracion o segmento autorizado.

Accion:

- Se asigna a la controladora la IP `172.23.11.66/28` con gateway `172.23.11.65`.

Resultado esperado:

- La controladora responde dentro de VLAN 99.
- El equipo tecnico puede operar la controladora desde acceso autorizado.
- Las redes de usuarios no alcanzan la controladora.

### Escenario 2: usuario intenta acceder a administracion

Condiciones:

- Un cliente esta en Red A, Red I, Red E, Red Repos, Red T o Red C.
- La VLAN 99 esta activa.

Accion:

- El cliente intenta llegar a `172.23.11.66`.

Resultado esperado:

- El acceso es bloqueado por firewall o ACL.
- La controladora no queda expuesta a usuarios del evento.

## Impacto en documentos base

| Documento | Cambio requerido |
| --- | --- |
| `00-fuente-de-verdad.md` | Agregar decision oficial, requerimientos, especificacion y supuestos de VLAN 99. |
| `01-arquitectura-red.md` | Agregar VLAN 99 al plan IP, reservas, direcciones estaticas, topologia, reglas y criterios tecnicos. |
| `02-eventos-escenarios.md` | Actualizar evento de administracion tecnica para usar VLAN 99. |
| `03-validacion-operacion.md` | Agregar VLAN 99 a checklist, matriz, pruebas y monitoreo. |
| `04-espacios-fisicos.md` | Actualizar leyenda de VLANs. |

## Validacion

- Confirmar que VLAN 99 aparece como Administracion tecnica.
- Confirmar que `172.23.11.64/28` no se traslapa con otras subredes.
- Confirmar que `172.23.11.66` queda reservado para controladora.
- Probar que redes de usuario no alcanzan VLAN 99.

## Decisiones pendientes

- Ninguna.

## Supuestos

- La controladora se modela en Packet Tracer como equipo de administracion tecnica.
- El equipo tecnico autorizado tiene un mecanismo de acceso fuera de redes de usuario o un host administrativo conectado a VLAN 99.
