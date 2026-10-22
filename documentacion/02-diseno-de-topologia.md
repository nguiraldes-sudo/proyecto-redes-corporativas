# Diseño inicial de topología

## 1. ¿Qué es una topología?

La topología es el plano de la red. Muestra qué dispositivos existen, cómo se
conectan y por dónde circulará el tráfico.

Antes de configurar comandos en Packet Tracer debemos definir este plano. Así
evitamos agregar equipos sin propósito, crear conexiones innecesarias o
mezclar redes que deberían estar separadas.

## 2. Arquitectura general

La solución tendrá dos sedes conectadas a través de una red ISP simulada:

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

### Access points

Proporcionan conectividad inalámbrica. Cada SSID se asociará a una VLAN
determinada:

- SSID corporativo por departamento.
- SSID de invitados.
- SSID o red móvil de gerencia.

### Servidores

Los servicios que deban publicarse se ubicarán en la DMZ. Los servicios de
infraestructura interna pueden ubicarse en una VLAN de servidores protegida.

## 4. Diagrama lógico preliminar

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
           │ SW-M1 / SW-M2  │           │ SW-S1 / SW-S2  │
           └───────┬────────┘           └───────┬────────┘
                   │                           │
          VLAN de usuarios,             VLAN de usuarios,
          servidores, voz,              servidores, voz,
          invitados e IoT               invitados e IoT
```

## 5. Separación de redes

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

## 6. Recorrido de algunos tráficos

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

## 7. Decisiones pendientes

- Modelo exacto de router o firewall disponible en Packet Tracer.
- Cantidad final de access points y teléfonos.
- Si la DMZ será centralizada en la casa matriz o existirá en ambas sedes.
- Plan de direccionamiento IP y tamaño de cada subred.
- Protocolo de redundancia disponible y demostrable en Packet Tracer.
- Reglas detalladas de ACL.

## 8. Conceptos aprendidos

- **Topología física:** ubicación y conexión material de los dispositivos.
- **Topología lógica:** VLAN, subredes, rutas y políticas que determinan cómo
  circula el tráfico.
- **Núcleo:** parte central que concentra el tráfico de una sede.
- **Perímetro:** frontera entre la red corporativa y las redes externas.
- **VLAN:** separación lógica de una red física.

## Bitácora

### 2026-10-21

- Se creó un diseño preliminar de topología para la casa matriz y la sucursal.
- Se identificaron los bloques de perímetro, núcleo, usuarios, servidores,
  Wi-Fi, voz e IoT.
- Se documentaron los recorridos de tráfico principales.
