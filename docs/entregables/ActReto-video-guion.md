# Guion de video - Solucion de red para la OMI

**Duracion objetivo:** 4 minutos  
**Participantes:** 3 personas  
**Formato:** explicacion por turnos, con apoyo visual del diagrama en Packet Tracer y documentacion SDD.

---

## Preguntas seleccionadas para el video

Para el video se responderan las preguntas que mejor demuestran la solucion tecnica:

1. Objetivo del proyecto y razon de la red paralela.
2. Diagrama logico y fisico de la solucion.
3. Tecnologias usadas: VLANs, trunks, inter-VLAN routing, WiFi y cableado.
4. Aislamiento entre la red existente y la red del evento.
5. Plan de direccionamiento IP.
6. Criterios para declarar lista la red.
7. Soporte durante la competencia.
8. Demostracion rapida ante un juez.
9. Retos, lecciones aprendidas y aportes.

---

## Guion

### Persona 1 - Introduccion, objetivo y diseno general

Hola, somos el equipo encargado de proponer la infraestructura de red para la Olimpiada Mexicana de Informatica en el TEC de Monterrey como campus sede.

El objetivo principal fue disenar, configurar e interconectar una red local capaz de soportar hasta **264 competidores simultaneos**, ademas de jueces, servidor, impresoras, reporteros, entrenadores, invitados y camaras de monitoreo. Se decidio implementar una **red de datos paralela** sobre la infraestructura existente porque no era conveniente mezclar el trafico del evento con la red normal del campus. Separar la red permite controlar seguridad, evitar interferencias y proteger la continuidad operativa.

La solucion se basa en una topologia segmentada. En Packet Tracer se representa un **router o firewall de capa 3**, conectado al core o switch principal. Desde ahi salen las VLANs del evento: competidores en **VLAN 10**, jueces en **VLAN 20**, servidor e impresoras en **VLAN 30**, reporteros en **VLAN 40**, entrenadores en **VLAN 50**, invitados en **VLAN 60**, camaras en **VLAN 70** y administracion tecnica en **VLAN 99** para la controladora. Los bloques IP y restricciones deben validarse con TI Nacional/campus antes de produccion.

Fisicamente, se aprovechan las salas de computo existentes, que aportan 144 equipos. Para completar primaria, se usa **Sala Borrego** con 120 laptops por WiFi en el SSID `OMI-Competidores`.

---

### Persona 2 - Tecnologias, aislamiento y direccionamiento

Las principales tecnologias usadas fueron **VLANs, enlaces troncales, enrutamiento inter-VLAN controlado, WiFi por SSID separado y cableado UTP donde ya existe infraestructura**.

Las VLANs se eligieron porque separan usuarios por rol. Esto evita que un competidor se comunique directamente con jueces, impresoras, invitados o camaras. Los enlaces troncales transportan varias VLANs entre switches, APs y el equipo de capa 3. El inter-VLAN routing se controla con firewall o ACLs, permitiendo solo los flujos necesarios.

Por ejemplo, Red A puede llegar al servidor de concurso, pero no a impresoras ni a jueces. Red T si puede acceder al servidor y a impresoras. Invitados y reporteros solo tienen Internet. Las camaras estan aisladas en VLAN 70 y no son alcanzables desde redes de usuario.

El direccionamiento usa el bloque interno `172.23.8.0/21`. La red mas grande es competidores: `172.23.8.0/23`, porque un `/24` solo da 254 hosts utiles y no alcanza para 264 equipos. Otros ejemplos son `172.23.11.32/28` para jueces, `172.23.11.48/29` para servidor e impresoras, `172.23.11.0/27` para camaras y `172.23.11.64/28` para administracion.

---

### Persona 3 - Validacion, soporte, demostracion y cierre

Para declarar lista la red, cada VLAN debe entregar una IP del rango correcto. Los competidores deben entrar al servidor de concurso y quedar bloqueados hacia jueces, impresoras, invitados, reporteros, entrenadores y camaras. Tambien se valida que `OMI-Competidores` entregue IP de VLAN 10, que `OMI-Jueces` entregue IP de VLAN 20 y que las camaras usen `OMI-Camaras` en VLAN 70.

Durante la competencia, el soporte monitorea DHCP, servidor, firewall, APs, clientes por SSID, impresoras y enlaces troncales. Si falla DHCP, se revisa scope, gateway y relay; si falla WiFi, clientes por AP; si hay congestion, se limita primero la red de invitados antes de afectar Red A.

Si un juez pidiera demostrar la capacidad en dos minutos, mostrariamos tres cosas en Packet Tracer: la tabla de VLANs y subredes, una prueba de conectividad desde un competidor al servidor, y una prueba de bloqueo hacia impresoras o camaras. Con eso se demuestra capacidad, funcionamiento y seguridad.

Uno de los retos principales fue cubrir 264 competidores cuando el campus solo tenia 144 equipos fijos. La solucion fue usar Sala Borrego con 120 laptops por WiFi, evitando instalar nodos nuevos masivos.

Como documentacion, entregamos fuente de verdad SDD, arquitectura, escenarios, validacion, bosquejos, propuesta economica y guion de video. Nuestro aporte mas significativo fue convertir los requerimientos del evento en una solucion representable en Packet Tracer, manteniendo coherencia entre topologia, VLANs, direccionamiento, seguridad y operacion real.

---

## Apoyos visuales sugeridos

- Mostrar el diagrama logico de Packet Tracer.
- Acercar la vista a las VLANs principales.
- Mostrar la Red A con salas de computo y Sala Borrego.
- Mostrar el router/firewall o switch capa 3.
- Mostrar una prueba exitosa hacia el servidor.
- Mostrar una prueba bloqueada hacia una red no autorizada.
