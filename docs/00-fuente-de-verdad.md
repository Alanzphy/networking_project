# Fuente de verdad SDD - Infraestructura de red OMI

## Proposito

Este documento es la fuente de verdad principal para el diseno de red de la Olimpiada Mexicana de Informatica en campus sede. Aplica Spec Driven Development en formato Markdown: primero se define el comportamiento esperado del evento, despues se derivan arquitectura, restricciones, escenarios y validaciones.

## Contexto del reto

El TEC de Monterrey se prepara para ser sede de la Olimpiada Mexicana de Informatica y requiere disenar, configurar e interconectar una nueva red local a la infraestructura de red actual del campus sin comprometer la continuidad de operacion.

La red del evento debe ser compatible con la red institucional, usar direcciones IP autorizadas, respetar restricciones tecnicas del campus y evitar interferencias con los servicios existentes. Por eso se plantea una red paralela y segmentada que se integra a la infraestructura actual mediante enlaces controlados, VLANs, reglas de acceso y validacion previa.

TI Nacional del Tecnologico de Monterrey funge como socio formador y entidad de apoyo para proporcionar informacion clave: bloques IP permitidos, restricciones de conectividad, condiciones para interconexion y lineamientos tecnicos que debe observar el diseno antes de ponerse en produccion.

## Documentos base

- `00-fuente-de-verdad.md`: conteos, decisiones, calendario, requerimientos y supuestos.
- `01-arquitectura-red.md`: VLANs, subredes, topologia logica, direccionamiento y reglas.
- `02-eventos-escenarios.md`: usuarios, eventos, escenarios, restricciones y flujos.
- `03-validacion-operacion.md`: pruebas, matriz de acceso, checklist operativo y fallos esperados.

## Instalaciones del campus

### Salas de computo

Los cinco salones con equipo de computo fijo disponibles para la competencia son:

| Sala | Equipos de computo | Sistema | Puertos Ethernet actuales | APs | Tomas de corriente | Camaras observadas | Conexion propuesta |
| --- | --- | --- | ---: | ---: | ---: | ---: | --- |
| 1223 | 40 PC | Windows 11 | 43 | 1 | 9 | 1 | Cableada |
| 1224 | 30 iMac | macOS | 48 | 1 | 24 | 1 | WiFi preferente |
| 12102 | 30 PC Workstation | Windows 11 | 40 | 1 | 43 | 0 | Cableada |
| 12104 (Lab de moviles) | 15 PC Workstation + 19 MacBook Pro | Windows 11 / macOS | 8 | 1 | 16 | 0 | Hibrida |
| 12401 (Lab de redes) | 10 PC | Windows 11 | 20 | 1 | Pendiente | 0 | Cableada |
| **Total de equipos** | **144 equipos** | | | | | | |

Notas:

- Solo se contabilizan equipos de computo utiles para la competencia: PC, iMac y MacBook.
- El levantamiento fisico actualizado reemplaza el conteo anterior de nodos para las salas medidas.
- La Red A se operara de forma hibrida: cable donde ya exista infraestructura suficiente y WiFi donde sea mas conveniente evitar instalacion nueva.
- Cada AP se considera con capacidad aproximada de 150 equipos para dimensionar si hacen falta APs adicionales.
- 12401 queda cerrado para conectividad del Plan A: sus 20 puertos Ethernet cubren sus 10 PCs base y cuenta con 1 AP disponible.
- El campus dispone de 144 equipos de computo, inferior al maximo requerido de 264 para primaria. La diferencia de 120 equipos se cubre con equipo externo en Sala Borrego como Plan A; Auditorio Escuela de Ingenieria queda como Plan B contingente.

### Espacios de gran capacidad disponibles

