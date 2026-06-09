# ActReto02 - Propuesta de infraestructura y costos

## Portada

**Proyecto:** Infraestructura de red para evento tipo Olimpiada Mexicana de Informática

**Curso:** Redes de Computadoras

**Integrantes del equipo:**

- [Nombre 1]
- [Nombre 2]
- [Nombre 3]
- [Nombre 4]

**Fecha:** Junio 2026

---

## 1. Plan A

Se selecciona el **Plan A: Sala Borrego como expansión principal de Red A**.

La decisión se mantiene porque permite cubrir el máximo simultáneo de 264 competidores en primaria sin cambiar el calendario, sin dividir categorías y sin crear olas. Las cinco salas de cómputo aportan 144 equipos fijos del campus y Sala Borrego recibe 120 laptops rentadas para completar la capacidad requerida.

La mejora respecto a la propuesta anterior es que la Red A se implementa de forma **híbrida**. Se usará cable donde ya existe infraestructura suficiente y WiFi donde sea más conveniente evitar instalaciones adicionales. Para ello se agrega el SSID `OMI-Competidores`, mapeado a Red A / VLAN 10. Los jueces pueden usar `OMI-Jueces`, mapeado a Red T / VLAN 20. Las cámaras adicionales/rentadas conservan `OMI-Camaras`, mapeado a Red C / VLAN 80.

Cada AP se considera con capacidad aproximada de 150 equipos para dimensionar si hacen falta APs adicionales. Sala Borrego tiene 3 APs, por lo que los 120 equipos externos no requieren APs adicionales como base.

### Por qué lo decidimos así

La alternativa seleccionada es la más adecuada porque atiende el requerimiento crítico del evento: tener 264 equipos de competencia disponibles en el turno de primaria sin modificar el calendario oficial. La opción de dividir categorías o crear olas se descarta porque cambiaría la logística del concurso y aumentaría el riesgo operativo. En cambio, el Plan A conserva la competencia completa, mantiene los roles separados y aprovecha espacios ya identificados.

La decisión también se sustenta en la infraestructura existente. Las salas de cómputo aportan 144 equipos fijos y Sala Borrego permite concentrar los 120 equipos externos faltantes en un solo espacio. Al usar laptops rentadas conectadas por `OMI-Competidores`, se evita instalar nodos Cat 6a masivos y se reduce la necesidad de switches rentados. Esto mantiene la segmentación de Red A / VLAN 10 y reduce intervenciones físicas en el campus.

Desde el punto de vista de seguridad y operación, la alternativa conserva una red por rol: competidores, jueces, servidor/impresoras, reporteros, entrenadores, invitados, gestión y cámaras. Esta separación facilita aplicar ACLs, validar DHCP por VLAN y bloquear accesos no autorizados. Por esa razón, el Plan A no solo cubre capacidad, sino que también mantiene una topología administrable para el equipo técnico durante los dos días del evento.

## 2. Inventario de espacios e infraestructura observada

| Lugar | Uso | Puertos Ethernet actuales | APs | Tomas de corriente | Cámaras observadas | Conexión propuesta |
| --- | --- | ---: | ---: | ---: | ---: | --- |
| Sala 1223 | Competidores base | 43 | 1 | 9 | 1 | Cableada |
| Sala 1224 | Competidores base | 48 | 1 | 24 | 1 | WiFi preferente |
| Sala 12102 | Competidores base | 40 | 1 | 43 | 0 | Cableada |
| Sala 12104 | Competidores base | 8 | 1 | 16 | 0 | Híbrida |
| Sala 12401 | Competidores base | 20 | 1 | Pendiente | 0 | Cableada |
| Sala Borrego | Expansión Red A | Pendiente | 3 | Pendiente | 1 | WiFi |
| Domo Life | Jueces | 2 | 3 | 56 | 0 | WiFi/cable |
| Domo Escuela de Negocios y Humanidades | Reporteros | 1 | 2 | 34 | 0 | WiFi |
| Auditorio Escuela de Negocios y Humanidades | Entrenadores | 16 | 2 | 12 | 0 | WiFi/cable |
| Sala Menlo | Invitados | Pendiente | Pendiente | Pendiente | Pendiente | Pendiente |

