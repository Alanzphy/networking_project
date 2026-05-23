# ADR-002 - Conteo diferenciado para estado sede

## Estado

Aceptado

## Contexto

El modelo anterior usaba 32 estados con identico numero de competidores por categoria (4 prepa, 6 secundaria, 8 primaria). El estado anfitrion de la OMI puede inscribir mas competidores porque como sede tiene lugares adicionales asignados. El modelo parejo subestimaba la capacidad maxima real del evento.

## Decision

Adoptar un conteo de 31 estados regulares con contingente normal y 1 estado sede con contingente doble:

- Estados regulares (31): 4 prepa, 6 secundaria, 8 primaria.
- Estado sede (1): 8 prepa, 12 secundaria, 16 primaria.

Totales resultantes:

| Categoria | Calculo | Total |
| --- | --- | ---: |
| Preparatoria | 31 x 4 + 8 | 132 |
| Secundaria | 31 x 6 + 12 | 198 |
| Primaria | 31 x 8 + 16 | 264 |
| Total competidores | | 594 |

## Consecuencias

- La capacidad maxima simultanea sube de 256 a 264 equipos (turno primaria, Dia 2).
- Los equipos externos requeridos suben de 112 a 120 (264 - 144 equipos fijos del campus).
- Red A con /23 sigue siendo valida: 510 hosts utiles >= 264.
- El scope DHCP de Red A debe configurarse para mas de 264 clientes.
- Sala Borrego y el Plan B deben dimensionarse para 120 equipos externos.
- El calendario de dos dias no cambia.
- Las subredes, VLANs y SSIDs no cambian.

## Alternativas descartadas

- Mantener 32 estados parejos: subestima la capacidad real del estado sede.
- Crear una VLAN separada para el estado sede: innecesario; todos son competidores Red A con el mismo acceso.
