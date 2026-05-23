# Olimpiada Mexicana de Informatica
## Diseno de Red - Espacios Fisicos e Infraestructura

---

**Curso:** Redes de Computadoras

**Integrantes del equipo:**

- [Nombre 1]
- [Nombre 2]
- [Nombre 3]
- [Nombre 4]

**Fecha:** Mayo 2026

---

## 1. Identificacion de espacios fisicos requeridos

### a) Numero de espacios fisicos

El evento requiere **10 espacios fisicos operativos minimos** en el Plan A seleccionado:

- 5 salas de computo fijas para competidores.
- 1 espacio de expansion para 112 equipos externos de competidores.
- 4 espacios de gran capacidad para jueces, entrenadores, reporteros e invitados.

### b) Capacidad requerida por espacio

| No. | Espacio requerido | Rol asignado | Personas / equipos necesarios |
| --- | --- | --- | ---: |
| 1 | Sala de computo 1223 | Competidores base | 40 equipos |
| 2 | Sala de computo 1224 | Competidores base | 30 equipos |
| 3 | Sala de computo 12102 | Competidores base | 30 equipos |
| 4 | Sala de computo 12104 | Competidores base | 34 equipos |
| 5 | Sala de computo 12401 | Competidores base | 10 equipos |
| 6 | Sala Borrego | Competidores adicionales | 112 equipos externos |
| 7 | Domo Life | Jueces | 10 personas |
| 8 | Auditorio Escuela de Negocios y Humanidades | Entrenadores | 40 personas |
| 9 | Domo Escuela de Negocios y Humanidades | Reporteros | 32 personas |
| 10 | Sala Menlo | Invitados | 100 personas |

**Total de competidores atendidos simultaneamente:** 256 equipos en Red A, con 144 equipos fijos del campus y 112 equipos externos.
**Total de usuarios no competidores:** 182 personas, sin contar servidor, impresoras y equipo tecnico.

---

## 2. Lista de espacios del campus - Inventario y planes

El inventario completo de espacios disponibles se conserva como contexto para tomar decisiones, comparar alternativas y construir planes operativos. No todos los espacios del inventario tienen que usarse en el plan final.

### Inventario completo de espacios disponibles

| Espacio | Aforo aproximado | Uso posible / contexto |
| --- | ---: | --- |
| Sala Menlo | ~300 personas | Invitados / audiencias grandes |
| Sala Borrego | ~120 personas | Expansion de competidores en Plan A |
| Auditorio Escuela de Negocios y Humanidades | ~150 personas | Entrenadores / comunicados |
| Auditorio Escuela de Ingenieria | ~120 personas | Expansion de competidores en Plan B |
| Domo Escuela de Negocios y Humanidades | Variable | Reporteros / prensa |
| Domo Life | Variable | Jueces / espacio controlado |
| Gimnasio | Variable | Alternativa de gran aforo |
| Cancha de americano | Variable | Alternativa abierta de gran aforo |
| Velarias | Variable | Alternativa abierta de gran aforo |
| Cancha de soccer | Variable | Alternativa abierta de gran aforo |
| Plaza Galileo | Variable | Alternativa abierta / logistica |
| Plaza Borrego | Variable | Alternativa abierta / logistica |
| Innovation Gym PIT3 | ~80 personas | Entrenadores / sesiones de trabajo |

### Salas de computo base

Las salas de computo son comunes a Plan A y Plan B porque contienen los equipos fijos del campus.

| Sala | Equipos disponibles | Sistema operativo | Nodos de red |
| --- | --- | --- | ---: |
| 1223 | 40 PC | Windows 11 | 40 |
| 1224 | 30 iMac | macOS | 0 (requiere cableado) |
| 12102 | 30 PC Workstation | Windows 11 | 30 |
| 12104 (Lab de moviles) | 15 PC Workstation + 19 MacBook Pro | Windows 11 / macOS | 14 |
| 12401 (Lab de redes) | 10 PC | Windows 11 | 0 (requiere cableado) |
| **Total base** | **144 equipos** | | **84 nodos** |

Los 144 equipos disponibles no cubren el maximo de 256 requerido para primaria. La diferencia de 112 equipos se resuelve mediante expansion fisica de Red A, sin cambiar calendario y sin dividir categorias.

### Comparativo Plan A vs Plan B

| Rol | Plan A seleccionado | Capacidad Plan A | Plan B contingente | Capacidad Plan B |
| --- | --- | ---: | --- | ---: |
| Competidores base | 5 salas de computo | 144 equipos | 5 salas de computo | 144 equipos |
| Competidores adicionales | Sala Borrego | 112 equipos externos | Auditorio Escuela de Ingenieria | 112 equipos externos |
| Jueces | Domo Life | 10 personas | Domo Life | 10 personas |
| Entrenadores | Auditorio Escuela de Negocios y Humanidades | 40 personas | Auditorio Escuela de Negocios y Humanidades | 40 personas |
| Reporteros | Domo Escuela de Negocios y Humanidades | 32 personas | Domo Escuela de Negocios y Humanidades | 32 personas |
| Invitados | Sala Menlo | 100 personas | Sala Menlo | 100 personas |

