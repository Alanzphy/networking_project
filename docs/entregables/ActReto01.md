# ActReto01 - Identificación de espacios físicos e infraestructura de red para la OMI

## Introducción

El presente documento tiene como objetivo identificar, analizar y justificar los espacios físicos requeridos para el diseño de una infraestructura de red destinada a un evento tipo Olimpiada Mexicana de Informática, tomando como sede el campus. La actividad se centra en reconocer los espacios necesarios, estimar la capacidad de cada uno, proponer dos alternativas de distribución y seleccionar la opción más conveniente con base en criterios de capacidad, separación de roles, seguridad, facilidad de operación y viabilidad técnica.

La propuesta considera la participación de competidores de nivel primaria, secundaria y preparatoria, además de jueces, entrenadores, reporteros, invitados y personal técnico. Debido a que el evento depende directamente de la conectividad de red y de la disponibilidad de equipos de cómputo, la selección de espacios físicos debe responder tanto a necesidades de aforo como a condiciones técnicas de instalación, energía, cableado temporal, segmentación de red y control de acceso.

## Identificación de espacios físicos requeridos

De acuerdo con el análisis del reto, el evento requiere diez espacios físicos operativos mínimos. Cinco de ellos corresponden a las salas de cómputo existentes del campus, que serán utilizadas por los competidores. Además, se requiere un espacio adicional para instalar 112 equipos externos, ya que las salas disponibles no cubren por sí solas la demanda máxima del evento. Finalmente, se requieren cuatro espacios de gran capacidad para jueces, entrenadores, reporteros e invitados.

La competencia contempla un máximo de 256 competidores simultáneos durante la categoría primaria. El campus cuenta con 144 equipos de cómputo fijos distribuidos en cinco salas, por lo que existe una diferencia de 112 equipos. Para cubrir esta necesidad sin modificar el calendario oficial, sin dividir categorías y sin crear olas de participación, se propone integrar dichos equipos externos dentro de la Red A de competidores.

Los espacios requeridos son los siguientes:

| Espacio requerido | Rol asignado | Capacidad requerida |
| --- | --- | ---: |
| Sala 1223 | Competidores base | 40 equipos |
| Sala 1224 | Competidores base | 30 equipos |
| Sala 12102 | Competidores base | 30 equipos |
| Sala 12104 | Competidores base | 34 equipos |
| Sala 12401 | Competidores base | 10 equipos |
| Sala Borrego | Competidores adicionales | 112 equipos externos |
| Domo Life | Jueces | 10 personas |
| Auditorio Escuela de Negocios y Humanidades | Entrenadores | 40 personas |
| Domo Escuela de Negocios y Humanidades | Reporteros | 32 personas |
| Sala Menlo | Invitados | 100 personas |

## Inventario de espacios disponibles

Durante el análisis se consideraron distintos espacios del campus con el propósito de contar con contexto suficiente para tomar decisiones y construir alternativas viables. Entre los espacios identificados se encuentran Sala Menlo, Sala Borrego, Auditorio de la Escuela de Negocios y Humanidades, Auditorio de la Escuela de Ingeniería, Domo de la Escuela de Negocios y Humanidades, Domo Life, Gimnasio, Cancha de americano, Velarias, Cancha de soccer, Plaza Galileo, Plaza Borrego e Innovation Gym PIT3.

No todos los espacios disponibles se utilizan en la propuesta final; sin embargo, se documentan como referencia para evaluar alternativas, contingencias y posibles ajustes operativos.

## Alternativas propuestas

Para cumplir con los requerimientos del evento se plantearon dos alternativas principales. Ambas mantienen las cinco salas de cómputo como base para los competidores y conservan los mismos espacios para jueces, entrenadores, reporteros e invitados. La diferencia principal entre ambas alternativas es el espacio utilizado para instalar los 112 equipos externos requeridos para completar los 256 equipos simultáneos.

### Plan A seleccionado

El Plan A utiliza Sala Borrego como espacio de expansión para los 112 equipos externos. En esta alternativa, la distribución queda de la siguiente manera:

