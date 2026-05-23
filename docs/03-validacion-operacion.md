# Validacion y operacion OMI

## Objetivo

Definir como comprobar que la red cumple la especificacion y como operarla durante los dos dias de evento.

La validacion cubre:

- Capacidad.
- DHCP.
- VLANs.
- Acceso al servidor.
- Bloqueo entre redes.
- WiFi.
- Camaras inalambricas.
- Impresion.
- Operacion por turnos.
- Respuesta a fallos.

## Matriz de permisos

Politica general: bloquear por defecto y permitir solo lo especificado.

| Origen | Red A | Red T | Red TI servidor | Red TI impresoras | Red Repos | Red E | Red I | Red C | Internet | Red M |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Red A | Local segun politica | No | Si, plataforma | No | No | No | No | No | Si | No |
| Red T | No | Local | Si | Si | No | No | No | No | Si | No |
| Red TI | No iniciado | Si segun respuesta | Local | Local | No | No | No | No | Limitado | No |
| Red Repos | No | No | No | No | Local | No | No | No | Si | No |
| Red E | No | No | Solo scoreboard si aplica | No | No | Local | No | No | Si | No |
| Red I | No | No | No | No | No | No | Local | No | Si | No |
| Red C | No | No | No | No | No | No | No | Local | No | Si, video/monitoreo |
| Red M | Si admin si aplica | Si admin si aplica | Si admin | Si admin | Si admin/AP | Si admin/AP | Si admin/AP | Si monitoreo | Si | Local |

Notas:

- "No iniciado" significa que Red TI no debe iniciar conexiones hacia usuarios salvo respuestas a sesiones permitidas.
- Red M es de administracion y debe estar limitada al equipo tecnico.
- Si el scoreboard se publica para entrenadores, debe ser solo lectura.
- Red C es exclusiva para camaras rentadas inalambricas y solo debe comunicarse con monitoreo autorizado.

## Checklist previo al evento

### Infraestructura

- Confirmar bloque IP final con el campus.
- Configurar VLANs 10, 20, 30, 40, 50, 60, 70 y 80.
- Configurar trunks entre core, switches de acceso, firewall y APs.
- Etiquetar puertos por salon y rol.
- Validar energia para switches, servidor, impresoras, APs y camaras rentadas.
- Confirmar uplinks con capacidad suficiente.

### DHCP y direccionamiento

- Crear scope DHCP para Red A con capacidad mayor a 256 clientes.
- Crear scopes para Red T, Red Repos, Red E, Red I y Red C.
- Asignar IPs fijas a servidor, impresoras y gestion.
- Probar gateway por VLAN.
- Probar DNS por VLAN.
- Confirmar que no hay conflicto con la red del campus.

### Seguridad

- Aplicar politica deny-by-default entre VLANs.
- Permitir Red A hacia servidor de concurso solo en puertos necesarios.
- Permitir Red T hacia servidor e impresoras.
- Bloquear invitados y reporteros hacia redes internas.
- Bloquear redes de usuario hacia Red C.
- Bloquear administracion desde redes de usuario.
- Documentar cualquier excepcion temporal.

### WiFi

- Crear SSIDs `OMI-Reporteros`, `OMI-Entrenadores`, `OMI-Invitados` y `OMI-Camaras`.
- Mapear cada SSID a su VLAN.
- Probar autenticacion.
- Validar cobertura en salas correspondientes.
- Validar cobertura de `OMI-Camaras` en las 5 salas de computo.
- Aplicar limite o prioridad menor a invitados si el equipo lo permite.

## Pruebas de aceptacion

| Prueba | Metodo | Resultado esperado |
| --- | --- | --- |
| DHCP Red A | Conectar clientes de prueba en VLAN 10 | IP `10.50.0.0/23`, gateway correcto |
| Capacidad Red A | Simular o conectar hasta 256 clientes | No se agota el scope |
| Acceso plataforma | Cliente Red A abre servidor | Plataforma responde |
| Bloqueo impresoras | Cliente Red A intenta imprimir | Acceso denegado |
| Acceso jueces | Cliente Red T abre servidor e impresoras | Acceso permitido |
| Bloqueo invitados | Cliente Red I intenta llegar a Red TI | Acceso denegado |
| Reporteros Internet | Cliente Red Repos navega | Internet disponible |
| Entrenadores scoreboard | Cliente Red E consulta resultados | Solo lectura si aplica |
| Gestion | Cliente Red M administra switch/AP | Acceso permitido solo a tecnicos |
| Camaras WiFi | Camara rentada se conecta a `OMI-Camaras` | IP de Red C y video visible solo para monitoreo autorizado |
| Bloqueo Red C | Cliente Red A, Red I o Red Repos intenta llegar a Red C | Acceso denegado |

