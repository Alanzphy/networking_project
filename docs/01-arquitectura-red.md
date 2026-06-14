# Arquitectura de red OMI

## Objetivo tecnico

Disenar una red segmentada, segura y operable para soportar la competencia OMI durante dos dias, con 264 equipos maximos en la Red A y roles separados por VLAN.

La red del evento se plantea como una red local paralela integrada a la infraestructura actual del campus. Su interconexion debe conservar la continuidad operativa de la red institucional, usar bloques IP autorizados por TI Nacional/campus y aplicar restricciones que eviten interferencias con servicios existentes.

La arquitectura se basa en:

- VLAN por tipo de usuario.
- Subred independiente por VLAN.
- Enrutamiento inter-VLAN controlado.
- Firewall o ACLs para permitir solo flujos necesarios.
- WiFi separado por SSID y mapeado a VLAN.
- Red A hibrida: cable donde ya existe infraestructura suficiente y WiFi para equipos portatiles o rentados.
- VLAN 99 para Administracion tecnica/controladora en Packet Tracer, aislada de usuarios.

## Justificacion de subnetting

Si todos los usuarios compartieran una sola red IP, seria dificil aislar competidores, jueces, servidor, impresoras, prensa, entrenadores e invitados. El subnetting permite que cada VLAN tenga su propio rango y que las reglas de acceso sean claras.

La Red A necesita una subred mayor a `/24`. Un `/24` ofrece 254 hosts utiles, pero primaria requiere 264 computadoras simultaneas, ademas de gateway, reservas y posibles equipos de soporte. Por eso Red A debe usar `/23`.

La VLAN 99 usa `/28` porque solo requiere un segmento tecnico pequeno para gateway, controladora y posibles reservas de administracion. No forma parte de las redes de usuarios del evento.

## Bloque base sugerido

Bloque propuesto para las VLANs internas del evento:

```text
172.23.8.0/21
```

Este bloque ofrece direcciones desde `172.23.8.0` hasta `172.23.15.255`. El enlace WAN entre RTFrontera y FCampus queda fuera de este bloque y usa `10.32.100.0/30`.

## Plan de VLANs y subredes

| Segmento | Hosts requeridos | Prefijo | Mascara decimal | Bloque asignado | Primera IP valida | Ultima IP valida |
| --- | ---: | --- | --- | --- | --- | --- |
| Competidores (VLAN 10) | 264 | `/23` | `255.255.254.0` | `172.23.8.0 - 172.23.9.255` | `172.23.8.1` | `172.23.9.254` |
| Invitados (VLAN 60) | 100 | `/25` | `255.255.255.128` | `172.23.10.0 - 172.23.10.127` | `172.23.10.1` | `172.23.10.126` |
| Entrenadores (VLAN 50) | 40 | `/26` | `255.255.255.192` | `172.23.10.128 - 172.23.10.191` | `172.23.10.129` | `172.23.10.190` |
| Reporteros (VLAN 40) | 32 | `/26` | `255.255.255.192` | `172.23.10.192 - 172.23.10.255` | `172.23.10.193` | `172.23.10.254` |
| Camaras (VLAN 70) | 20 | `/27` | `255.255.255.224` | `172.23.11.0 - 172.23.11.31` | `172.23.11.1` | `172.23.11.30` |
| Jueces (VLAN 20) | 10 | `/28` | `255.255.255.240` | `172.23.11.32 - 172.23.11.47` | `172.23.11.33` | `172.23.11.46` |
| Servidor e impresoras (VLAN 30) | 5 | `/29` | `255.255.255.248` | `172.23.11.48 - 172.23.11.55` | `172.23.11.49` | `172.23.11.54` |
| Administracion (VLAN 99) | 14 | `/28` | `255.255.255.240` | `172.23.11.64 - 172.23.11.79` | `172.23.11.65` | `172.23.11.78` |
| Enlace WAN RTFrontera - FCampus | 2 | `/30` | `255.255.255.252` | `10.32.100.0 - 10.32.100.3` | `10.32.100.1` | `10.32.100.2` |

## Reservas recomendadas

