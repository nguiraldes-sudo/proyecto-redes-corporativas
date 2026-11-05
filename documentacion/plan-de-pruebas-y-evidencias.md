# Plan de pruebas y evidencias

## 1. Propósito

Este plan permite verificar la topología oficial:

```text
packet-tracer/TRABAJO REDES-backup.pkt
```

La configuración guardada en el archivo no se considera prueba suficiente.
Cada resultado debe registrarse con fecha, dispositivo, comando o acción,
resultado esperado, resultado real y captura.

## 2. Datos de la ejecución

| Campo | Valor |
|---|---|
| Fecha | Pendiente |
| Integrantes | Pendiente |
| Versión Packet Tracer | Pendiente |
| Archivo probado | `TRABAJO REDES-backup.pkt` |
| Copia de trabajo | Pendiente |

## 3. Reglas de seguridad durante las pruebas

- Trabajar sobre una copia del `.pkt`; no modificar el archivo oficial.
- No guardar contraseñas reales en capturas o documentos públicos.
- Realizar pruebas de caída solo sobre enlaces de la simulación.
- Registrar cualquier cambio realizado antes de continuar.

## 4. Pruebas de capa 2 y VLAN

### T01 — VLAN existentes

**Equipos:** `CORE-HQ1`, `CORE-HQ2`, `CORE-S1`, `CORE-S2`,
`SW-HQ-A1/A2/A3`, `SW-S-A1/A2`, `SW-DMZ`.

```text
show vlan brief
```

**Esperado:** VLAN 10, 20, 30, 40, 50, 60, 70, 80, 90 cuando corresponda,
99 y 999 aparecen con nombres coherentes.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T02 — Enlaces troncales

```text
show interfaces trunk
```

**Esperado:** los trunks están activos, usan VLAN 999 como nativa y permiten
solo las VLAN necesarias para cada enlace.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T03 — EtherChannel

```text
show etherchannel summary
show interfaces port-channel 1
```

**Esperado:** `Port-channel1` aparece operativo y los enlaces miembros están
en estado `P`/bundled.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T04 — Port-security y hardening de acceso

```text
show port-security
show port-security interface fastethernet 0/1
show spanning-tree interface fastethernet 0/1 detail
```

**Esperado:** port-security está activo; los puertos de usuarios tienen
PortFast y BPDU Guard.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

## 5. Pruebas de capa 3 y disponibilidad

### T05 — Estado de interfaces y SVI

```text
show ip interface brief
show interfaces description
```

**Esperado:** interfaces de tránsito y SVIs necesarias están `up/up`.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T06 — HSRP

```text
show standby brief
```

**Esperado:** para cada VLAN, un core aparece `Active` y el otro `Standby`;
las direcciones virtuales coinciden con la tabla de direccionamiento.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T07 — OSPF

```text
show ip ospf neighbor
show ip route ospf
show ip protocols
```

**Esperado:** vecinos en estado `FULL`, rutas OSPF hacia las dos sedes y
área 0 configurada.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T08 — Conectividad entre sedes

Desde un PC de cada sede:

```text
ping <gateway-HSRP-local>
ping <host-de-la-otra-sede>
tracert <host-de-la-otra-sede>
```

**Esperado:** el gateway responde y el tráfico autorizado llega a la otra
sede por las rutas previstas.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T09 — DHCP

En cada PC de prueba:

```text
ipconfig /all
```

En los routers:

```text
show ip dhcp binding
show ip dhcp pool
```

**Esperado:** cada equipo recibe una IP de su VLAN, gateway HSRP y DNS
correspondientes.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

## 6. Pruebas de seguridad perimetral

### T10 — IPsec

En `R-HQ` y `R-SUC`:

```text
show crypto isakmp sa
show crypto ipsec sa
```

Después generar tráfico autorizado entre sedes y repetir los comandos.

**Esperado:** asociación IKE establecida y contadores de paquetes cifrados
incrementándose. Si no se negocia, registrar el estado exacto y no declarar
VPN operativa.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T11 — NAT/PAT

Desde un PC interno, generar tráfico hacia el PC externo. Luego, en `R-HQ`:

```text
show ip nat translations
show ip nat statistics
```

**Esperado:** aparece una traducción PAT para el tráfico hacia Internet.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T12 — Publicación web y DMZ

Desde el PC externo:

```text
http://200.10.10.2
```

En `R-HQ`:

```text
show ip nat translations
show access-lists
```

**Esperado:** solo el servicio web publicado responde. Antes de probar,
verificar que el servidor web use la misma IP configurada en NAT y una máscara
coherente con `10.10.1.144/28`.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T13 — ACL y acceso a gestión

Desde un equipo de la VLAN 99:

```text
ssh -l admin <IP-del-equipo-de-red>
```

Repetir desde una VLAN no autorizada.

**Esperado:** SSH funciona desde VLAN 99 y falla desde una red no autorizada.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

## 7. Pruebas de servicios

### T14 — Servicios de la DMZ

Verificar en cada servidor:

| Servidor | Servicio | Verificación |
|---|---|---|
| Server HTTP | HTTP/HTTPS | Navegador desde red autorizada y PC externo |
| Server1 | DNS | Resolver registros configurados |
| Server2 | NTP | Consultar sincronización |
| Server3 | Email | Enviar y recibir correo |
| Server4 | Syslog | Revisar recepción de eventos |
| Server5 | AAA | Autenticación de un equipo autorizado |
| Server6 | IoT | Acceso al monitor IoT |

**Esperado:** cada servicio está habilitado, tiene una IP estable —estática o
reservada por DHCP— y responde solo desde las redes permitidas.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

### T15 — Telefonía IP

```text
show ephone registered
show telephony-service
```

**Esperado:** teléfonos registrados con extensiones de matriz `100x` y
sucursal `200x`. Realizar una llamada de prueba si Packet Tracer lo permite.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

## 8. Prueba de tolerancia a fallas

### T16 — Caída controlada de un core o enlace

Antes de apagar un enlace, registrar HSRP, OSPF y EtherChannel. Deshabilitar
un enlace o apagar un core durante la simulación y repetir:

```text
show standby brief
show ip ospf neighbor
show etherchannel summary
ping <destino-autorizado>
```

**Esperado:** el tráfico continúa por el camino disponible o se documenta
claramente la limitación, especialmente para `SW-DMZ`, que tiene un único
uplink hacia `CORE-HQ1`.

**Resultado real:** Pendiente.  
**Captura:** Pendiente.

## 9. Registro consolidado

| ID | Resultado | Evidencia | Observación |
|---|---|---|---|
| T01 | Pendiente | Pendiente | |
| T02 | Pendiente | Pendiente | |
| T03 | Pendiente | Pendiente | |
| T04 | Pendiente | Pendiente | |
| T05 | Pendiente | Pendiente | |
| T06 | Pendiente | Pendiente | |
| T07 | Pendiente | Pendiente | |
| T08 | Pendiente | Pendiente | |
| T09 | Pendiente | Pendiente | |
| T10 | Pendiente | Pendiente | |
| T11 | Pendiente | Pendiente | |
| T12 | Pendiente | Pendiente | |
| T13 | Pendiente | Pendiente | |
| T14 | Pendiente | Pendiente | |
| T15 | Pendiente | Pendiente | |
| T16 | Pendiente | Pendiente | |