## Validacion por turno

### Antes de Dia 1 manana: preparatoria

- Confirmar 128 equipos encendidos y conectados.
- Probar 5 equipos aleatorios en Red A.
- Confirmar acceso al servidor de concurso.
- Confirmar bloqueo hacia impresoras y jueces.
- Revisar hora del sistema en servidor y equipos.
- Abrir monitoreo de servidor, firewall y DHCP.

### Cambio Dia 1: preparatoria a secundaria

- Cerrar sesiones de preparatoria.
- Reiniciar o limpiar equipos segun politica del concurso.
- Liberar o renovar leases si es necesario.
- Probar 5 equipos aleatorios.
- Confirmar que secundaria tiene 192 equipos disponibles.
- Validar servidor antes de abrir el turno.

### Antes de Dia 2: primaria

- Confirmar 256 equipos listos.
- Revisar que el scope Red A tenga margen suficiente.
- Probar conectividad desde varios puntos del salon.
- Confirmar que el enlace troncal no este saturado.
- Priorizar Red A y limitar invitados si hace falta.
- Validar plataforma antes del inicio.

## Operacion durante competencia

Monitorear continuamente:

- Uso de DHCP por VLAN.
- Estado del servidor local.
- Latencia desde Red A al servidor.
- Uso de CPU/memoria/disco del servidor.
- Estado de uplinks.
- Clientes conectados por SSID.
- Camaras conectadas al SSID `OMI-Camaras`.
- Eventos de firewall bloqueados/permitidos.
- Estado de impresoras.

Acciones permitidas durante un turno:

- Diagnosticar conectividad de un equipo individual.
- Cambiar cable o puerto manteniendo VLAN correcta.
- Renovar IP de un cliente afectado.
- Reiniciar una impresora si no impacta competencia.
- Aplicar limitacion a Red I si hay congestion.

Acciones que requieren autorizacion del comite:

- Cambiar reglas de acceso de Red A.
- Reiniciar servidor de concurso.
- Cambiar horario de competencia.
- Habilitar acceso nuevo no previsto.
- Mover usuarios entre VLANs.
- Cambiar acceso de Red C o exponer video fuera del monitoreo autorizado.

## Procedimientos de fallo

### DHCP Red A falla

1. Verificar si el problema afecta a un equipo, un switch o toda la VLAN.
2. Revisar scope DHCP y agotamiento de direcciones.
3. Confirmar relay/helper si DHCP esta centralizado.
4. Probar gateway `10.50.0.1`.
5. Escalar si afecta a multiples competidores.

### Servidor local falla

1. Confirmar energia y enlace fisico.
2. Probar acceso desde Red T y Red M.
3. Revisar servicios de plataforma.
4. Avisar a jueces antes de reiniciar.
5. Registrar hora de falla y recuperacion.

### Internet falla

1. Confirmar si Red A todavia llega al servidor local.
2. Validar salida desde firewall.
3. Reducir trafico de Red I si hay degradacion.
4. Escalar al campus si el enlace institucional esta caido.

### Impresora falla

1. Probar ping desde Red T o Red M.
2. Revisar cola de impresion.
3. Cambiar a otra impresora disponible.
4. Mantener impresoras bloqueadas para Red A e invitados.

### WiFi saturado

1. Identificar SSID afectado.
2. Revisar numero de clientes por AP.
3. Limitar Red I antes que Red Repos o Red E.
4. Agregar AP o redistribuir usuarios si hay equipo disponible.

### Camara inalambrica falla

1. Verificar energia o bateria de la camara.
2. Confirmar que la camara este conectada a `OMI-Camaras`.
3. Revisar intensidad de senal en la sala afectada.
4. Reubicar camara o AP si la cobertura es insuficiente.
5. Confirmar que Red C no sea accesible desde redes de usuario.

## Cierre del evento

- Exportar o respaldar logs necesarios.
- Desactivar SSIDs temporales.
- Retirar reglas temporales.
- Liberar scopes DHCP si fueron exclusivos del evento.
- Documentar incidentes y tiempos de respuesta.
- Confirmar que la red del campus vuelve a su estado normal.

## Evidencia recomendada

Guardar capturas o registros de:

- Tabla de VLANs configuradas.
- Scopes DHCP.
- Matriz de reglas firewall/ACL.
- Pruebas de bloqueo entre redes.
- Pruebas de acceso al servidor.
- Pruebas de camaras inalambricas y bloqueo hacia Red C.
- Clientes conectados por turno.
- Incidentes y resoluciones.

## Criterio final de exito

El evento se considera exitoso si los tres turnos completan la competencia sin que la red impida el envio de soluciones, sin fugas de acceso entre roles y sin agotamiento de direccionamiento en el turno maximo de primaria.