| Espacio | Aforo aproximado | Uso previsto actual |
| --- | ---: | --- |
| Sala Menlo | ~300 personas | Invitados |
| Sala Borrego | ~120 personas | Expansion Red A / Plan A |
| Auditorio Escuela de Negocios y Humanidades | ~150 personas | Entrenadores |
| Auditorio Escuela de Ingenieria | ~120 personas | Expansion Red A / Plan B |
| Domo Escuela de Negocios y Humanidades | Variable | Reporteros |
| Domo Life | Variable | Jueces |
| Gimnasio | Variable | Disponible / por definir |
| Cancha de americano | Variable | Disponible / por definir |
| Velarias | Variable | Disponible / por definir |
| Cancha de soccer | Variable | Disponible / por definir |
| Plaza Galileo | Variable | Disponible / por definir |
| Plaza Borrego | Variable | Disponible / por definir |
| Innovation Gym | ~80 personas | Disponible / por definir |

Nota: el inventario completo de espacios se conserva para dar contexto a futuras decisiones, alternativas y planes operativos.

### Levantamiento fisico complementario

| Espacio | Uso previsto | Puertos Ethernet actuales | APs | Tomas de corriente | Camaras observadas | Estado |
| --- | --- | ---: | ---: | ---: | ---: | --- |
| Sala Borrego | Expansion Red A / Plan A | Pendiente | 3 | Pendiente | 1 | Medido parcialmente |
| Domo Life | Jueces | 2 | 3 | 56 | 0 | Medido |
| Domo Escuela de Negocios y Humanidades | Reporteros | 1 | 2 | 34 | 0 | Medido |
| Auditorio Escuela de Negocios y Humanidades | Entrenadores | 16 | 2 | 12 | 0 | Medido |
| Auditorio Escuela de Ingenieria | Expansion Red A / Plan B | Pendiente | Pendiente | Pendiente | Pendiente | Pendiente |
| Sala Menlo | Invitados | Pendiente | Pendiente | Pendiente | Pendiente | Pendiente |

### Camaras de vigilancia

Se contemplan camaras existentes observadas y hasta 20 camaras adicionales/rentadas para el evento. Las camaras adicionales deben ser inalambricas para facilitar la instalacion y conectarse al SSID dedicado `OMI-Camaras`, mapeado a Red C (VLAN 70), separada de las redes de competencia, invitados, reporteros y entrenadores.

| Lugar | Camaras observadas | Camaras adicionales/rentadas propuestas | Total de cobertura | Criterio |
| --- | ---: | ---: | ---: | --- |
| Sala 1223 | 1 | 3 | 4 | Completar 4 puntos de vigilancia. |
| Sala 1224 | 1 | 3 | 4 | Completar 4 puntos de vigilancia. |
| Sala 12102 | 0 | 4 | 4 | Completar 4 puntos de vigilancia. |
| Sala 12104 | 0 | 4 | 4 | Completar 4 puntos de vigilancia. |
| Sala 12401 | 0 | 3 | 3 | Agregar 3 camaras adicionales/rentadas. |
| Sala Borrego | 1 | 2 | 3 | Refuerzo para expansion Red A con las camaras adicionales restantes. |

Con esta distribucion se usan hasta 19 camaras adicionales/rentadas: 17 para complementar las 5 salas de computo y 2 como refuerzo en Sala Borrego. Si se exige cobertura de 4 camaras tambien en 12401 o Sala Borrego, se requeririan camaras adicionales fuera de esta base.

---

## Alcance

La infraestructura debe soportar un evento presencial con participantes de los 32 estados de Mexico (31 estados regulares y 1 estado sede con contingente doble), equipos de competencia del campus, roles operativos separados y redes inalambricas para usuarios no criticos.

Incluye:

- Segmentacion por tipo de usuario.
- Direccionamiento IP por subred.
- VLANs por rol.
- Acceso controlado al servidor local de concurso.
- Salida a Internet para roles permitidos.
- WiFi separado para competidores, jueces, reporteros, entrenadores, invitados y camaras adicionales/rentadas.
- Operacion durante dos dias de competencia.
- Validacion tecnica antes y durante el evento.

No incluye:

