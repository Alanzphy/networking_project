# F001 - Expansion Red A para 112 equipos externos

## Estado

Integrada

## Problema

El campus dispone de 144 equipos de computo fijos en cinco salas, pero el Dia 2 de primaria requiere 256 competidores simultaneos. Faltan 112 equipos para cumplir el calendario oficial sin olas y sin dividir categorias.

## Objetivo

Definir una expansion fisica de Red A para instalar 112 equipos externos adicionales, manteniendo el calendario oficial de dos dias y la competencia completa por categoria.

## Alcance

Incluye:

- Seleccion de Plan A con Sala Borrego para 112 equipos externos.
- Definicion de Plan B contingente con Auditorio Escuela de Ingenieria.
- Integracion de los 112 equipos externos a Red A / VLAN 10.
- Validacion de DHCP, acceso al servidor y bloqueo entre VLANs.

No incluye:

- Compra o renta exacta de equipos.
- Diseno electrico detallado.
- Plano fisico definitivo de mesas y cableado.
- Cambio de calendario, olas o formato de competencia.

## Requerimientos

| ID | Requerimiento | Prioridad |
| --- | --- | --- |
| RF-EXP-01 | Red A debe soportar 144 equipos fijos y 112 equipos externos simultaneos. | Alta |
| RF-EXP-02 | Sala Borrego debe operar como expansion principal de Red A. | Alta |
| RF-EXP-03 | Auditorio Escuela de Ingenieria debe documentarse como contingencia. | Media |
| RNF-EXP-01 | La expansion no debe modificar el calendario oficial ni dividir categorias. | Alta |
| RNF-EXP-02 | Los equipos externos deben quedar aislados igual que cualquier competidor de Red A. | Alta |

## Restricciones

- No hay olas dentro de una categoria.
- Primaria compite completa en Dia 2 con 256 equipos simultaneos.
- Los 112 equipos externos deben conectarse a Red A / VLAN 10.
- Los equipos externos no deben acceder a jueces, impresoras, invitados, reporteros, entrenadores ni camaras.

## Escenarios

### Escenario 1: primaria usa Plan A

Condiciones:

- Las cinco salas de computo tienen 144 equipos disponibles.
- Sala Borrego esta disponible y validada.
- Existen 112 equipos externos aptos para competencia.

Accion:

- El equipo tecnico conecta Sala Borrego a Red A / VLAN 10 con switches y cableado temporal.

Resultado esperado:

- Red A soporta 256 competidores simultaneos.
- Los 112 equipos externos acceden al servidor de concurso.
- Los 112 equipos externos quedan aislados de redes no autorizadas.

### Escenario 2: Sala Borrego no cumple

Condiciones:

- Sala Borrego no cumple energia, mobiliario, conectividad o disponibilidad.

Accion:

- Se activa Plan B con Auditorio Escuela de Ingenieria para los 112 equipos externos.

Resultado esperado:

- El calendario no cambia.
- Red A mantiene capacidad de 256 equipos.

## Impacto en documentos base

| Documento | Cambio requerido |
| --- | --- |
| `00-fuente-de-verdad.md` | Registrar Sala Borrego como Plan A para 112 equipos externos. |
| `01-arquitectura-red.md` | Incluir Sala Borrego como extension de Red A / VLAN 10. |
| `02-eventos-escenarios.md` | Sin cambio requerido; los equipos externos son competidores Red A. |
| `03-validacion-operacion.md` | Agregar pruebas de Sala Borrego y contingencia Auditorio Ingenieria. |
| `04-espacios-fisicos.md` | Documentar Plan A seleccionado y Plan B contingente. |

## Validacion

- Confirmar `144 + 112 = 256` equipos en Red A.
- Probar DHCP Red A en Sala Borrego.
- Probar acceso al servidor de concurso desde equipos externos.
- Probar bloqueo hacia Red T, Red TI impresoras, Red Repos, Red E, Red I y Red C.
- Confirmar energia, mobiliario, switches, uplinks y cableado temporal antes del Dia 2.

## Decisiones pendientes

- Validacion fisica final de energia, mobiliario y cableado temporal en Sala Borrego.
- Validacion fisica equivalente para Auditorio Escuela de Ingenieria como Plan B.

## Supuestos

- Los 112 equipos externos seran computadoras aptas para competencia.
- Sala Borrego tiene aforo aproximado de 120 personas, suficiente por capacidad nominal.
- Auditorio Escuela de Ingenieria tiene aforo aproximado de 120 personas y funciona como contingencia.
