# ActReto01 - Bosquejos de red

Este documento complementa el entregable `ActReto01.md` con los bosquejos de red de las dos alternativas documentadas para el evento. El Plan A queda como alternativa seleccionada y el Plan B queda como contingencia si Sala Borrego no cumple las condiciones físicas requeridas.

## Bosquejo Plan A - Sala Borrego

```mermaid
---
config:
  theme: default
  layout: dagre
  fontFamily: '''Recursive Variable'', sans-serif'
  themeVariables:
    fontFamily: '''Recursive Variable'', sans-serif'
---
flowchart LR
    internet[Internet] --> firewall[Firewall / Router L3]
    firewall --> core[Switch core del campus]

    core --> redABase[Red A base / VLAN 10]
    core --> redAExpansion[Red A expansion / VLAN 10]
    core --> roles[Espacios por rol]
    core --> redTI[Red TI / VLAN 30]
    core --> redC[Red C / VLAN 70]
    core --> admin[VLAN 99 Administracion]

    subgraph base["Competidores base - 144 equipos"]
        redABase --> s1223[Sala 1223 / 40 PC]
        redABase --> s1224[Sala 1224 / 30 iMac]
        redABase --> s12102[Sala 12102 / 30 PC]
        redABase --> s12104[Sala 12104 / 34 equipos]
        redABase --> s12401[Sala 12401 / 10 PC]
    end

    subgraph expansionA["Expansion Plan A"]
        redAExpansion --> borrego[Sala Borrego / 120 equipos externos]
        borrego --> wifiBorrego[SSID OMI-Competidores / APs existentes]
    end

    subgraph servicios["Servicios y operacion"]
        redTI --> servidor[Servidor de concurso]
        redTI --> impresoras[4 impresoras]
        redC --> camaras[Camaras adicionales/rentadas / SSID OMI-Camaras]
        admin --> controladora[Controladora Packet Tracer]
    end

    subgraph espacios["Roles no competidores"]
        roles --> jueces[Domo Life / Red T VLAN 20 / 10 jueces]
        roles --> entrenadores[Auditorio ENH / Red E VLAN 50 / 40 entrenadores]
        roles --> reporteros[Domo ENH / Red Repos VLAN 40 / 32 reporteros]
        roles --> invitados[Sala Menlo / Red I VLAN 60 / 100 invitados]
    end
```

## Bosquejo Plan B - Auditorio Escuela de Ingeniería

```mermaid
---
config:
  theme: default
  layout: dagre
  fontFamily: '''Recursive Variable'', sans-serif'
  themeVariables:
    fontFamily: '''Recursive Variable'', sans-serif'
---
flowchart LR
    internet[Internet] --> firewall[Firewall / Router L3]
    firewall --> core[Switch core del campus]

    core --> redABase[Red A base / VLAN 10]
    core --> redAExpansion[Red A expansion / VLAN 10]
    core --> roles[Espacios por rol]
    core --> redTI[Red TI / VLAN 30]
    core --> redC[Red C / VLAN 70]
    core --> admin[VLAN 99 Administracion]

    subgraph base["Competidores base - 144 equipos"]
        redABase --> s1223[Sala 1223 / 40 PC]
        redABase --> s1224[Sala 1224 / 30 iMac]
        redABase --> s12102[Sala 12102 / 30 PC]
        redABase --> s12104[Sala 12104 / 34 equipos]
        redABase --> s12401[Sala 12401 / 10 PC]
    end

    subgraph expansionB["Expansion Plan B"]
        redAExpansion --> audIng[Auditorio Escuela de Ingenieria / 120 equipos externos]
        audIng --> wifiAud[SSID OMI-Competidores / AP o cobertura temporal]
    end

    subgraph servicios["Servicios y operacion"]
        redTI --> servidor[Servidor de concurso]
        redTI --> impresoras[4 impresoras]
        redC --> camaras[Camaras adicionales/rentadas / SSID OMI-Camaras]
        admin --> controladora[Controladora Packet Tracer]
    end

    subgraph espacios["Roles no competidores"]
        roles --> jueces[Domo Life / Red T VLAN 20 / 10 jueces]
        roles --> entrenadores[Auditorio ENH / Red E VLAN 50 / 40 entrenadores]
        roles --> reporteros[Domo ENH / Red Repos VLAN 40 / 32 reporteros]
        roles --> invitados[Sala Menlo / Red I VLAN 60 / 100 invitados]
    end
```

## Leyenda de VLANs

| VLAN | Red | Uso |
| ---: | --- | --- |
| 10 | Red A | Competidores base y equipos externos |
| 20 | Red T | Jueces |
| 30 | Red TI | Servidor e impresoras |
| 40 | Red Repos | Reporteros |
| 50 | Red E | Entrenadores |
| 60 | Red I | Invitados |
| 70 | Red C | Cámaras inalámbricas rentadas |
| 99 | Administración | Controladora Packet Tracer |
