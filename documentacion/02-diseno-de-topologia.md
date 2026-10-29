# Diseño inicial de topología

## 1. ¿Qué es una topología?

La topología es el plano de la red. Muestra qué dispositivos existen, cómo se
conectan y por dónde circulará el tráfico.

Antes de configurar comandos en Packet Tracer debemos definir este plano. Así
evitamos agregar equipos sin propósito, crear conexiones innecesarias o
mezclar redes que deberían estar separadas.

## 2. Arquitectura general

La solución tendrá dos sedes conectadas a través de una red ISP simulada. La
captura actual representa una versión de trabajo de la topología física; aún
faltan algunos enlaces de cobre, por lo que no debe considerarse la versión
final del archivo `.pkt`.

```text
Casa matriz ── Router ISP 1 ── Router ISP 2 ── Sucursal
     │                                  │
     └──────────── VPN IPsec ───────────┘
```

Los routers ISP representan el proveedor de Internet. La VPN IPsec se
establecerá entre los routers perimetrales de la casa matriz y la sucursal,
pasando por la red externa simulada.

## 3. Bloques de cada sede

Cada sede se dividirá en los siguientes bloques:

```text
Internet / ISP
       │
Router perimetral o firewall
       │
Switches multicapa de núcleo
   ┌───┼───────────────┐
   │   │               │
Usuarios  Servidores   Wi-Fi / IoT / Voz
```

### Router perimetral o firewall

Es la frontera entre la red de la empresa e Internet. Se encargará de
funciones como:

- Conexión con el ISP.
- NAT/PAT.
- VPN IPsec.
- Filtrado de tráfico externo.
- Enrutamiento hacia la red interna.

### Dos switches multicapa

Los switches multicapa combinan funciones de switch y router. Se utilizarán
para:

- Crear y transportar VLAN.
- Realizar enrutamiento inter-VLAN.
- Servir como puerta de enlace de los departamentos.
- Conectar usuarios, access points, teléfonos y servidores.
- Aumentar la disponibilidad dentro de cada sede.

En Packet Tracer se documentará claramente si el diseño utiliza ambos switches
activamente o si uno se reserva como respaldo, porque la alta disponibilidad
completa depende de las funciones soportadas por los equipos simulados.

### Access points y Wireless

La conectividad Wireless es opcional, por lo que se incorporará como una capa
adicional de movilidad. La infraestructura principal seguirá conectada por
cobre y no dependerá de los access points para operar.

Los access points se conectarán por Ethernet a los switches y podrán ofrecer:

- SSID corporativos asociados a las VLAN de los departamentos.
- SSID de invitados asociado a una VLAN aislada.
- Red móvil para gerencia, si Packet Tracer permite demostrarla de forma
  clara.

La seguridad inalámbrica utilizará WPA2 o WPA3 según las opciones disponibles
en Packet Tracer. La WLAN debe respetar las mismas políticas de segmentación y
ACL que las conexiones cableadas.

### Servidores

Los servicios que deban publicarse se ubicarán en la DMZ. Los servicios de
infraestructura interna pueden ubicarse en una VLAN de servidores protegida.

## 4. Topología física observada en la versión de trabajo

### Casa matriz

En la parte izquierda de la captura se observan:

- `R-HQ`: router perimetral de la casa matriz.
- `CORE-HQ1` y `CORE-HQ2`: dos switches multicapa Catalyst 3650-24PS.
- `SW-DMZ`: switch de la DMZ, conectado a seis servidores.
- Tres switches de acceso para usuarios y dispositivos finales.
- Computadores, impresoras, teléfonos IP, cámaras/webcam y termostatos IoT.
- Un access point conectado a uno de los switches de acceso.

Los dos switches core aparecen interconectados y enlazados con los switches
de acceso. `R-HQ` tiene conexiones visibles hacia el bloque core y hacia
`SW-DMZ`. La captura todavía requiere completar y revisar algunos enlaces de
cobre antes de validar la redundancia y la conectividad total.