| Rol | Espacio | Cantidad | Red |
| --- | --- | ---: | --- |
| Competidores base | 5 salas de cómputo | 144 equipos | Red A / VLAN 10 |
| Competidores adicionales | Sala Borrego | 112 equipos externos | Red A / VLAN 10 |
| Jueces | Domo Life | 10 personas | Red T / VLAN 20 |
| Entrenadores | Auditorio Escuela de Negocios y Humanidades | 40 personas | Red E / VLAN 50 |
| Reporteros | Domo Escuela de Negocios y Humanidades | 32 personas | Red Repos / VLAN 40 |
| Invitados | Sala Menlo | 100 personas | Red I / VLAN 60 |

### Plan B contingente

El Plan B se conserva como alternativa de respaldo en caso de que Sala Borrego no cumpla con las condiciones físicas necesarias. En esta alternativa, los 112 equipos externos se instalarían en el Auditorio de la Escuela de Ingeniería.

| Rol | Espacio | Cantidad | Red |
| --- | --- | ---: | --- |
| Competidores base | 5 salas de cómputo | 144 equipos | Red A / VLAN 10 |
| Competidores adicionales | Auditorio Escuela de Ingeniería | 112 equipos externos | Red A / VLAN 10 |
| Jueces | Domo Life | 10 personas | Red T / VLAN 20 |
| Entrenadores | Auditorio Escuela de Negocios y Humanidades | 40 personas | Red E / VLAN 50 |
| Reporteros | Domo Escuela de Negocios y Humanidades | 32 personas | Red Repos / VLAN 40 |
| Invitados | Sala Menlo | 100 personas | Red I / VLAN 60 |

## Verificación de capacidad

Con base en la información disponible, las cinco salas de cómputo proporcionan 144 equipos. Esta capacidad es suficiente para cubrir parcialmente la demanda de competidores, pero no alcanza el máximo requerido de 256 equipos simultáneos. Por ello, se requiere un espacio adicional para 112 equipos externos.

Sala Borrego cuenta con un aforo aproximado de 120 personas, por lo que cumple nominalmente con la capacidad necesaria para instalar los 112 equipos externos. No obstante, antes de la implementación final se debe validar físicamente que cuente con mobiliario adecuado, energía suficiente y posibilidad de conectarse a Red A mediante switches y cableado temporal.

El Auditorio de la Escuela de Ingeniería también cuenta con un aforo aproximado de 120 personas, por lo que funciona como alternativa de contingencia. Sin embargo, al ser un espacio tipo auditorio, debe verificarse que su mobiliario permita la instalación adecuada de computadoras.

Los espacios asignados a jueces, entrenadores, reporteros e invitados cumplen con las capacidades requeridas: Domo Life para 10 jueces, Auditorio de Negocios y Humanidades para 40 entrenadores, Domo de Negocios y Humanidades para 32 reporteros y Sala Menlo para 100 invitados.

## Medidas exactas

Las medidas exactas de los espacios seleccionados se encuentran pendientes. Se contempla completarlas con apoyo del plano AutoCAD del campus, solicitado al área correspondiente. Esta información permitirá confirmar dimensiones, distribución de mobiliario, rutas de cableado, ubicación de switches, puntos de energía y posibles trayectorias de instalación temporal.

Mientras no se cuente con dichas medidas, la selección de espacios se justifica con base en aforo aproximado, disponibilidad operativa, separación de roles y viabilidad técnica preliminar.

## Justificación del Plan A

El Plan A fue seleccionado porque permite resolver la necesidad principal del evento: alcanzar 256 equipos simultáneos para la categoría primaria sin modificar el calendario oficial. La Sala Borrego permite concentrar los 112 equipos externos en un solo espacio, lo cual simplifica la instalación de red, la administración de switches, el cableado temporal, las pruebas de DHCP, el monitoreo y el soporte técnico durante la competencia.

Además, el Plan A mantiene separados los roles operativos. Los jueces permanecen en Domo Life, lo que favorece el aislamiento físico necesario para proteger el proceso de evaluación. Los entrenadores se ubican en el Auditorio de la Escuela de Negocios y Humanidades, los reporteros en el Domo de la Escuela de Negocios y Humanidades, y los invitados en Sala Menlo. Esta separación reduce interferencias entre usuarios y facilita la aplicación de VLANs y reglas de acceso.

