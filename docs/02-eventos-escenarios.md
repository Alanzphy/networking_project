# Eventos y escenarios OMI

## Modelo SDD

La especificacion se organiza como:

```text
Usuario -> Evento -> Escenario -> Requerimientos -> Restricciones -> Resultado esperado
```


## Usuarios del evento

| Usuario | Red | Cantidad | Tipo de conexion | Criticidad |
| --- | --- | ---: | --- | --- |
| Competidor preparatoria | Red A | 128 | Cableada | Alta |
| Competidor secundaria | Red A | 192 | Cableada | Alta |
| Competidor primaria | Red A | 256 | Cableada | Alta |
| Juez | Red T | 10 | Cableada | Alta |
| Servidor local | Red TI | 1 | Cableada | Alta |
| Impresora | Red TI | 4 | Cableada | Media |
| Reportero | Red Repos | 32 | WiFi | Media |
| Entrenador | Red E | 40 | Mixta/WiFi | Media |
| Invitado | Red I | 100 | WiFi | Baja |
| Administrador de red | Red M | Equipo tecnico | Cableada/gestion | Alta |

## Evento: inicio de turno de competencia

### Escenario: preparatoria inicia Dia 1 por la manana

Requerimientos:

- 128 equipos de Red A disponibles.
- DHCP activo en VLAN 10.
- Servidor local disponible.
- Acceso permitido desde Red A al servidor de concurso.

Restricciones:

- Preparatoria no comparte turno con secundaria o primaria.
- Los equipos deben estar limpios de sesiones anteriores.
- Red A no debe alcanzar redes internas no autorizadas.

Resultado esperado:

- Los 128 competidores reciben IP valida.
- Todos pueden entrar a la plataforma de concurso.
- Ningun competidor puede acceder a jueces, impresoras o gestion.

### Escenario: secundaria inicia Dia 1 por la tarde

Requerimientos:

- 192 equipos de Red A disponibles.
- Reinicio o limpieza logica despues del turno de preparatoria.
- Misma politica de acceso que preparatoria.

Restricciones:

- El cambio de turno debe validar DHCP, DNS, gateway y servidor.
- No deben quedar sesiones o credenciales del turno anterior.

Resultado esperado:

- Los 192 competidores reciben servicio estable.
- La capacidad de Red A no se excede.
- El servidor de concurso recibe envios correctamente.

### Escenario: primaria inicia Dia 2

Requerimientos:

- 256 equipos de Red A disponibles.
- Red A dimensionada con subred `/23`.
- Switches, uplinks y DHCP preparados para el maximo simultaneo.

Restricciones:

- No se divide primaria en olas.
- No se permite usar `/24` en Red A.
- Se debe priorizar Red A si hay congestion.

Resultado esperado:

- Los 256 competidores participan al mismo tiempo.
- No hay agotamiento de direcciones IP.
- La plataforma de concurso se mantiene accesible.

## Evento: competidor envia solucion

Requerimientos:

- El competidor tiene IP de Red A.
- El servidor local esta en Red TI.
- El firewall permite puertos de plataforma desde Red A al servidor.

Restricciones:

- El competidor no puede acceder a administracion del servidor.
- El competidor no puede acceder a impresoras.
- El competidor no puede ver equipos de jueces u otros roles.

Resultado esperado:

- La solucion llega a la plataforma.
- La respuesta del sistema regresa al competidor.
- No se abren rutas laterales hacia redes internas.

## Evento: juez evalua soluciones

Requerimientos:

- El juez usa un equipo en Red T.
- Red T puede llegar al servidor local.
- Red T puede imprimir en Red TI.

Restricciones:

- Red T debe estar aislada de Red A salvo por flujos necesarios hacia servidor.
- Accesos administrativos deben limitarse a usuarios autorizados.

Resultado esperado:

- El juez revisa envios y resultados.
- El juez puede imprimir documentos operativos.
- Los competidores no pueden observar ni interferir con esta actividad.

## Evento: servidor local opera la plataforma

Requerimientos:

- Servidor con IP fija en Red TI.
- Acceso desde Red A para plataforma de concurso.
- Acceso desde Red T para jueces y administracion autorizada.
- Respaldo o procedimiento de recuperacion documentado.

