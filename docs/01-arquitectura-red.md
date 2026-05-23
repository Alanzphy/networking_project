# Arquitectura de red OMI

## Objetivo tecnico

Disenar una red segmentada, segura y operable para soportar la competencia OMI durante dos dias, con 256 equipos maximos en la Red A y roles separados por VLAN.

La arquitectura se basa en:

- VLAN por tipo de usuario.
- Subred independiente por VLAN.
- Enrutamiento inter-VLAN controlado.
- Firewall o ACLs para permitir solo flujos necesarios.
- WiFi separado por SSID y mapeado a VLAN.
- Red de gestion aislada para infraestructura.

## Justificacion de subnetting

Si todos los usuarios compartieran una sola red IP, seria dificil aislar competidores, jueces, servidor, impresoras, prensa, entrenadores e invitados. El subnetting permite que cada VLAN tenga su propio rango y que las reglas de acceso sean claras.

La Red A necesita una subred mayor a `/24`. Un `/24` ofrece 254 hosts utiles, pero primaria requiere 256 computadoras simultaneas, ademas de gateway, reservas y posibles equipos de soporte. Por eso Red A debe usar `/23`.

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
| Red C | 80 | `10.50.3.160/27` | `10.50.3.161` | 30 | Camaras rentadas inalambricas |

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
| Red C | DHCP reservado | 20 camaras rentadas inalambricas en 5 salones de computo |

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
Sala 1223  (40 PC Win11,  40 nodos)  VLAN 10 + cobertura SSID OMI-Camaras
Sala 1224  (30 iMac,       0 nodos)  VLAN 10 + cobertura SSID OMI-Camaras  (requiere cableado para computo)
Sala 12102 (30 PC Win11,  30 nodos)  VLAN 10 + cobertura SSID OMI-Camaras
Sala 12104 (15 PC + 19 MB, 14 nodos) VLAN 10 + cobertura SSID OMI-Camaras
Sala 12401 (10 PC Win11,   0 nodos)  VLAN 10 + cobertura SSID OMI-Camaras  (requiere cableado para computo)
```

Espacios de gran capacidad del evento:

```text
Sala Menlo         (~300 personas)  VLAN 50/60 (entrenadores e invitados)
Auditorio ENH      (~150 personas)  VLAN 40    (prensa y reporteros)
Domo ENH           (variable)       VLAN 60    (invitados y soporte logistico)
```

Nota: las asignaciones de espacios se conservan como estan y quedan pendientes de decision final; el inventario completo de espacios disponibles vive en `00-fuente-de-verdad.md`.

El enrutamiento entre VLANs debe pasar por el firewall o por un dispositivo capa 3 con reglas equivalentes.

## SSIDs WiFi

| SSID | VLAN | Usuarios | Seguridad minima |
| --- | ---: | --- | --- |
| `OMI-Reporteros` | 40 | Reporteros | WPA2/WPA3 |
| `OMI-Entrenadores` | 50 | Entrenadores | WPA2/WPA3 |
| `OMI-Invitados` | 60 | Invitados | WPA2/WPA3 o portal cautivo |
| `OMI-Camaras` | 80 | Camaras rentadas | WPA2/WPA3 con clave controlada |

Buenas practicas:

- No publicar SSIDs internos de gestion.
- Separar invitados de cualquier red operativa.
- Separar camaras rentadas de usuarios y gestion mediante VLAN 80.
- Limitar ancho de banda de invitados si hay congestion.
- Priorizar Red A y Red T sobre redes WiFi no criticas.

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
| Red C | Servidor de grabacion/monitoreo en Red M | Permitir solo streaming de video | Vigilancia de salones con camaras rentadas |
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

- 84 nodos de red existentes cubren salas 1223 (40), 12102 (30) y 12104 (14).
- Salas 1224 y 12401 no tienen nodos; requieren cableado adicional para sus 30 y 10 equipos respectivamente.
- 20 camaras de vigilancia rentadas (4 por sala) deben conectarse por WiFi al SSID `OMI-Camaras`, asignado a VLAN 80.
- Las camaras no requieren punto de red por unidad, pero si energia, bateria o toma electrica y cobertura WiFi estable.
- Switches de acceso en cada salon con puertos suficientes para equipos de computo + uplink troncal.

Espacios de gran capacidad:

- Sala Menlo: cobertura WiFi para hasta 300 usuarios simultanios (entrenadores e invitados).
- Auditorio ENH: cobertura WiFi para hasta 150 usuarios (prensa y reporteros).
- Domo ENH: cobertura WiFi segun aforo configurado, VLAN de invitados.

General:

- Cableado probado en todos los equipos de Red A antes de cada turno.
- Uplinks troncales con capacidad suficiente hacia core/firewall.
- Energia estable para switches, servidor, impresoras, APs y camaras rentadas.
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
- Red A soporta 256 clientes con IP unica.
- El servidor local responde desde Red A y Red T.
- Las impresoras responden desde Red T y no desde Red A.
- Invitados, reporteros y entrenadores no alcanzan redes internas no autorizadas.
- La red de gestion alcanza switches, APs y firewall.
- Las 20 camaras rentadas reciben IP en `10.50.3.160/27` por `OMI-Camaras` y solo el sistema de monitoreo puede alcanzarlas.
- Ningun equipo de competidor, invitado o reportero puede alcanzar Red C.
