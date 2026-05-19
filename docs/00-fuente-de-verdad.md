# Fuente de verdad SDD - Infraestructura de red OMI

## Proposito

Este documento es la fuente de verdad principal para el diseno de red de la Olimpiada Mexicana de Informatica en campus sede. Aplica Spec Driven Development en formato Markdown: primero se define el comportamiento esperado del evento, despues se derivan arquitectura, restricciones, escenarios y validaciones.


- `00-fuente-de-verdad.md`: conteos, decisiones, calendario, requerimientos y supuestos.
- `01-arquitectura-red.md`: VLANs, subredes, topologia logica, direccionamiento y reglas.
- `02-eventos-escenarios.md`: usuarios, eventos, escenarios, restricciones y flujos.
- `03-validacion-operacion.md`: pruebas, matriz de acceso, checklist operativo y fallos esperados.

## Alcance

La infraestructura debe soportar un evento presencial con participantes de los 32 estados de Mexico, equipos de competencia del campus, roles operativos separados y redes inalambricas para usuarios no criticos.

Incluye:

- Segmentacion por tipo de usuario.
- Direccionamiento IP por subred.
- VLANs por rol.
- Acceso controlado al servidor local de concurso.
- Salida a Internet para roles permitidos.
- WiFi separado para reporteros, entrenadores e invitados.
- Operacion durante dos dias de competencia.
- Validacion tecnica antes y durante el evento.

No incluye:

- Compra exacta de switches, access points o cableado fisico.
- Configuracion especifica por marca de equipo.
- Desarrollo de la plataforma de concurso.
- Politicas institucionales internas del campus fuera de la red del evento.

## Conteo oficial

El modelo oficial usa 32 estados parejos. No se aplica el modelo alternativo de estado sede al doble.

| Grupo | Formula | Total |
| --- | ---: | ---: |
| Preparatoria | 32 estados x 4 alumnos | 128 |
| Secundaria | 32 estados x 6 alumnos | 192 |
| Primaria | 32 estados x 8 alumnos | 256 |
| Competidores totales | 128 + 192 + 256 | 576 |
| Jueces | Fijo | 10 |
| Servidor local | Fijo | 1 |
| Impresoras | Fijo | 4 |
| Reporteros | 1 por estado | 32 |
| Entrenadores | Fijo | 40 |
| Invitados | Aforo reservado | 100 |

La capacidad maxima simultanea de competencia es de 256 equipos, porque primaria participa completa en el segundo dia.

## Calendario oficial

| Dia | Horario | Categoria | Competidores | Equipos requeridos |
| --- | --- | --- | ---: | ---: |
| Dia 1 | Manana | Preparatoria | 128 | 128 |
| Dia 1 | Tarde | Secundaria | 192 | 192 |
| Dia 2 | Jornada de competencia | Primaria | 256 | 256 |

Restricciones del calendario:

- No hay olas dentro de una misma categoria.
- Cada categoria compite completa en su horario.
- La red de competidores debe soportar el peor caso de 256 equipos simultaneos.
- Entre turnos debe existir tiempo de limpieza logica: reinicio de equipos, revision de conectividad, liberacion de sesiones y validacion del servidor.

## Decisiones oficiales

| Decision | Valor oficial | Razon |
| --- | --- | --- |
| Formato de especificacion | Markdown | Facil de leer, versionar y presentar. |
| Conteo de estados | 32 estados parejos | Coincide con la decision del proyecto. |
| Dias de evento | 2 dias | Preparatoria y secundaria en Dia 1; primaria en Dia 2. |
| Capacidad maxima Red A | 256 equipos | Primaria requiere 256 computadoras simultaneas. |
| Red A | Subred `/23` | Un `/24` solo ofrece 254 hosts utiles y no alcanza. |
| Bloque base sugerido | `10.50.0.0/22` | Permite segmentar el evento completo en subredes internas. |
| Servidor local | Plataforma de concurso | Competidores acceden al servicio; jueces administran/evaluan. |
| Separacion de roles | VLAN por rol | Reduce riesgos y facilita reglas de acceso. |

