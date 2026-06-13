# Eventos y escenarios OMI

## Modelo SDD

La especificacion se organiza como:

```text
Usuario -> Evento -> Escenario -> Requerimientos -> Restricciones -> Resultado esperado
```


## Usuarios del evento

| Usuario | Red | Cantidad | Tipo de conexion | Criticidad |
| --- | --- | ---: | --- | --- |
| Competidor preparatoria | Red A | 132 | Hibrida cable/WiFi | Alta |
| Competidor secundaria | Red A | 198 | Hibrida cable/WiFi | Alta |
| Competidor primaria | Red A | 264 | Hibrida cable/WiFi | Alta |
| Juez | Red T | 10 | Hibrida cable/WiFi | Alta |
| Servidor local | Red TI | 1 | Cableada | Alta |
| Impresora | Red TI | 4 | Cableada | Media |
| Reportero | Red Repos | 32 | WiFi | Media |
| Entrenador | Red E | 40 | Mixta/WiFi | Media |
| Invitado | Red I | 100 | WiFi | Baja |
| Camara adicional/rentada | Red C | Hasta 20 | WiFi | Media |

## Evento: inicio de turno de competencia

### Escenario: preparatoria inicia Dia 1 por la manana

Requerimientos:

- 132 equipos de Red A disponibles.
- DHCP activo en VLAN 10.
- SSID `OMI-Competidores` disponible para equipos que usen WiFi.
- Servidor local disponible.
- Acceso permitido desde Red A al servidor de concurso.

Restricciones:

- Preparatoria no comparte turno con secundaria o primaria.
- Los equipos deben estar limpios de sesiones anteriores.
- Red A no debe alcanzar redes internas no autorizadas.

Resultado esperado:

- Los 132 competidores reciben IP valida.
- Todos pueden entrar a la plataforma de concurso.
- Ningun competidor puede acceder a jueces, impresoras, camaras u otras redes de usuario.

### Escenario: secundaria inicia Dia 1 por la tarde

Requerimientos:

- 198 equipos de Red A disponibles.
- Reinicio o limpieza logica despues del turno de preparatoria.
- Misma politica de acceso que preparatoria.
- Conectividad Red A validada por cable y por `OMI-Competidores`.

Restricciones:

- El cambio de turno debe validar DHCP, DNS, gateway y servidor.
- No deben quedar sesiones o credenciales del turno anterior.

Resultado esperado:

- Los 198 competidores reciben servicio estable.
- La capacidad de Red A no se excede.
- El servidor de concurso recibe envios correctamente.

### Escenario: primaria inicia Dia 2

Requerimientos:

- 264 equipos de Red A disponibles.
- Red A dimensionada con subred `/23`.
- APs, uplinks y DHCP preparados para el maximo simultaneo.
- 120 laptops rentadas en Sala Borrego conectadas por `OMI-Competidores`.

Restricciones:

- No se divide primaria en olas.
- No se permite usar `/24` en Red A.
- Se debe priorizar Red A si hay congestion.
- Los competidores por WiFi deben recibir los mismos permisos que los competidores cableados.

Resultado esperado:

- Los 264 competidores participan al mismo tiempo.
- No hay agotamiento de direcciones IP.
- La plataforma de concurso se mantiene accesible.
- Los 120 equipos de Sala Borrego reciben IP de VLAN 10 por WiFi.

## Evento: competidor envia solucion

Requerimientos:

- El competidor tiene IP de Red A.
- El servidor local esta en Red TI.
- El firewall permite puertos de plataforma desde Red A al servidor.
- Si el competidor usa WiFi, debe estar conectado a `OMI-Competidores`.

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

- El juez usa un equipo en Red T por cable o por `OMI-Jueces`.
- Red T puede llegar al servidor local.
- Red T puede imprimir en Red TI.

Restricciones:

- Red T debe estar aislada de Red A salvo por flujos necesarios hacia servidor.
- Accesos administrativos deben limitarse a usuarios autorizados.

Resultado esperado:

- El juez revisa envios y resultados.
- El juez puede imprimir documentos operativos.
- Los competidores no pueden observar ni interferir con esta actividad.

## Evento: competidor se conecta por WiFi

Requerimientos:

