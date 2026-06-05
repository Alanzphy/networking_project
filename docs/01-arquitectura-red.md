# Arquitectura de red OMI

## Objetivo tecnico

Disenar una red segmentada, segura y operable para soportar la competencia OMI durante dos dias, con 264 equipos maximos en la Red A y roles separados por VLAN.

La arquitectura se basa en:

- VLAN por tipo de usuario.
- Subred independiente por VLAN.
- Enrutamiento inter-VLAN controlado.
- Firewall o ACLs para permitir solo flujos necesarios.
- WiFi separado por SSID y mapeado a VLAN.
- Red A hibrida: cable donde ya existe infraestructura suficiente y WiFi para equipos portatiles o rentados.
- Red de gestion aislada para infraestructura.

## Justificacion de subnetting

Si todos los usuarios compartieran una sola red IP, seria dificil aislar competidores, jueces, servidor, impresoras, prensa, entrenadores e invitados. El subnetting permite que cada VLAN tenga su propio rango y que las reglas de acceso sean claras.

La Red A necesita una subred mayor a `/24`. Un `/24` ofrece 254 hosts utiles, pero primaria requiere 264 computadoras simultaneas, ademas de gateway, reservas y posibles equipos de soporte. Por eso Red A debe usar `/23`.

## Bloque base sugerido

Bloque propuesto para el evento:

```text
10.50.0.0/22
```

Este bloque ofrece direcciones desde `10.50.0.0` hasta `10.50.3.255`. Debe cambiarse si el campus ya usa ese rango.

## Plan de VLANs y subredes

| Red | VLAN | Subred | Gateway sugerido | Hosts utiles | Uso |
| --- | ---: | --- | --- | ---: | --- |
| Red A | 10 | `10.50.0.0/23` | `10.50.0.1` | 510 | Competidores |
| Red T | 20 | `10.50.2.0/27` | `10.50.2.1` | 30 | Jueces |
| Red TI | 30 | `10.50.2.32/28` | `10.50.2.33` | 14 | Servidor e impresoras |
| Red Repos | 40 | `10.50.2.64/26` | `10.50.2.65` | 62 | Reporteros |
| Red E | 50 | `10.50.2.128/26` | `10.50.2.129` | 62 | Entrenadores |
| Red I | 60 | `10.50.3.0/25` | `10.50.3.1` | 126 | Invitados |
| Red M | 70 | `10.50.3.128/27` | `10.50.3.129` | 30 | Gestion |
| Red C | 80 | `10.50.3.160/27` | `10.50.3.161` | 30 | Camaras de monitoreo del evento |

## Reservas recomendadas

| Red | Rango DHCP sugerido | Reservas |
| --- | --- | --- |
| Red A | `10.50.0.50 - 10.50.1.254` | Gateway, monitoreo, equipos de soporte |
| Red T | `10.50.2.10 - 10.50.2.30` | Gateway y puestos fijos |
| Red TI | No DHCP o DHCP reservado | Servidor e impresoras con IP fija |
| Red Repos | `10.50.2.70 - 10.50.2.126` | Gateway y APs fuera de la VLAN cliente |
| Red E | `10.50.2.140 - 10.50.2.190` | Gateway y equipo de soporte |
| Red I | `10.50.3.10 - 10.50.3.126` | Gateway |
| Red M | IPs estaticas | Switches, APs, firewall, monitoreo |
| Red C | DHCP reservado | Hasta 20 camaras adicionales/rentadas inalambricas |

## Direcciones estaticas de servicios

| Servicio | Red | IP sugerida | Notas |
| --- | --- | --- | --- |
| Servidor de concurso | Red TI | `10.50.2.34` | Plataforma visible desde Red A y Red T |
| Impresora 1 | Red TI | `10.50.2.35` | Solo jueces/TI |
| Impresora 2 | Red TI | `10.50.2.36` | Solo jueces/TI |
| Impresora 3 | Red TI | `10.50.2.37` | Solo jueces/TI |
| Impresora 4 | Red TI | `10.50.2.38` | Solo jueces/TI |
| DNS/DHCP del evento | Campus o firewall | Por definir | Debe entregar scopes por VLAN |

## Topologia logica