| Red | Rango DHCP sugerido | Reservas |
| --- | --- | --- |
| Red A | `172.23.8.50 - 172.23.9.254` | Gateway, reservas y equipos de soporte |
| Red T | `172.23.11.34 - 172.23.11.46` | Gateway y puestos fijos |
| Red TI | No DHCP o DHCP reservado | Servidor e impresoras con IP fija |
| Red Repos | `172.23.10.194 - 172.23.10.254` | Gateway y APs fuera de la VLAN cliente |
| Red E | `172.23.10.130 - 172.23.10.190` | Gateway y equipo de apoyo |
| Red I | `172.23.10.10 - 172.23.10.126` | Gateway y reservas |
| Red C | DHCP reservado | Hasta 20 camaras adicionales/rentadas inalambricas |
| Administracion | No DHCP o DHCP reservado | Gateway, controladora Packet Tracer y equipo tecnico autorizado |

## Direcciones estaticas de servicios

| Servicio | Red | IP sugerida | Notas |
| --- | --- | --- | --- |
| Servidor de concurso | Red TI | `172.23.11.50` | Plataforma visible desde Red A y Red T |
| Impresora 1 | Red TI | `172.23.11.51` | Solo jueces/TI |
| Impresora 2 | Red TI | `172.23.11.52` | Solo jueces/TI |
| Impresora 3 | Red TI | `172.23.11.53` | Solo jueces/TI |
| Impresora 4 | Red TI | `172.23.11.54` | Solo jueces/TI |
| Controladora Packet Tracer | Administracion / VLAN 99 | `172.23.11.66` | Administracion tecnica; gateway sugerido `172.23.11.65` |
| DNS/DHCP del evento | Campus o firewall | Por definir | Debe entregar scopes por VLAN |

## Topologia logica

```text
Internet
   |
Firewall / Router capa 3
   |
Troncal campus / core switch
   |
+------------------+------------------+--------------------------+
|                  |                  |                          |
Switches Red A     Switch jueces/TI   Switch/APs WiFi y camaras   Controladora
VLAN 10            VLAN 20/30         VLAN 40/50/60/70             VLAN 99
```

Salones de computo con equipo:

```text
Sala 1223  (40 PC Win11, 43 puertos, 1 AP) VLAN 10 cableada + OMI-Camaras
Sala 1224  (30 iMac, 48 puertos, 1 AP)     VLAN 10 por OMI-Competidores
Sala 12102 (30 PC Win11, 40 puertos, 1 AP) VLAN 10 cableada + OMI-Camaras
Sala 12104 (15 PC + 19 MB, 8 puertos, 1 AP) VLAN 10 hibrida
Sala 12401 (10 PC Win11, 20 puertos, 1 AP)  VLAN 10 cableada
Sala Borrego (120 laptops externas, 3 AP)   VLAN 10 por OMI-Competidores (Plan A)
Auditorio Ingenieria (120 externos)         Plan B contingente pendiente de levantamiento
```

Espacios de gran capacidad del evento:

```text
Sala Menlo         (~300 personas)  VLAN 60 (invitados)
Auditorio ENH      (~150 personas)  VLAN 50 (entrenadores)
Domo ENH           (variable)       VLAN 40 (prensa y reporteros)
Domo Life          (variable)       VLAN 20 (jueces)
```

Nota: el inventario completo de espacios disponibles vive en `00-fuente-de-verdad.md` y sirve como contexto para decisiones, alternativas y planes operativos.

El enrutamiento entre VLANs debe pasar por el firewall o por un dispositivo capa 3 con reglas equivalentes.

La controladora de Packet Tracer se ubica en Administracion / VLAN 99 con IP sugerida `172.23.11.66`. Esta VLAN no debe ser alcanzable desde redes de usuarios.

## SSIDs WiFi

| SSID | VLAN | Usuarios | Seguridad minima |
| --- | ---: | --- | --- |
| `OMI-Competidores` | 10 | Competidores por WiFi | WPA2/WPA3 con clave controlada |
| `OMI-Jueces` | 20 | Jueces por WiFi | WPA2/WPA3 con clave controlada |
| `OMI-Reporteros` | 40 | Reporteros | WPA2/WPA3 |
| `OMI-Entrenadores` | 50 | Entrenadores | WPA2/WPA3 |
| `OMI-Invitados` | 60 | Invitados | WPA2/WPA3 o portal cautivo |
| `OMI-Camaras` | 70 | Camaras adicionales/rentadas | WPA2/WPA3 con clave controlada |