- Compra exacta de switches, access points o cableado fisico.
- Configuracion especifica por marca de equipo.
- Desarrollo de la plataforma de concurso.
- Politicas institucionales internas del campus fuera de la red del evento.

## Conteo oficial

El modelo oficial usa 31 estados regulares con contingente normal y 1 estado sede con contingente doble, sumando 32 estados en total.

- Estados regulares (31): 4 alumnos de prepa, 6 de secundaria, 8 de primaria.
- Estado sede (1): 8 alumnos de prepa, 12 de secundaria, 16 de primaria.

| Grupo | Formula | Total |
| --- | ---: | ---: |
| Preparatoria | (31 x 4) + 8 | 132 |
| Secundaria | (31 x 6) + 12 | 198 |
| Primaria | (31 x 8) + 16 | 264 |
| Competidores totales | 132 + 198 + 264 | 594 |
| Jueces | Fijo | 10 |
| Servidor local | Fijo | 1 |
| Impresoras | Fijo | 4 |
| Reporteros | 1 por estado | 32 |
| Entrenadores | Fijo | 40 |
| Invitados | Aforo reservado | 100 |

La capacidad maxima simultanea de competencia es de 264 equipos, porque primaria participa completa en el segundo dia.

## Calendario oficial

| Dia | Horario | Categoria | Competidores | Equipos requeridos |
| --- | --- | --- | ---: | ---: |
| Dia 1 | Manana | Preparatoria | 132 | 132 |
| Dia 1 | Tarde | Secundaria | 198 | 198 |
| Dia 2 | Jornada de competencia | Primaria | 264 | 264 |

Restricciones del calendario:

- No hay olas dentro de una misma categoria.
- Cada categoria compite completa en su horario.
- La red de competidores debe soportar el peor caso de 264 equipos simultaneos.
- Entre turnos debe existir tiempo de limpieza logica: reinicio de equipos, revision de conectividad, liberacion de sesiones y validacion del servidor.

## Decisiones oficiales

| Decision | Valor oficial | Razon |
| --- | --- | --- |
| Formato de especificacion | Markdown | Facil de leer, versionar y presentar. |
| Conteo de estados | 31 estados regulares + 1 estado sede con contingente doble | El estado sede tiene el doble de competidores por categoria. |
| Dias de evento | 2 dias | Preparatoria y secundaria en Dia 1; primaria en Dia 2. |
| Capacidad maxima Red A | 264 equipos | Primaria requiere 264 computadoras simultaneas. |
| Red A | Subred `/23` | Un `/24` solo ofrece 254 hosts utiles y no alcanza. |
| Bloque base sugerido | `172.23.8.0/21` | Permite segmentar las VLANs internas del evento. |
| Enlace WAN | `10.32.100.0/30` | Enlace RTFrontera - FCampus fuera del bloque interno `/21`. |
| Servidor local | Plataforma de concurso | Competidores acceden al servicio; jueces administran/evaluan. |
| Separacion de roles | VLAN por rol | Reduce riesgos y facilita reglas de acceso. |
| Integracion con campus | Validar con TI Nacional/campus | El bloque IP, las restricciones y la interconexion no deben afectar la continuidad operativa institucional. |
| Camaras de vigilancia | Observadas + adicionales/rentadas inalambricas, Red C / VLAN 70, subred `/27` | Aisladas de redes de competencia y usuarios; el video se consulta desde acceso tecnico autorizado fuera de VLANs de usuario. |
| Espacios del evento | Inventario completo registrado | Se conserva para apoyar decisiones, alternativas y planes operativos. |
| Expansion Red A | Sala Borrego como Plan A; Auditorio Ingenieria como Plan B | Permite cubrir 120 equipos externos sin cambiar calendario ni dividir categorias. |
| Red A hibrida | Cable + SSID `OMI-Competidores` en VLAN 10 | Optimiza recursos y evita instalacion masiva de nodos Cat 6a. |
| Red T hibrida | Cable + SSID `OMI-Jueces` en VLAN 20 | Permite operar jueces con laptops sin cableado adicional. |
| VLAN 99 Administracion | `172.23.11.64/28` | Segmento tecnico para controladora de Packet Tracer; no es red de usuarios. |
| Capacidad AP | ~150 equipos por AP | Supuesto operativo para dimensionar si hacen falta APs adicionales. |
| Mobiliario | Sillas existentes suficientes | No se renta mobiliario de sillas como base de costos. |