---

## 3. Verificacion de capacidad

### Resultados de la verificacion documental

| Espacio | Aforo declarado | Rol verificado | Cumple capacidad |
| --- | ---: | --- | --- |
| Salas de computo (5 salas) | 144 equipos | Competidores base | Si, para equipo fijo disponible |
| Sala Borrego | ~120 personas | 112 equipos externos (Plan A) | Si por aforo nominal; requiere validacion fisica |
| Auditorio Escuela de Ingenieria | ~120 personas | 112 equipos externos (Plan B) | Si por aforo nominal; requiere validacion fisica |
| Domo Life | Variable | Jueces | Si |
| Auditorio Escuela de Negocios y Humanidades | ~150 personas | Entrenadores | Si |
| Domo Escuela de Negocios y Humanidades | Variable | Reporteros | Si |
| Sala Menlo | ~300 personas | Invitados | Si |

### Validacion fisica pendiente

Antes de cerrar la implementacion fisica se debe confirmar:

- Sala Borrego permite instalar 112 computadoras externas con mesas o superficies suficientes.
- Sala Borrego tiene energia suficiente o puede recibir distribucion electrica temporal segura.
- Sala Borrego puede conectarse a Red A / VLAN 10 mediante switches y cableado temporal.
- Auditorio Escuela de Ingenieria cumple las mismas condiciones como Plan B.
- Los 112 equipos externos son computadoras aptas para competencia.

### Observaciones de la visita

- Las salas de computo cuentan con tomas de corriente y puntos de red en numero variable; las salas 1224 y 12401 requieren extension de cableado estructurado.
- Sala Menlo es el espacio abierto de mayor capacidad del campus y se conserva para invitados.
- Domo Life ofrece privacidad suficiente para el area de jueces.
- Auditorio ENH y Auditorio de Ingenieria tienen mobiliario fijo tipo auditorio; si se usan para computadoras se debe validar que el mobiliario soporte el montaje.
- Sala Borrego se selecciona por aforo nominal suficiente para los 112 equipos externos y por mantener separados los roles operativos.

---

## 4. Medidas exactas de los espacios seleccionados

| Espacio | Largo (m) | Ancho (m) | Area (m2) | Observaciones |
| --- | --- | --- | --- | --- |
| Sala 1223 | Pendiente | Pendiente | Pendiente | |
| Sala 1224 | Pendiente | Pendiente | Pendiente | |
| Sala 12102 | Pendiente | Pendiente | Pendiente | |
| Sala 12104 (Lab de moviles) | Pendiente | Pendiente | Pendiente | |
| Sala 12401 (Lab de redes) | Pendiente | Pendiente | Pendiente | |
| Sala Borrego | Pendiente | Pendiente | Pendiente | Plan A para 112 equipos externos |
| Auditorio Escuela de Ingenieria | Pendiente | Pendiente | Pendiente | Plan B para 112 equipos externos |
| Domo Life | Pendiente | Pendiente | Pendiente | Jueces |
| Auditorio ENH | Pendiente | Pendiente | Pendiente | Entrenadores |
| Domo ENH | Pendiente | Pendiente | Pendiente | Reporteros |
| Sala Menlo | Pendiente | Pendiente | Pendiente | Invitados |

Las medidas seran completadas con el plano AutoCAD del campus solicitado al area responsable.

---

## 5. Justificacion de los planes

### Plan A seleccionado - Sala Borrego para expansion Red A

**Salas de computo base (competidores)**
Las 5 salas de computo son las instalaciones con equipo fijo del campus. Aportan 144 equipos para Red A y se usan en todos los turnos de competencia.

**Sala Borrego (112 equipos externos)**
Sala Borrego se selecciona como expansion principal de Red A porque su aforo aproximado de 120 personas permite acomodar los 112 equipos externos necesarios para completar los 256 competidores simultaneos de primaria. Al concentrar los equipos adicionales en un solo espacio, se simplifica la operacion: un solo bloque de switches, cableado temporal, pruebas DHCP, etiquetado y monitoreo.

**Domo Life (jueces)**
El Domo Life conserva separacion fisica para jueces, reduciendo el contacto con competidores, entrenadores, reporteros e invitados. Este aislamiento apoya la integridad de la evaluacion.

**Auditorio Escuela de Negocios y Humanidades (entrenadores)**
El auditorio tiene aforo suficiente para 40 entrenadores y permite operar la Red E con conectividad WiFi o mixta sin interferir con la competencia.

**Domo Escuela de Negocios y Humanidades (reporteros)**
El Domo ENH se conserva para reporteros por su flexibilidad de acomodo y por permitir una red separada de prensa mediante Red Repos / VLAN 40.