```text
Internet
   |
Firewall / Router capa 3
   |
Troncal campus / core switch
   |
+------------------+------------------+------------------+------------------+
|                  |                  |                  |                  |
Switches Red A     Switch jueces/TI   Switch/APs WiFi    Gestion / Camaras
VLAN 10            VLAN 20/30         VLAN 40/50/60/80   VLAN 70
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

## SSIDs WiFi

| SSID | VLAN | Usuarios | Seguridad minima |
| --- | ---: | --- | --- |
| `OMI-Competidores` | 10 | Competidores por WiFi | WPA2/WPA3 con clave controlada |
| `OMI-Jueces` | 20 | Jueces por WiFi | WPA2/WPA3 con clave controlada |
| `OMI-Reporteros` | 40 | Reporteros | WPA2/WPA3 |
| `OMI-Entrenadores` | 50 | Entrenadores | WPA2/WPA3 |
| `OMI-Invitados` | 60 | Invitados | WPA2/WPA3 o portal cautivo |
| `OMI-Camaras` | 80 | Camaras adicionales/rentadas | WPA2/WPA3 con clave controlada |

Buenas practicas:

- No publicar SSIDs internos de gestion.
- `OMI-Competidores` debe mapearse solo a VLAN 10 y recibir las mismas ACLs que Red A cableada.
- `OMI-Jueces` debe mapearse solo a VLAN 20 y recibir las mismas ACLs que Red T cableada.
- Separar invitados de cualquier red operativa.
- Separar camaras adicionales/rentadas de usuarios y gestion mediante VLAN 80.
- Limitar ancho de banda de invitados si hay congestion.
- Priorizar Red A y Red T sobre redes WiFi no criticas.
- Cada AP se dimensiona con capacidad aproximada de 150 equipos para estimar si hacen falta APs adicionales.

## Reglas de acceso

Politica base: negar por defecto y permitir solo lo necesario.

| Origen | Destino | Acceso | Motivo |
| --- | --- | --- | --- |
| Red A | Servidor concurso `10.50.2.34` | Permitir HTTP/HTTPS y puertos de plataforma | Envio de soluciones |
| Red A | Internet | Permitir segun politica del concurso | Recursos autorizados o plataforma externa |
| Red A | Red T | Bloquear | Aislamiento de jueces |
| Red A | Red TI impresoras | Bloquear | Evitar uso no autorizado |
| Red A | Red E, Repos, I, M | Bloquear | Aislamiento |
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
| Red M | Equipos de red | Permitir administracion | Operacion tecnica |
| Red C | Servidor de grabacion/monitoreo en Red M | Permitir solo streaming de video | Vigilancia de salones con camaras adicionales/rentadas |
| Red C | Redes de usuario | Bloquear | Las camaras no deben ser accesibles desde competidores ni invitados |
| Red A / Red I / Red E / Red Repos | Red C | Bloquear | Ningun usuario accede a camaras |

## Priorizacion de trafico

Orden recomendado de prioridad:

1. Red A hacia servidor de concurso.
2. Red T hacia servidor e impresoras.
3. Red M para gestion y monitoreo.
4. Red E y Red Repos.
5. Red I invitados.

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
- Hasta 20 camaras adicionales/rentadas deben conectarse por WiFi al SSID `OMI-Camaras`, asignado a VLAN 80.
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

## Restricciones tecnicas

- No mezclar puertos de competidores con puertos de jueces.
- No conectar impresoras a WiFi de invitados o prensa.
- No administrar switches desde redes de usuario.
- No depender de direccionamiento automatico sin scopes DHCP separados.
- No usar el mismo SSID para roles distintos.
- No conectar camaras al SSID de invitados, reporteros, entrenadores o gestion.

## Criterios de aceptacion tecnica

- Cada VLAN entrega IP del rango correcto.
- El gateway de cada VLAN responde desde clientes autorizados.
- Red A soporta 264 clientes con IP unica.
- `OMI-Competidores` entrega IP de Red A / VLAN 10 y aplica las mismas reglas de acceso que la Red A cableada.
- `OMI-Jueces` entrega IP de Red T / VLAN 20 y permite acceso a servidor e impresoras.
- El servidor local responde desde Red A y Red T.
- Las impresoras responden desde Red T y no desde Red A.
- Invitados, reporteros y entrenadores no alcanzan redes internas no autorizadas.
- La red de gestion alcanza switches, APs y firewall.
- Las camaras adicionales/rentadas reciben IP en `10.50.3.160/27` por `OMI-Camaras` y solo el sistema de monitoreo puede alcanzarlas.
- Ningun equipo de competidor, invitado o reportero puede alcanzar Red C.
