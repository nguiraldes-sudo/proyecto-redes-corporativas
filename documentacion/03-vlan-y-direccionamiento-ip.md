# VLAN y direccionamiento IP

> **Nota de versión:** Los ejemplos didácticos de este documento usan
> `10.10.10.0/27`, pero la topología oficial utiliza la tabla de VLAN y
> subredes de la sección 4. No mezclar ambos esquemas en el informe final.

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

La fuente oficial actual (`TRABAJO REDES-backup.pkt`) utiliza bloques privados
separados por sede:

- **Casa matriz:** `10.10.0.0` a `10.10.15.255`.
- **Sucursal:** `10.20.0.0` a `10.20.7.255`.
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
| 10 | Gerencia | Gerencia | `10.10.1.128/28` | `10.20.1.16/28` |
| 20 | Finanzas | Administración y Finanzas | `10.10.1.0/27` | `10.20.0.160/27` |
| 30 | RRHH | Recursos Humanos | `10.10.1.32/27` | `10.20.0.192/27` |
| 40 | TI | Administración técnica | `10.10.1.64/27` | `10.20.0.224/27` |
| 50 | Ventas | Ventas y Comercial | `10.10.0.0/25` | `10.20.0.0/26` |
| 60 | Operaciones | Operaciones | `10.10.0.128/26` | `10.20.0.64/26` |
| 70 | Voz | Telefonía IP | `10.10.0.192/26` | `10.20.0.128/27` |
| 80 | IoT | Cámaras, MCU y termostatos | `10.10.1.96/27` | `10.20.1.0/28` |
| 90 | DMZ | Servicios publicados, solo matriz | `10.10.1.144/28` | No implementada |
| 99 | Gestión | Administración de red | `10.10.1.160/28` | `10.20.1.32/28` |
| 999 | Nativa | VLAN nativa no utilizada por usuarios | Troncales | Troncales |

La WLAN y una VLAN de invitados no aparecen en el archivo oficial. Si la
rúbrica exige SSID por departamento, deben implementarse en una copia de
trabajo o respaldarse una autorización docente para omitirlas.

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

La fuente oficial utiliza switches multicapa con SVIs y HSRP v2. Los gateways
virtuales son:

| Sede | VLAN de ejemplo | Gateway HSRP | Estado |
|---|---:|---|---|
| Matriz | 10 | `10.10.1.129/28` | Configurado, por probar |
| Matriz | 90 | `10.10.1.145/28` | Configurado, publicación por corregir |
| Matriz | 99 | `10.10.1.161/28` | Configurado, por probar |
| Sucursal | 10 | `10.20.1.17/28` | Configurado, por probar |
| Sucursal | 80 | `10.20.1.1/28` | Configurado, por probar |
| Sucursal | 99 | `10.20.1.33/28` | Configurado, por probar |

La auditoría confirma que las configuraciones están guardadas, pero no
equivale a pruebas de conectividad, convergencia HSRP ni DHCP exitoso.

## 9. Política de asignación de direcciones

Para reducir la configuración manual de los numerosos equipos finales, se
utilizará **DHCP por VLAN** como regla general:

| Tipo de equipo | Asignación recomendada | Motivo |
|---|---|---|
| PCs y notebooks | DHCP | Alta cantidad y cambios frecuentes |
| Teléfonos IP | DHCP | Obtienen dirección y opciones de telefonía |
| Impresoras y equipos IoT | DHCP con reserva por MAC | Administración simple, pero ubicación identificable |
| Access points | DHCP con reserva por MAC | Facilita reemplazos sin perder trazabilidad |
| Gateways HSRP/SVI | Estática | Son el punto de salida de cada VLAN |
| Routers, switches y enlaces de tránsito | Estática | OSPF, gestión, ACL y monitoreo requieren direcciones conocidas |
| Servidores DMZ y corporativos | Estática o reserva DHCP | NAT, DNS, ACL y publicación de servicios dependen de una dirección estable |

Una **reserva DHCP** entrega siempre la misma dirección a un dispositivo según
su dirección MAC. Así se reduce el trabajo manual sin dejar que un servidor o
equipo de infraestructura cambie de IP inesperadamente. Las direcciones de
gateway, tránsito y gestión quedan fuera de los pools DHCP para evitar
conflictos.

Los pools deben excluir como mínimo la dirección del gateway, las direcciones
reservadas y el rango de infraestructura de cada subred. Por ejemplo, en una
VLAN cuyo gateway HSRP es `.129`, el pool no debe entregar `.129` ni las
direcciones destinadas a reservas.

## 10. Decisiones aún pendientes

- Confirmar si se implementarán WLAN/SSID y VLAN de invitados.
- Confirmar si los dispositivos IoT serán cableados o inalámbricos.
- Revisar que los modelos de Packet Tracer soporten las funciones necesarias.
- Ejecutar pruebas de HSRP, STP, EtherChannel y DHCP.
- Completar la tabla de asignación de IP para infraestructura y reservas DHCP,
  verificando físicamente cada equipo en Packet Tracer.

## Resumen para la defensa oral

> Separamos la red por departamentos mediante VLAN y asignamos una subred
> independiente a cada una. La casa matriz y la sucursal utilizan bloques
> privados distintos, lo que evita solapamientos y facilita el enrutamiento por
> la VPN. El tamaño de cada subred se definió según la cantidad de usuarios y
> el crecimiento esperado. **Versión oficial actualizada:** VLAN 99 para
> gestión, VLAN 90 para DMZ y VLAN 999 como nativa. Los switches tienen
> port-security, PortFast, BPDU Guard y VTP en modo transparent configurados;
> falta validar su efecto con pruebas.

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
- Se configuró DHCP para las VLAN de usuarios, voz e IoT; el teléfono IP
  recibió `10.10.80.11`.
- Quedan pendientes SSH, ACL, DHCP, servicios, DMZ, OSPF, NAT/PAT y VPN.