### Red ISP simulada

En la parte superior central se observan `R-HQ`, `ISP1`, `ISP2` y `R-SUC`.
También aparece un computador externo conectado a `ISP1` para representar una
prueba desde Internet.

Esta cadena representa el tránsito entre las sedes y el acceso externo
simulado. Los enlaces entre routers deberán revisarse en Packet Tracer junto
con el tipo de cable y el estado de las interfaces.

### Sucursal

En la parte derecha se observan:

- `R-SUC`: router perimetral de la sucursal.
- `CORE-S1` y `CORE-S2`: dos switches multicapa Catalyst 3650-24PS.
- `SW-S-A1` y `SW-S-A2`: dos switches de acceso.
- Computadores, impresoras, teléfonos IP, webcam y termostato IoT.

En esta versión del screenshot, `CORE-S1` y `CORE-S2` aparecen sin enlaces de
cobre visibles hacia `R-SUC` ni hacia los switches de acceso. Los dispositivos
finales están conectados a `SW-S-A1` y `SW-S-A2`, pero esos switches todavía no
se observan conectados al bloque core. Por lo tanto, deben agregarse y
verificarse los enlaces `R-SUC`–core, core–core y core–acceso antes de iniciar
las pruebas de redundancia, OSPF y segmentación.

### Estado visual provisional

- Los enlaces negros representan conexiones de cobre Ethernet visibles.
- Los equipos sin cable visible todavía no se consideran integrados.
- La vista de Packet Tracer debe complementarse con `show cdp neighbors` y
  `show interfaces status` para validar conexiones y estados.
- Los nombres de los dispositivos finales siguen siendo los nombres
  predeterminados de Packet Tracer, como `Server-PT`, `PC-PT`,
  `Printer-PT`, `IP Phone`, `Webcam` y `Thermostat`.

## 5. Diagrama lógico preliminar

```text
                         ┌──────────────┐
                         │   Internet   │
                         └──────┬───────┘
                                │
                      ┌─────────┴─────────┐
                      │   Red ISP simulada │
                      └───────┬─────┬─────┘
                              │     │
                   ┌──────────┘     └──────────┐
                   │                           │
          ┌────────▼────────┐         ┌────────▼────────┐
          │ Router matriz   │◄═IPsec═►│ Router sucursal │
          └────────┬────────┘         └────────┬────────┘
                   │                           │
           ┌───────▼────────┐           ┌───────▼────────┐
           │ Core multilayer│           │ Core multilayer│
           │ CORE-HQ1 / HQ2 │           │ CORE-S1 / CORE-S2│
           └───────┬────────┘           └───────┬────────┘
                   │                           │
          VLAN de usuarios,             VLAN de usuarios,
          servidores, voz,              servidores, voz,
          invitados e IoT               invitados e IoT
```

## 6. Separación de redes

Cada departamento tendrá una VLAN independiente. La VLAN es una separación
lógica: permite que varios grupos compartan switches físicos sin quedar en la
misma red de broadcast.

También se reservarán VLAN para:

- Gestión de equipos.
- Servidores internos.
- DMZ.
- Voz.
- IoT.
- Invitados.
- Gerencia.

La lista y el direccionamiento exactos se definirán en el siguiente documento,
después de revisar la cantidad de usuarios y el crecimiento esperado.

## 7. Recorrido de algunos tráficos

### Usuario interno hacia Internet

```text
PC → VLAN del departamento → switch multicapa
   → router perimetral → NAT/PAT → ISP → Internet
```

### Usuario de sucursal hacia un servidor de la matriz

```text
PC sucursal → switch multicapa → router sucursal
   → túnel VPN IPsec → router matriz
   → servidor autorizado
```

### Usuario externo hacia el servidor web

```text
Internet → router/firewall matriz → regla publicada
         → DMZ → servidor web
```

El tráfico externo no debe saltar desde Internet directamente hacia las VLAN
de usuarios.