## Requerimientos funcionales

RF-01. Los competidores deben conectarse a la Red A durante su turno por cable o por el SSID `OMI-Competidores`, segun el espacio.

RF-02. La Red A debe soportar al menos 264 equipos simultaneos.

RF-03. Los competidores deben poder acceder al servidor local de concurso.

RF-04. Los competidores deben tener salida a Internet si la plataforma o recursos autorizados lo requieren.

RF-05. Los jueces deben contar con 10 equipos en Red T por cable o por el SSID `OMI-Jueces`.

RF-06. Los jueces deben acceder al servidor local y a las 4 impresoras.

RF-07. El servidor local y las impresoras deben estar en una red protegida de TI.

RF-08. Los reporteros deben conectarse por WiFi a una red separada y tener salida a Internet.

RF-09. Los entrenadores deben contar con conectividad mixta o WiFi suficiente para 40 usuarios.

RF-10. Los invitados deben usar una red WiFi separada solo para Internet.

RF-11. La administracion de switches, access points y firewall debe realizarse fuera de las VLANs de usuario del evento.

RF-12. Las camaras adicionales/rentadas deben conectarse por WiFi dedicado a Red C para vigilancia de las salas de computo.

RF-13. Los 120 equipos externos deben conectarse a Red A / VLAN 10 desde Sala Borrego en Plan A, preferentemente por WiFi.

RF-14. La controladora de Packet Tracer debe ubicarse en VLAN 99 Administracion, usando el bloque `172.23.11.64/28`.

## Requerimientos no funcionales

RNF-01. Seguridad: cada rol debe estar aislado en su propia VLAN y subred.

RNF-02. Disponibilidad: el servicio de red debe permanecer activo durante cada turno de competencia.

RNF-03. Desempeno: el envio de soluciones no debe degradarse por trafico de invitados, prensa o entrenadores.

RNF-04. Control: el trafico entre VLANs debe pasar por firewall o dispositivo capa 3 con ACLs.

RNF-05. Operabilidad: el equipo tecnico debe poder diagnosticar DHCP, gateway, DNS, servidor y salida a Internet.

RNF-06. Escalabilidad limitada: las subredes deben reservar margen para gateway, monitoreo y crecimiento razonable.

RNF-07. Integracion campus: el bloque IP final no debe chocar con la red institucional.

RNF-08. Continuidad operativa: la red paralela del evento no debe degradar ni interrumpir la operacion normal de la red del campus.

RNF-09. Administracion tecnica: VLAN 99 debe quedar aislada de redes de usuario y accesible solo por equipo tecnico autorizado.

## Restricciones principales

- Red A no puede compartir subred con invitados, reporteros, entrenadores ni jueces.
- Red A no puede usar `/24`; necesita una subred con mas de 254 hosts utiles.
- El servidor local no debe quedar en la misma VLAN que competidores.
- Las impresoras no deben ser visibles desde competidores, reporteros, entrenadores o invitados.
- Las redes WiFi deben mapearse a VLANs separadas.
- Invitados y reporteros no deben acceder a redes internas del evento.
- La administracion de red no debe exponerse en ninguna VLAN de usuario del evento.
- VLAN 99 no debe usarse para competidores, jueces, reporteros, entrenadores, invitados, servidor, impresoras ni camaras.
- Las camaras inalambricas deben usar un SSID dedicado y no deben compartir red con invitados, reporteros, entrenadores o competidores.
- La expansion de 120 equipos externos no debe modificar el calendario oficial ni dividir categorias.
- `OMI-Competidores` debe mapearse exclusivamente a Red A / VLAN 10.
- `OMI-Jueces` debe mapearse exclusivamente a Red T / VLAN 20.

