# VLAN y direccionamiento IP

## 1. ¿Qué es una VLAN?

Una **VLAN** (*Virtual Local Area Network*) es una red lógica creada dentro de
uno o más switches. Permite separar departamentos aunque utilicen los mismos
equipos físicos.

Por ejemplo, un computador de Administración y uno de Ventas pueden estar
conectados al mismo switch, pero pertenecer a VLAN diferentes. Así no quedan
en el mismo dominio de broadcast y el tráfico puede ser controlado mediante
enrutamiento y ACL.

La VLAN separa el tráfico, pero por sí sola no es una medida de seguridad
completa. Las ACL y las políticas del firewall determinan qué VLAN pueden
comunicarse.

## 2. ¿Qué es una subred?

Una **subred** es una división de un bloque de direcciones IP. Cada VLAN tendrá
su propia subred para que los dispositivos puedan identificarse y comunicarse.

Ejemplo:

```text
VLAN 10 Administración
Red:       10.10.10.0/27
Gateway:   10.10.10.1
Hosts:     10.10.10.2 - 10.10.10.30
Broadcast: 10.10.10.31
```

El prefijo `/27` indica el tamaño de la subred. En este caso hay 30 direcciones
utilizables, suficientes para la cantidad de usuarios estimada y un margen de
crecimiento.

## 3. Criterio de direccionamiento

Se utilizarán bloques privados separados por sede:

- **Casa matriz:** `10.10.0.0/16`.
- **Sucursal:** `10.20.0.0/16`.
- **Enlaces de tránsito y VPN:** bloques reservados independientes.

La diferencia entre el segundo octeto facilita identificar rápidamente el
origen de una dirección:

```text
10.10.x.x = casa matriz
10.20.x.x = sucursal
```

Esto no es una medida de seguridad; es una convención de orden y
administración.

## 4. Plan preliminar de VLAN

La numeración será igual en ambas sedes para facilitar la administración. La
subred cambia según la sede.

| VLAN | Nombre | Uso | Matriz | Sucursal |
|---:|---|---|---|---|
| 10 | ADMIN | Administración y Finanzas | `10.10.10.0/27` | `10.20.10.0/28` |
| 20 | RRHH | Recursos Humanos | `10.10.20.0/28` | `10.20.20.0/29` |
| 30 | INGENIERIA | Ingeniería y Proyectos | `10.10.30.0/26` | `10.20.30.0/27` |
| 40 | OPERACIONES | Operaciones | `10.10.40.0/27` | `10.20.40.0/28` |
| 50 | VENTAS | Ventas y Comercial | `10.10.50.0/27` | `10.20.50.0/28` |
| 60 | GERENCIA | Gerencia | `10.10.60.0/28` | `10.20.60.0/29` |
| 70 | SERVIDORES | Servicios internos | `10.10.70.0/27` | `10.20.70.0/28` |
| 80 | VOZ | Telefonía IP | `10.10.80.0/27` | `10.20.80.0/28` |
| 90 | IOT | Cámaras y termostatos | `10.10.90.0/27` | `10.20.90.0/28` |
| 100 | GUEST | Invitados, si se autoriza | `10.10.100.0/27` | `10.20.100.0/28` |
| 110 | GESTION | Administración de red | `10.10.110.0/28` | `10.20.110.0/28` |
| 120 | DMZ | Servicios publicados | `10.10.120.0/28` | Pendiente |

Las VLAN 100 y la conectividad asociada a dispositivos inalámbricos quedan
condicionadas a la confirmación del profesor. La VLAN 90 puede implementarse
con dispositivos IoT conectados por cobre si esa es la exigencia final.

## 5. ¿Por qué las subredes tienen tamaños distintos?

No todos los departamentos tienen la misma cantidad de usuarios. Ingeniería
necesita más direcciones que Recursos Humanos, por lo que recibe una subred
mayor.

Esto es una introducción a **VLSM** (*Variable Length Subnet Mask*): asignar
subredes de distinto tamaño según la necesidad real, evitando desperdiciar
direcciones IP.

La capacidad aproximada de cada prefijo es:

| Prefijo | Hosts utilizables |
|---|---:|
| `/28` | 14 |
| `/27` | 30 |
| `/26` | 62 |

La cantidad considera direcciones reservadas para red y broadcast. Además, el
gateway ocupa una dirección utilizable.

## 6. Gateways de las VLAN