## 8. Decisiones pendientes

- Completar los enlaces de cobre que faltan en la captura de trabajo.
- Confirmar qué puertos físicos conectan cada router, core y switch de acceso.
- Verificar los enlaces entre routers y su estado operativo.
- Modelo exacto de router o firewall disponible en Packet Tracer.
- Cantidad final de access points y teléfonos.
- Si la DMZ será centralizada en la casa matriz o existirá en ambas sedes.
- Plan de direccionamiento IP y tamaño de cada subred.
- Protocolo de redundancia disponible y demostrable en Packet Tracer.
- Reglas detalladas de ACL.
- Validar qué funciones Wireless permite demostrar Packet Tracer 9.0.

## 9. Nombres de la topología actual

La documentación debe utilizar los nombres que aparecen actualmente en
`redes-corporativas-topologia-screenshot-2026-10-27.pkt`. No se deben usar
los nombres de una convención futura mientras los dispositivos no hayan sido
renombrados en Packet Tracer.

### Routers perimetrales
- `R-HQ`: router perimetral de la casa matriz.
- `R-SUC`: router perimetral de la sucursal.

### Red externa simulada
- `ISP1`: router ISP (lado matriz).
- `ISP2`: router ISP (lado sucursal).

### Switches multicapa (núcleo)
- `CORE-HQ1` y `CORE-HQ2`: switches multicapa Catalyst 3650-24PS de la casa matriz.
- `CORE-S1` y `CORE-S2`: switches multicapa Catalyst 3650-24PS de la sucursal.

### Switches de acceso
- `SW-HQ-A1`, `SW-HQ-A2` y `SW-HQ-A3`: switches de acceso de la casa matriz.
- `SW-S-A1` y `SW-S-A2`: switches de acceso de la sucursal (VTP transparent,
  VLAN 99 gestión, Port-security activado, PortFast/BPDU Guard enableados).

### Zona DMZ
- `SW-DMZ`: switch de la zona DMZ, conectado a servidores públicos.

### Dispositivos finales (nombres predeterminados de Packet Tracer)
- `Server-PT`, `PC-PT`, `Printer-PT`, `IP Phone`, `Webcam` y `Thermostat`.

### Access point
- `AccessPoint-PT`: access point observado en la matriz.

La topología completa se construirá como una segunda versión del archivo
`.pkt`, sin sobrescribir `redes-corporativas-etapa-01-lan.pkt`.

La topología completa se construirá como una segunda versión del archivo
`.pkt`, sin sobrescribir `redes-corporativas-etapa-01-lan.pkt`.

## 10. Conceptos aprendidos

- **Topología física:** ubicación y conexión material de los dispositivos.
- **Topología lógica:** VLAN, subredes, rutas y políticas que determinan cómo
  circula el tráfico.
- **Núcleo:** parte central que concentra el tráfico de una sede.
- **Perímetro:** frontera entre la red corporativa y las redes externas.
- **VLAN:** separación lógica de una red física.
- **Enlace de cobre:** conexión Ethernet física mediante cable, normalmente
  cable UTP, entre un dispositivo y otro.

## Bitácora

### 2026-10-21

- Se creó un diseño preliminar de topología para la casa matriz y la sucursal.
- Se identificaron los bloques de perímetro, núcleo, usuarios, servidores,
  Wi-Fi, voz e IoT.
- Se documentaron los recorridos de tráfico principales.
- Se incorporó Wireless como componente opcional, manteniendo la infraestructura
  principal conectada por cobre.

### 2026-10-27

- Se actualizó el diseño con base en una captura de la topología física de
  trabajo.
- Se identificaron `R-HQ`, `ISP1`, `ISP2`, `R-SUC`, dos cores por sede,
  `SW-DMZ`, switches de acceso, servidores, usuarios, telefonía e IoT.
- La captura todavía no representa la versión final: faltan algunos cables de
  cobre y deben verificarse los enlaces de routers y cores antes de declarar
  completa la topología.