## Especificacion resumida

| Red | Rol | Tipo | Capacidad objetivo | Criticidad |
| --- | --- | --- | ---: | --- |
| Red A | Competidores | Hibrida cable/WiFi | 264 | Alta |
| Red T | Jueces | Hibrida cable/WiFi | 10 | Alta |
| Red TI | Servidor e impresoras | Cableada | 5 | Alta |
| Red Repos | Reporteros | WiFi | 32 | Media |
| Red E | Entrenadores | Mixta/WiFi | 40 | Media |
| Red I | Invitados | WiFi | 100 | Baja |
| Red C | Camaras de monitoreo del evento | WiFi | Hasta 20 | Media |
| Administracion | Controladora Packet Tracer | Cableada/tecnica | 14 IP utiles | Alta |

## Criterios de aceptacion

La infraestructura queda aceptada cuando:

- Red A entrega direccion IP valida a 264 equipos simultaneos.
- `OMI-Competidores` entrega direccion IP de Red A / VLAN 10 a equipos autorizados.
- Sala Borrego entrega conectividad Red A a 120 equipos externos en Plan A por WiFi.
- Los competidores pueden acceder al servidor de concurso.
- Los competidores no pueden acceder a jueces, impresoras, prensa, entrenadores, invitados o camaras.
- Los jueces pueden acceder al servidor e impresoras desde Red T cableada o `OMI-Jueces`.
- Reporteros, entrenadores e invitados reciben conectividad segun su rol.
- La matriz de permisos esta validada antes del primer turno.
- El calendario de dos dias no rebasa la capacidad maxima de equipos.
- Las camaras adicionales/rentadas reciben conectividad por `OMI-Camaras` sin ser accesibles desde redes de usuario.
- VLAN 99 Administracion permite operar la controladora de Packet Tracer sin ser accesible desde redes de usuario.
- Existe checklist operativo para inicio, cambio y cierre de turnos.

## Supuestos

- El campus dispone de 144 equipos de computo fijos en 5 salones; los 120 equipos restantes para cubrir el maximo de 264 provienen de equipo externo en Sala Borrego como Plan A.
- Auditorio Escuela de Ingenieria queda como Plan B contingente para los mismos 120 equipos externos si Sala Borrego no cumple.
- Las laptops rentadas tienen WiFi funcional.
- Existen sillas suficientes en los espacios del evento; no se cotiza renta de sillas como base.
- Los APs existentes pueden mapear SSIDs a VLANs o ser configurados por TI.
- Cada AP soporta aproximadamente 150 equipos para estimar si hacen falta APs adicionales.
- No se contemplan nodos Cat 6a nuevos como base de la propuesta; solo se agregarian si una necesidad fisica puntual lo vuelve necesario.
- La red troncal del campus permite transportar las VLANs requeridas incluyendo VLAN 70 para camaras adicionales/rentadas.
- El bloque `172.23.8.0/21` puede cambiar si ya existe conflicto.
- La plataforma de concurso estara en el servidor local.
- El equipo tecnico del campus puede configurar VLANs, DHCP, ACLs y WiFi.
- TI Nacional/campus proporciona o valida los bloques IP permitidos, restricciones de conectividad y condiciones de interconexion.
- VLAN 99 Administracion usa `172.23.11.64/28`; gateway sugerido `172.23.11.65` y controladora sugerida `172.23.11.66`.
- Las camaras adicionales/rentadas requieren cobertura WiFi dedicada por `OMI-Camaras`.
- 12401 no tiene camaras existentes; se agregan 3 camaras adicionales/rentadas para ese salon.
- Sala Menlo y Auditorio Escuela de Ingenieria requieren levantamiento fisico actualizado.