## 3. Equipo requerido

| Elemento | Cantidad | Uso | Observaciones |
| --- | ---: | --- | --- |
| Laptops rentadas | 130 | 120 competidores externos + 10 jueces | Incluyen patch cord y tarjeta WiFi según tabla base. |
| Sillas | 0 renta base | Mobiliario disponible en campus | Se considera que hay sillas suficientes en todos los espacios. |
| APs existentes | 15 observados | Competidores, jueces, reporteros, entrenadores y cámaras | No se cotizan como renta base. |
| Cámaras adicionales/rentadas | 19 base | Refuerzo de vigilancia | Complementan las cámaras observadas según la matriz de ubicación. Se usa costo de referencia de compra equivalente. |
| Switches rentados | 0 base | No requeridos como renta base | Se usa la infraestructura de switching del campus; solo se agregarían switches si TI detecta falta de uplinks o puertos críticos. |
| Nodos Cat 6a nuevos | 0 base | No requeridos como base | Se evita instalación masiva; solo sería puntual si una necesidad física lo exige. |
| Soporte TI | 4 personas por 2 días | Configuración, monitoreo y atención de fallas | Se considera $500 M.N. por persona por día; cada persona equivale a $1,000 M.N. por los dos días. |

### Factores de decisión para seleccionar equipos y materiales

| Equipo o material | Decisión | Factores considerados | Justificación |
| --- | --- | --- | --- |
| Laptops rentadas | Rentar 130 | Capacidad faltante, movilidad, WiFi integrado y disponibilidad para jueces. | Se requieren 120 equipos externos para competidores y 10 equipos para jueces. Al ser laptops con WiFi, pueden conectarse a `OMI-Competidores` o `OMI-Jueces` sin cableado nuevo. |
| APs existentes | Reutilizar 15 observados | Capacidad aproximada de 150 equipos por AP, SSIDs por VLAN y menor intervención física. | Los APs existentes permiten cubrir competidores, jueces, reporteros, entrenadores y cámaras. Sala Borrego tiene 3 APs, suficientes como base para 120 laptops. |
| Switches rentados | 0 renta base | Uso preferente de WiFi, puertos existentes y reducción de costo. | No se requiere conectar masivamente las laptops por cable. Sí se utilizarán los switches existentes del campus para troncales, VLANs, APs y equipos fijos; lo que se elimina de la propuesta base es la renta de switches adicionales. |
| Nodos Cat 6a nuevos | 0 base | Infraestructura existente, EIA/TIA 568 y optimización de recursos. | La propuesta evita instalar nodos nuevos porque los equipos externos se conectan por WiFi y las salas base ya tienen puertos suficientes o conexión híbrida. |
| Cámaras adicionales/rentadas | 19 base | Cobertura de vigilancia, separación en Red C y facilidad de instalación inalámbrica. | Complementan las cámaras observadas y se conectan a `OMI-Camaras` / VLAN 80, sin usar red de invitados ni red de competidores. |
| Core/firewall/router L3 | Usar infraestructura del campus | Enrutamiento inter-VLAN, ACLs, DHCP relay y salida a Internet. | Es necesario para separar redes, aplicar reglas de acceso y controlar la comunicación entre Red A, Red T, Red TI, Red C y redes de usuarios. |
| Servidor e impresoras | Mantener en Red TI | Seguridad, control de acceso e IP fija. | El servidor debe ser accesible desde competidores y jueces, mientras que las impresoras solo deben estar disponibles para jueces/TI. |
| Sillas y mobiliario | 0 renta base | Disponibilidad en espacios y reducción de costos. | El levantamiento considera que hay sillas suficientes, por lo que no se incluye renta de mobiliario como costo base. |