Desde el punto de vista de red, los 112 equipos externos de Sala Borrego se integran a la Red A mediante VLAN 10, con las mismas restricciones que los demás competidores. Esto significa que podrán acceder al servidor de concurso y a los servicios permitidos, pero no a redes internas de jueces, impresoras, invitados, reporteros, entrenadores o cámaras.

## Justificación del Plan B

El Plan B utiliza el Auditorio de la Escuela de Ingeniería como espacio alternativo para los 112 equipos externos. Esta opción se conserva como contingencia en caso de que Sala Borrego no cumpla con las condiciones físicas de instalación, energía, disponibilidad o conectividad.

La ventaja principal del Plan B es que mantiene intacta la lógica del diseño: no modifica calendario, no divide categorías y conserva los mismos espacios para jueces, entrenadores, reporteros e invitados. Su principal desventaja es que, al tratarse de un auditorio, debe validarse que el mobiliario permita instalar computadoras de manera cómoda y segura para los competidores.

## Comparación y selección de alternativa

| Criterio | Plan A: Sala Borrego | Plan B: Auditorio Ingeniería |
| --- | --- | --- |
| Estado | Seleccionado | Contingencia |
| Capacidad para 112 equipos | Suficiente por aforo nominal | Suficiente por aforo nominal |
| Simplicidad operativa | Alta | Media |
| Riesgo principal | Energía, mesas y cableado temporal | Mobiliario y cableado temporal |
| Impacto en calendario | No cambia | No cambia |
| División de categorías | No requiere | No requiere |
| Separación de roles | Clara | Clara |

Se selecciona el Plan A como alternativa principal, ya que permite instalar los equipos adicionales en un espacio concentrado y mantiene una distribución clara de roles. El Plan B queda documentado como respaldo operativo.

## Bosquejo general de red

La red propuesta se organiza mediante segmentación por VLANs. La Red A corresponde a competidores y utiliza VLAN 10. En ella se conectan las cinco salas de cómputo y la expansión de 112 equipos externos. Los jueces utilizan Red T / VLAN 20, el servidor e impresoras se ubican en Red TI / VLAN 30, los reporteros utilizan Red Repos / VLAN 40, los entrenadores Red E / VLAN 50, los invitados Red I / VLAN 60, la gestión Red M / VLAN 70 y las cámaras inalámbricas rentadas Red C / VLAN 80.

En el Plan A, Sala Borrego se conecta a Red A mediante switches de acceso y cableado temporal. En el Plan B, el mismo esquema se replica en el Auditorio de la Escuela de Ingeniería. En ambos casos, los equipos externos reciben direccionamiento dentro de la subred de Red A y quedan sujetos a las mismas reglas de seguridad que los competidores ubicados en las salas de cómputo.

Los bosquejos completos de cada alternativa se mantienen como anexo en `ActReto01-bosquejos-red.md`. Ese archivo contiene un diagrama Mermaid para el Plan A, un diagrama Mermaid para el Plan B y la leyenda de VLANs utilizada en ambos diseños.


## Conclusión

La propuesta cumple con los requerimientos de la actividad al identificar los espacios físicos necesarios, documentar su capacidad, plantear dos alternativas de distribución, justificar la selección del Plan A y presentar un bosquejo de red para el evento. La solución mantiene el calendario oficial de la competencia, permite atender a 256 competidores simultáneos y conserva una separación clara entre competidores, jueces, entrenadores, reporteros e invitados.

El Plan A, basado en el uso de Sala Borrego para los 112 equipos externos, se considera la alternativa más viable por su capacidad nominal y simplicidad operativa. El Plan B, basado en el Auditorio de la Escuela de Ingeniería, queda como contingencia. La validación final dependerá de confirmar físicamente las medidas, el mobiliario, la energía disponible y las rutas de cableado temporal con apoyo del plano AutoCAD del campus.