Restricciones:

- El servidor no debe estar en Red A.
- La administracion del servidor no debe exponerse a invitados, reporteros o competidores.
- Los puertos no usados deben permanecer bloqueados.

Resultado esperado:

- La plataforma funciona durante cada turno.
- Los roles acceden solo a lo que necesitan.
- Un fallo puede diagnosticarse rapidamente desde Red M o Red T.

## Evento: impresion operativa

Requerimientos:

- 4 impresoras en Red TI.
- Acceso desde Red T.
- Cola o procedimiento de impresion disponible.

Restricciones:

- Red A no imprime.
- Red Repos, Red E y Red I no imprimen.
- Las impresoras no deben aceptar trafico desde redes no autorizadas.

Resultado esperado:

- Los jueces imprimen documentos necesarios.
- Las impresoras permanecen protegidas.

## Evento: reportero publica informacion

Requerimientos:

- 32 reporteros conectados a `OMI-Reporteros`.
- SSID mapeado a Red Repos.
- Salida a Internet.

Restricciones:

- Reporteros no acceden a Red A, Red T, Red TI, Red E, Red I o Red M.
- El ancho de banda de prensa no debe afectar envios de competidores.

Resultado esperado:

- Los reporteros publican informacion al exterior.
- No pueden alcanzar recursos internos del evento.

## Evento: entrenador consulta resultados

Requerimientos:

- 40 entrenadores con conectividad en Red E.
- Acceso a Internet.
- Acceso de solo lectura al scoreboard si el comite lo autoriza.

Restricciones:

- Entrenadores no acceden a equipos de competidores.
- Entrenadores no acceden a jueces, impresoras o gestion.
- El scoreboard no debe exponer administracion ni datos sensibles.

Resultado esperado:

- Los entrenadores consultan desempeno de sus delegaciones.
- La red no interfiere con la competencia.

## Evento: invitado usa Internet

Requerimientos:

- SSID `OMI-Invitados`.
- Capacidad para 100 invitados.
- Salida a Internet.

Restricciones:

- Invitados no acceden a ninguna red interna.
- Invitados deben tener menor prioridad que Red A.
- El acceso puede limitarse por ancho de banda o portal cautivo.

Resultado esperado:

- Invitados navegan por Internet.
- La red de competencia permanece protegida.

## Evento: administrador monitorea infraestructura

Requerimientos:

- Acceso por Red M.
- Visibilidad de switches, APs, firewall, DHCP y servidor.
- Herramientas de diagnostico disponibles.

Restricciones:

- Red M no debe estar disponible para usuarios generales.
- Credenciales de administracion no deben usarse en redes invitadas.

Resultado esperado:

- El equipo tecnico detecta fallos rapido.
- Puede corregir DHCP, VLANs, APs o reglas sin entrar a redes de usuario.

## Escenarios de fallo

| Fallo | Impacto | Respuesta esperada |
| --- | --- | --- |
| DHCP Red A no entrega IP | Competidores no conectan | Revisar scope VLAN 10, gateway relay y capacidad |
| Servidor local caido | No hay plataforma | Activar procedimiento de recuperacion y avisar a jueces |
| Impresora caida | Jueces no imprimen | Cambiar a otra impresora o cola alternativa |
| AP saturado invitados | Invitados lentos | Limitar ancho de banda o agregar AP |
| Internet congestionado | Publicacion/recursos lentos | Priorizar Red A y limitar Red I |
| VLAN mal asignada | Usuario recibe red incorrecta | Revisar puerto, trunk y tagging |
| ACL demasiado abierta | Riesgo de acceso indebido | Volver a politica deny-by-default |

## Reglas de negocio

- La competencia tiene prioridad sobre cualquier otra red.
- Una categoria no inicia hasta que DHCP, servidor y ACLs hayan sido validados.
- El cambio de turno requiere limpieza logica y prueba rapida.
- Ningun usuario debe recibir mas acceso del necesario para su rol.
- Cualquier excepcion temporal debe registrarse y revertirse al cierre del turno.