## 4. Ubicación física bajo EIA/TIA 568

La propuesta sigue el criterio de cableado estructurado EIA/TIA 568 en lo necesario: los equipos de red se deben ubicar en puntos protegidos, con recorridos ordenados, etiquetado por VLAN, separación de energía/datos cuando aplique y terminación en rack, IDF o punto de distribución autorizado por TI.

| Elemento | Ubicación propuesta | Criterio |
| --- | --- | --- |
| Core/firewall/router L3 | MDF o cuarto de telecomunicaciones del campus | Concentrar enrutamiento inter-VLAN, ACLs y salida a Internet. |
| APs de Sala Borrego | 3 APs existentes para 120 laptops | Usar `OMI-Competidores` / VLAN 10; colocar AP adicional solo si falta AP físico o cobertura mínima. |
| APs de laboratorios | 1223, 1224, 12102, 12104 y 12401 | Dar cobertura a `OMI-Competidores` y `OMI-Camaras` donde aplique. |
| APs de Domo Life | Domo Life | Dar servicio a `OMI-Jueces` / VLAN 20. |
| APs de reporteros e invitados | Domo ENH, Auditorio ENH y Sala Menlo | Mantener redes separadas por SSID y VLAN. |
| Servidor e impresoras | Red TI / VLAN 30 | Mantener IP fija y acceso solo desde redes autorizadas. |

### Ubicación de cámaras

Las cámaras observadas en sitio se contemplan como cobertura existente. Las cámaras adicionales/rentadas se colocan para completar el criterio de vigilancia de las salas de competencia y reforzar Sala Borrego.

| Lugar | Cámaras observadas | Cámaras adicionales/rentadas propuestas | Total de cobertura | Criterio |
| --- | ---: | ---: | ---: | --- |
| Sala 1223 | 1 | 3 | 4 | Completar 4 puntos de vigilancia. |
| Sala 1224 | 1 | 3 | 4 | Completar 4 puntos de vigilancia. |
| Sala 12102 | 0 | 4 | 4 | Completar 4 puntos de vigilancia. |
| Sala 12104 | 0 | 4 | 4 | Completar 4 puntos de vigilancia. |
| Sala 12401 | 0 | 3 | 3 | Agregar 3 cámaras adicionales/rentadas. |
| Sala Borrego | 1 | 2 | 3 | Refuerzo para expansión Red A con las cámaras adicionales restantes. |

Con esta distribución se usan 19 cámaras adicionales/rentadas como base: 17 para complementar las 5 salas de cómputo y 2 como refuerzo en Sala Borrego. Si el comité exige 4 cámaras también en 12401 o Sala Borrego, se requerirían cámaras adicionales fuera de esta base.

## 5. Optimización de recursos

La optimización principal consiste en aprovechar la infraestructura ya observada:

- Se reutilizan los 144 equipos fijos del campus.
- Se aprovechan APs existentes con capacidad aproximada de 150 equipos por AP.
- 12401 queda cubierto con 20 puertos Ethernet para sus 10 PCs base, sin nodos, switches ni APs adicionales.
- Se evita instalar 60 o 77 nodos Cat 6a como base de la propuesta.
- Las laptops rentadas de Sala Borrego usan WiFi por `OMI-Competidores`.
- Los jueces pueden usar laptops por `OMI-Jueces`, evitando cableado adicional en Domo Life.
- Los switches rentados se dejan en 0 renta base porque no se requieren para conectar masivamente las laptops por cable. La red sí usa switches existentes del campus para troncales, VLANs, APs y equipos fijos; solo se agregaría un switch adicional si TI detecta falta de uplinks o puertos críticos.
- No se renta mobiliario de sillas porque el campus cuenta con sillas suficientes en los espacios.
- Las cámaras existentes se aprovechan como cobertura observada y las adicionales se ubican donde hacen falta.

## 6. Propuesta económica

**Lista de equipos de interconexión y materiales requeridos para la organización de la XXV Olimpiada Mexicana de Informática (OMI).**