**Sala Menlo (invitados)**
Sala Menlo tiene capacidad suficiente para 100 invitados y permite mantener a visitantes separados de los espacios criticos de competencia y jueces.

### Plan B contingente - Auditorio Escuela de Ingenieria para expansion Red A

El Plan B mantiene la misma asignacion de roles, pero reemplaza Sala Borrego por el Auditorio Escuela de Ingenieria para los 112 equipos externos. Solo se activa si Sala Borrego no cumple con disponibilidad, energia, mobiliario, conectividad o condiciones de montaje.

El Auditorio de Ingenieria cuenta con aforo aproximado de 120 personas, suficiente por capacidad nominal. La validacion fisica debe confirmar que el mobiliario permite instalar computadoras y que se puede llevar Red A / VLAN 10 mediante cableado temporal.

---

## 6. Comparativo de preferencia

| Criterio | Plan A: Sala Borrego | Plan B: Auditorio Ingenieria |
| --- | --- | --- |
| Estado | Seleccionado | Contingencia |
| Capacidad para 112 equipos | Suficiente por aforo nominal | Suficiente por aforo nominal |
| Impacto en roles no competidores | Bajo | Bajo |
| Simplicidad operativa | Alta: un solo espacio de expansion | Media: depende de mobiliario de auditorio |
| Riesgo principal | Validar energia, mesas y cableado temporal | Validar mobiliario y cableado temporal |
| Calendario | No cambia | No cambia |
| Categorias por olas | No usa olas | No usa olas |

**Alternativa seleccionada: Plan A.**
El Plan A resuelve la falta de 112 equipos sin modificar la estructura de la olimpiada. Mantiene primaria completa en Dia 2, concentra la expansion de Red A en Sala Borrego y conserva espacios separados para jueces, entrenadores, reporteros e invitados.

---

## 7. Bosquejos de red

### Plan A seleccionado - Topologia por espacio

```text
                        INTERNET
                            |
                      FIREWALL / ROUTER L3
                            |
                       CORE SWITCH
                            |
          +-----------------+-----------------------------+
          |                 |                             |
   +------+-------+   +-----+-------+             +-------+-------+
   | RED A BASE   |   | RED A EXP.  |             | ESPACIOS ROL |
   | VLAN 10      |   | VLAN 10     |             |               |
   |              |   |             |             | DOMO LIFE     |
   | 1223  40 PC  |   | SALA        |             | VLAN 20       |
   | 1224  30 Mac |   | BORREGO     |             | Red T jueces  |
   | 12102 30 PC  |   | 112 equipos |             |               |
   | 12104 34 Eq. |   | externos    |             | AUDITORIO ENH |
   | 12401 10 PC  |   | SW acceso   |             | VLAN 50 Red E |
   |              |   | cableado    |             |               |
   | SSID         |   | temporal    |             | DOMO ENH      |
   | OMI-Camaras  |   +-------------+             | VLAN 40 Repos |
   | VLAN 80      |                               |               |
   +--------------+                               | SALA MENLO    |
                                                  | VLAN 60 Red I |
                                                  +---------------+
```

### Plan B contingente - Topologia por espacio

```text
                        INTERNET
                            |
                      FIREWALL / ROUTER L3
                            |
                       CORE SWITCH
                            |
          +-----------------+-----------------------------+
          |                 |                             |
   +------+-------+   +-----+-------+             +-------+-------+
   | RED A BASE   |   | RED A EXP.  |             | ESPACIOS ROL |
   | VLAN 10      |   | VLAN 10     |             |               |
   | 5 salas      |   | AUDITORIO   |             | DOMO LIFE     |
   | 144 equipos  |   | INGENIERIA  |             | Red T jueces  |
   |              |   | 112 equipos |             |               |
   | SSID         |   | externos    |             | AUDITORIO ENH |
   | OMI-Camaras  |   | SW acceso   |             | Red E         |
   | VLAN 80      |   | cableado    |             |               |
   +--------------+   | temporal    |             | DOMO ENH      |
                      +-------------+             | Red Repos     |
                                                  | SALA MENLO    |
                                                  | Red I         |
                                                  +---------------+
```

Leyenda:

| VLAN | Red | Uso |
| ---: | --- | --- |
| 10 | Red A | Competidores base y equipos externos |
| 20 | Red T | Jueces |
| 30 | Red TI | Servidor e impresoras |
| 40 | Red Repos | Reporteros |
| 50 | Red E | Entrenadores |
| 60 | Red I | Invitados |
| 70 | Red M | Gestion |
| 80 | Red C | Camaras rentadas por WiFi |

Las camaras son rentadas para el evento y se conectan de forma inalambrica al SSID `OMI-Camaras`. No forman parte del inventario actual del campus y no requieren punto de red por camara.

---