Buenas practicas:

- No publicar SSIDs internos de administracion.
- `OMI-Competidores` debe mapearse solo a VLAN 10 y recibir las mismas ACLs que Red A cableada.
- `OMI-Jueces` debe mapearse solo a VLAN 20 y recibir las mismas ACLs que Red T cableada.
- Separar invitados de cualquier red operativa.
- Separar camaras adicionales/rentadas de usuarios mediante VLAN 70.
- Limitar ancho de banda de invitados si hay congestion.
- Priorizar Red A y Red T sobre redes WiFi no criticas.
- Cada AP se dimensiona con capacidad aproximada de 150 equipos para estimar si hacen falta APs adicionales.

## Reglas de acceso

Politica base: negar por defecto y permitir solo lo necesario.

| Origen | Destino | Acceso | Motivo |
| --- | --- | --- | --- |
| Red A | Servidor concurso `172.23.11.50` | Permitir HTTP/HTTPS y puertos de plataforma | Envio de soluciones |
| Red A | Internet | Permitir segun politica del concurso | Recursos autorizados o plataforma externa |
| Red A | Red T | Bloquear | Aislamiento de jueces |
| Red A | Red TI impresoras | Bloquear | Evitar uso no autorizado |
| Red A | Red E, Repos, I | Bloquear | Aislamiento |
| Red T | Servidor concurso | Permitir | Evaluacion y administracion |
| Red T | Impresoras | Permitir | Impresion operativa |
| Red T | Internet | Permitir | Operacion |
| Red TI | Internet | Permitir limitado | Actualizaciones o servicios necesarios |
| Red Repos | Internet | Permitir | Publicacion de informacion |
| Red Repos | Redes internas | Bloquear | Seguridad |
| Red E | Internet | Permitir | Consulta general |
| Red E | Scoreboard/servidor | Permitir solo lectura si aplica | Seguimiento de delegaciones |
| Red I | Internet | Permitir | Invitados |
| Red I | Redes internas | Bloquear | Seguridad |
| Acceso tecnico autorizado fuera de VLANs de usuario | Red C | Permitir solo visualizacion de video si aplica | Vigilancia de salones con camaras adicionales/rentadas |
| Acceso tecnico autorizado | Administracion / VLAN 99 | Permitir solo administracion de controladora e infraestructura | Operacion tecnica en Packet Tracer |
| Red A / Red T / Red TI / Red Repos / Red E / Red I / Red C | Administracion / VLAN 99 | Bloquear | La controladora no debe quedar expuesta a usuarios ni servicios del evento |
| Red C | Redes de usuario | Bloquear | Las camaras no deben ser accesibles desde competidores ni invitados |
| Red A / Red I / Red E / Red Repos | Red C | Bloquear | Ningun usuario accede a camaras |

## Priorizacion de trafico

Orden recomendado de prioridad:

1. Red A hacia servidor de concurso.
2. Red T hacia servidor e impresoras.
3. Red E y Red Repos.
4. Red I invitados.

Si el enlace a Internet se congestiona, se debe limitar Red I antes de afectar Red A.

## Requerimientos de infraestructura fisica

Salones de computo (Red A + Red C inalambrica):