| Cantidad | Número de producto | Descripción/Justificación | Costo Unitario | Costo total |
| ---: | --- | --- | ---: | ---: |
| 130 | RENTA-LAPTOP-2D | Renta de laptop por 2 días para 120 competidores externos en Sala Borrego y 10 jueces. Incluye patch cord y tarjeta WiFi. | $1,200 M.N. | $156,000 M.N. |
| 19 | CCTV-223 | Cámaras adicionales/rentadas para vigilancia del evento por SSID `OMI-Camaras` / VLAN 80. Costo de referencia Steren México. | $899 M.N. | $17,081 M.N. |
| 4 | SOPORTE-TI-2D | Soporte T.I. para cuatro personas durante dos días, considerando $500 M.N. por persona por día. | $1,000 M.N. | $4,000 M.N. |
| 0 | SWITCH-RENTA-2D | No se rentan switches adicionales como base; se usan switches existentes del campus para troncales, VLANs, APs y equipos fijos. | $600 M.N. | $0 M.N. |
| 0 | CAT6A-NODO | Nodos Cat 6a nuevos no requeridos como base. Referencia de consigna: $200 USD por nodo si fuera puntual. | $0 M.N. | $0 M.N. |
| 0 | AP-ADICIONAL | APs adicionales no requeridos como base. Sala Borrego cuenta con 3 APs para 120 laptops. | $0 M.N. | $0 M.N. |
| 0 | MOBILIARIO | Sillas y mobiliario existentes suficientes en los espacios; no se renta mobiliario como base. | $0 M.N. | $0 M.N. |
| **Total** | | | | **$177,081 M.N.** |

Notas de costo:

- La renta por evento de cámaras de seguridad requiere cotización personalizada, por lo que se usa un costo de compra equivalente como referencia presupuestal.
- Steren México lista una cámara WiFi CCTV-223 en $899 M.N.; se toma como costo unitario conservador para las 19 cámaras adicionales/rentadas.
- Cyberpuerta muestra un modelo Steren similar con precio mayor, por lo que el monto de $899 M.N. se conserva como estimación conservadora.

Fuentes de referencia:

- Steren México: https://www.steren.com.mx/seguridad/camaras-de-seguridad-wifi
- Cyberpuerta: https://www.cyberpuerta.mx/Seguridad-y-Vigilancia/Camaras-y-Sistemas-de-Vigilancia/Camaras-de-Seguridad-IP/Steren-Camara-de-Seguridad-IP-Smart-WiFi-Box-IR-para-Interiores-CCTV-223-Inalambrico-1920x1080-Full-HD-Dia-Noche.html

## 7. Validación requerida

Antes del evento se debe comprobar:

- DHCP de Red A por cable y por `OMI-Competidores`.
- 120 laptops rentadas conectadas en Sala Borrego con IP de VLAN 10.
- Confirmación de 3 APs disponibles en Sala Borrego; agregar AP solo si falta AP físico o cobertura mínima.
- Acceso de competidores al servidor de concurso.
- Bloqueo de Red A hacia jueces, impresoras, invitados, reporteros, entrenadores, gestión y cámaras.
- Acceso de jueces por `OMI-Jueces` al servidor e impresoras.
- Cobertura de `OMI-Camaras` para cámaras adicionales/rentadas en las salas de cómputo.
- Ubicación física de cámaras observadas y cámaras adicionales/rentadas según la matriz de vigilancia.

## 8. Conclusión

La propuesta seleccionada mantiene el Plan A, pero optimiza la implementación: en lugar de rentar switches e instalar nodos para todos los equipos faltantes, aprovecha APs existentes y laptops con WiFi. El costo preliminar queda en $177,081 M.N., incluyendo laptops, soporte TI y cámaras adicionales/rentadas de referencia. La estrategia disminuye la intervención física en el campus y conserva la segmentación por VLANs necesaria para seguridad y operación.