- El competidor usa un equipo autorizado con WiFi funcional.
- El SSID `OMI-Competidores` esta mapeado a Red A / VLAN 10.
- El AP del espacio tiene capacidad suficiente; se estima hasta 150 equipos por AP.

Restricciones:

- El SSID no debe compartir VLAN con invitados, reporteros, entrenadores, jueces o camaras.
- La clave o metodo de acceso debe entregarse solo a competidores autorizados.
- Los permisos deben ser iguales a Red A cableada.

Resultado esperado:

- El competidor recibe IP de Red A.
- Puede acceder al servidor de concurso.
- No puede acceder a redes internas no autorizadas.

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
- Un fallo puede diagnosticarse rapidamente desde Red T o desde acceso tecnico autorizado fuera de las VLANs de usuario.

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

- Reporteros no acceden a Red A, Red T, Red TI, Red E, Red I o Red C.
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
- Entrenadores no acceden a jueces, impresoras, camaras ni administracion tecnica.
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

## Evento: equipo tecnico monitorea infraestructura

Requerimientos:

- Acceso por consola, red institucional autorizada o puerto administrativo fuera de las VLANs de usuario del evento.
- Visibilidad de switches, APs, firewall, DHCP y servidor.
- Herramientas de diagnostico disponibles.

Restricciones:

- La administracion tecnica no debe estar disponible para usuarios generales.
- Credenciales de administracion no deben usarse en redes invitadas.
- No se reserva una VLAN de gestion dentro del diseno del evento.

Resultado esperado:

- El equipo tecnico detecta fallos rapido.
- Puede corregir DHCP, VLANs, APs o reglas sin entrar a redes de usuario.

## Evento: camara adicional transmite vigilancia

Requerimientos:

- Camaras observadas consideradas como cobertura existente.
- Hasta 20 camaras adicionales/rentadas distribuidas para completar cobertura de salas de competencia.
- Conexion de camaras adicionales/rentadas al SSID `OMI-Camaras`.
- SSID mapeado a Red C / VLAN 70.
- Cobertura WiFi suficiente en cada sala de computo.
- Energia, bateria o toma electrica disponible por camara.

Restricciones:

- Las camaras observadas forman parte de la infraestructura existente; las adicionales/rentadas se documentan por separado.
- Las camaras no deben conectarse a SSIDs de invitados, reporteros, entrenadores o competidores.
- Competidores, invitados, reporteros y entrenadores no deben acceder a Red C.
- El video solo debe consultarse desde acceso tecnico autorizado fuera de las VLANs de usuario del evento.

Resultado esperado:

- Las camaras adicionales/rentadas reciben IP de Red C.
- La vigilancia de salas opera sin cableado de red por camara.
- El trafico de video queda aislado de la red de competencia.

## Escenarios de fallo

| Fallo | Impacto | Respuesta esperada |
| --- | --- | --- |
| DHCP Red A no entrega IP | Competidores no conectan | Revisar scope VLAN 10, gateway relay y capacidad |
| SSID `OMI-Competidores` falla | Competidores WiFi no conectan | Revisar APs, VLAN 10, autenticacion y DHCP |
| AP Sala Borrego saturado | Laptops rentadas lentas o desconectadas | Redistribuir clientes entre APs o reducir redes no criticas |
| Servidor local caido | No hay plataforma | Activar procedimiento de recuperacion y avisar a jueces |
| Impresora caida | Jueces no imprimen | Cambiar a otra impresora o cola alternativa |
| AP saturado invitados | Invitados lentos | Limitar ancho de banda o agregar AP |
| Internet congestionado | Publicacion/recursos lentos | Priorizar Red A y limitar Red I |
| VLAN mal asignada | Usuario recibe red incorrecta | Revisar puerto, trunk y tagging |
| ACL demasiado abierta | Riesgo de acceso indebido | Volver a politica deny-by-default |
| Camara sin cobertura WiFi | Punto de vigilancia sin video | Reubicar camara, revisar AP o agregar cobertura para `OMI-Camaras` |

## Reglas de negocio

- La competencia tiene prioridad sobre cualquier otra red.
- Una categoria no inicia hasta que DHCP, servidor y ACLs hayan sido validados.
- El cambio de turno requiere limpieza logica y prueba rapida.
- Ningun usuario debe recibir mas acceso del necesario para su rol.
- Cualquier excepcion temporal debe registrarse y revertirse al cierre del turno.