El gateway será la primera dirección utilizable de cada subred y se configurará
en una interfaz virtual del switch multicapa:

```text
VLAN 10 matriz
Red:     10.10.10.0/27
Gateway: 10.10.10.1

VLAN 10 sucursal
Red:     10.20.10.0/28
Gateway: 10.20.10.1
```

Cuando un equipo necesita comunicarse con otra VLAN o con Internet, envía el
tráfico a su gateway. El switch multicapa realiza el enrutamiento inter-VLAN
o lo deriva hacia el router perimetral.

## 7. Redes de tránsito

Las conexiones entre routers y equipos de infraestructura no deben compartir
las subredes de usuarios.

Propuesta inicial:

| Enlace | Red |
|---|---|
| Router matriz - ISP 1 | `172.16.0.0/30` |
| ISP 1 - ISP 2 | `172.16.0.4/30` |
| ISP 2 - Router sucursal | `172.16.0.8/30` |
| Router matriz - core matriz | `172.16.0.12/30` |
| Router sucursal - core sucursal | `172.16.0.16/30` |

Una red `/30` tiene dos direcciones utilizables, adecuadas para un enlace
entre dos dispositivos.

## 8. Estado de implementación en Packet Tracer

En la primera etapa se implementó en la casa matriz una topología de práctica
con un router, un switch 2960 y equipos finales. El router utiliza
**router-on-a-stick**: una interfaz física en trunk y una subinterfaz por VLAN.

Se configuraron y probaron los siguientes gateways:

| VLAN | Nombre | Gateway | Resultado |
|---:|---|---|---|
| 10 | ADMIN | `10.10.10.1/27` | Operativa |
| 20 | RRHH | `10.10.20.1/28` | Operativa |
| 30 | INGENIERIA | `10.10.30.1/26` | Operativa |
| 40 | OPERACIONES | `10.10.40.1/27` | Operativa |
| 50 | VENTAS | `10.10.50.1/27` | Operativa |
| 60 | GERENCIA | `10.10.60.1/28` | Operativa |
| 70 | SERVIDORES | `10.10.70.1/27` | Operativa |
| 80 | VOZ | `10.10.80.1/27` | Gateway configurado |
| 90 | IOT | `10.10.90.1/27` | Probada con PC temporal |
| 100 | GUEST | `10.10.100.1/27` | Operativa |
| 110 | GESTION | `10.10.110.1/28` | Operativa |

Las pruebas realizadas incluyeron ping desde cada equipo de prueba hacia su
gateway y pings entre VLAN. El dispositivo IoT definitivo queda pendiente:
la webcam probada no generó una dirección MAC en el puerto del switch, por lo
que la VLAN 90 se validó temporalmente con un PC.

## 9. Decisiones aún pendientes

- Confirmar si se implementarán VLAN de invitados y WLAN.
- Confirmar si los dispositivos IoT serán cableados o inalámbricos.
- Revisar que los modelos de Packet Tracer soporten las funciones necesarias.
- Definir la segunda dirección de gateway si se implementa redundancia real.
- Completar la tabla de asignación de IP para servidores, routers y switches.

## Resumen para la defensa oral

> Separamos la red por departamentos mediante VLAN y asignamos una subred
> independiente a cada una. La casa matriz y la sucursal utilizan bloques
> privados distintos, lo que evita solapamientos y facilita el enrutamiento por
> la VPN. El tamaño de cada subred se definió según la cantidad de usuarios y
> el crecimiento esperado.

## Bitácora

### 2026-10-21

- Se definió un plan preliminar de VLAN para ambas sedes.
- Se asignaron bloques privados separados para la casa matriz y la sucursal.
- Se dejaron WLAN, invitados e IoT inalámbrico como decisiones pendientes.

### 2026-10-25

- Se implementó la primera etapa de la casa matriz en
  `packet-tracer/redes-corporativas-etapa-01-lan.pkt`.
- Se configuraron VLAN 10, 20, 30, 40, 50, 60, 70, 80, 90, 100 y 110.
- Se verificó el enrutamiento inter-VLAN mediante subinterfaces
  `G0/0.10` a `G0/0.110`.
- Se validó la VLAN 90 con un PC temporal porque la webcam IoT no aprendió
  una dirección MAC en el switch.
- Quedan pendientes SSH, ACL, DHCP, servicios, DMZ, OSPF, NAT/PAT y VPN.