## Requerimientos funcionales

RF-01. Los competidores deben conectarse por cable a la Red A durante su turno.

RF-02. La Red A debe soportar al menos 256 equipos simultaneos.

RF-03. Los competidores deben poder acceder al servidor local de concurso.

RF-04. Los competidores deben tener salida a Internet si la plataforma o recursos autorizados lo requieren.

RF-05. Los jueces deben contar con 10 equipos en Red T.

RF-06. Los jueces deben acceder al servidor local y a las 4 impresoras.

RF-07. El servidor local y las impresoras deben estar en una red protegida de TI.

RF-08. Los reporteros deben conectarse por WiFi a una red separada y tener salida a Internet.

RF-09. Los entrenadores deben contar con conectividad mixta o WiFi suficiente para 40 usuarios.

RF-10. Los invitados deben usar una red WiFi separada solo para Internet.

RF-11. La red de gestion debe permitir administrar switches, access points y firewall.

## Requerimientos no funcionales

RNF-01. Seguridad: cada rol debe estar aislado en su propia VLAN y subred.

RNF-02. Disponibilidad: el servicio de red debe permanecer activo durante cada turno de competencia.

RNF-03. Desempeno: el envio de soluciones no debe degradarse por trafico de invitados, prensa o entrenadores.

RNF-04. Control: el trafico entre VLANs debe pasar por firewall o dispositivo capa 3 con ACLs.

RNF-05. Operabilidad: el equipo tecnico debe poder diagnosticar DHCP, gateway, DNS, servidor y salida a Internet.

RNF-06. Escalabilidad limitada: las subredes deben reservar margen para gateway, monitoreo y crecimiento razonable.

RNF-07. Integracion campus: el bloque IP final no debe chocar con la red institucional.

## Restricciones principales

- Red A no puede compartir subred con invitados, reporteros, entrenadores ni jueces.
- Red A no puede usar `/24`; necesita una subred con mas de 256 hosts utiles.
- El servidor local no debe quedar en la misma VLAN que competidores.
- Las impresoras no deben ser visibles desde competidores, reporteros, entrenadores o invitados.
- Las redes WiFi deben mapearse a VLANs separadas.
- Invitados y reporteros no deben acceder a redes internas del evento.
- La administracion de red debe estar separada en una VLAN de gestion.

## Especificacion resumida

| Red | Rol | Tipo | Capacidad objetivo | Criticidad |
| --- | --- | --- | ---: | --- |
| Red A | Competidores | Cableada | 256 | Alta |
| Red T | Jueces | Cableada | 10 | Alta |
| Red TI | Servidor e impresoras | Cableada | 5+ | Alta |
| Red Repos | Reporteros | WiFi | 32 | Media |
| Red E | Entrenadores | Mixta/WiFi | 40 | Media |
| Red I | Invitados | WiFi | 100 | Baja |
| Red M | Gestion | Cableada/administrativa | 20+ | Alta |

## Criterios de aceptacion

La infraestructura queda aceptada cuando:

- Red A entrega direccion IP valida a 256 equipos simultaneos.
- Los competidores pueden acceder al servidor de concurso.
- Los competidores no pueden acceder a jueces, impresoras, gestion, prensa, entrenadores o invitados.
- Los jueces pueden acceder al servidor e impresoras.
- Reporteros, entrenadores e invitados reciben conectividad segun su rol.
- La matriz de permisos esta validada antes del primer turno.
- El calendario de dos dias no rebasa la capacidad maxima de equipos.
- Existe checklist operativo para inicio, cambio y cierre de turnos.

## Supuestos

- El campus puede prestar 256 computadoras cableadas simultaneas.
- Hay switches y puertos suficientes para conectar la Red A.
- La red troncal del campus permite transportar las VLANs requeridas.
- El bloque `10.50.0.0/22` puede cambiar si ya existe conflicto.
- La plataforma de concurso estara en el servidor local.
- El equipo tecnico del campus puede configurar VLANs, DHCP, ACLs y WiFi.