- Red A se implementa de forma hibrida: Ethernet donde ya hay puertos suficientes y WiFi por `OMI-Competidores` para equipos portatiles, iMacs, MacBooks y laptops rentadas.
- Sala 1223 tiene 43 puertos Ethernet, 1 AP, 9 tomas y 1 camara observada.
- Sala 1224 tiene 48 puertos Ethernet, 1 AP, 24 tomas y 1 camara observada; se usara WiFi preferente para evitar instalaciones adicionales.
- Sala 12102 tiene 40 puertos Ethernet, 1 AP y 43 tomas.
- Sala 12104 tiene 8 puertos Ethernet, 1 AP y 16 tomas; se operara de forma hibrida.
- 12401 tiene 20 puertos Ethernet, 1 AP y 0 camaras observadas; sus 10 PCs base operan por cable en Red A.
- Sala Borrego debe conectarse a Red A / VLAN 10 para 120 laptops externas por `OMI-Competidores` en Plan A.
- Auditorio Escuela de Ingenieria debe poder reemplazar a Sala Borrego como Plan B para los mismos 120 equipos externos.
- Sala Borrego tiene 3 APs y 1 camara observada; con capacidad aproximada de 150 equipos por AP, no requiere APs adicionales como base para 120 laptops.
- La expansion Red A no requiere nodos Cat 6a nuevos como base; solo se agregarian si una necesidad fisica puntual lo vuelve necesario.
- Hasta 20 camaras adicionales/rentadas deben conectarse por WiFi al SSID `OMI-Camaras`, asignado a VLAN 70.
- Las camaras observadas se contemplan como cobertura existente: 1223 (1), 1224 (1) y Sala Borrego (1).
- La distribucion propuesta de camaras adicionales/rentadas es: 1223 (3), 1224 (3), 12102 (4), 12104 (4), 12401 (3) y Sala Borrego (2).
- Las camaras adicionales/rentadas no requieren punto de red por unidad, pero si energia, bateria o toma electrica y conectividad al SSID `OMI-Camaras`.
- Switches de acceso se consideran existentes o no rentados en la propuesta base; se mantienen solo si hacen falta para uplinks, APs o servicios criticos.

Espacios de gran capacidad:

- Domo Life: 3 APs, 2 puertos Ethernet y 56 tomas para jueces en Red T / `OMI-Jueces`.
- Auditorio ENH: 2 APs, 16 puertos Ethernet y 12 tomas para entrenadores en Red E.
- Domo ENH: 2 APs, 1 puerto Ethernet y 34 tomas para reporteros en Red Repos.
- Sala Menlo: pendiente de levantamiento actualizado para invitados en Red I.

General:

- Conectividad Red A probada por cable y por `OMI-Competidores` antes de cada turno.
- WiFi de Sala Borrego probado con laptops de competencia antes del turno de primaria.
- Uplinks troncales con capacidad suficiente hacia core/firewall.
- Energia estable para switches, servidor, impresoras, APs y camaras adicionales/rentadas.
- Access points suficientes para reporteros, entrenadores, invitados y camaras inalambricas.
- Etiquetado fisico de puertos por salon y por VLAN.
- Controladora de Packet Tracer conectada a VLAN 99 Administracion.

## Restricciones tecnicas

- No mezclar puertos de competidores con puertos de jueces.
- No conectar impresoras a WiFi de invitados o prensa.
- No administrar switches desde redes de usuario.
- No exponer VLAN 99 a redes de usuario.
- No depender de direccionamiento automatico sin scopes DHCP separados.
- No usar el mismo SSID para roles distintos.
- No conectar camaras al SSID de invitados, reporteros o entrenadores.

## Criterios de aceptacion tecnica

- Cada VLAN entrega IP del rango correcto.
- El gateway de cada VLAN responde desde clientes autorizados.
- Red A soporta 264 clientes con IP unica.
- `OMI-Competidores` entrega IP de Red A / VLAN 10 y aplica las mismas reglas de acceso que la Red A cableada.
- `OMI-Jueces` entrega IP de Red T / VLAN 20 y permite acceso a servidor e impresoras.
- El servidor local responde desde Red A y Red T.
- Las impresoras responden desde Red T y no desde Red A.
- Invitados, reporteros y entrenadores no alcanzan redes internas no autorizadas.
- La administracion de switches, APs, firewall y controladora se realiza fuera de VLANs de usuario; la controladora de Packet Tracer usa VLAN 99.
- Las camaras adicionales/rentadas reciben IP en `172.23.11.0/27` por `OMI-Camaras` y solo pueden ser consultadas desde acceso tecnico autorizado fuera de las VLANs de usuario del evento.
- Ningun equipo de competidor, invitado o reportero puede alcanzar Red C.
- VLAN 99 responde en `172.23.11.64/28` y la controladora sugerida `172.23.11.66` no es alcanzable desde redes de usuario.
