# Matriz de tráfico y políticas de acceso

## 1. Propósito

Esta matriz traduce el diseño de la red en reglas verificables. Para cada flujo
se define el origen, el destino, el servicio, la acción y la razón de negocio.
Las reglas se implementarán mediante ACL, políticas del perímetro y, cuando
corresponda, controles de la WLAN.

La política general es **mínimo privilegio**: todo tráfico no descrito como
permitido debe considerarse bloqueado.

## 2. Zonas y redes involucradas

| Zona | Redes principales | Función |
|---|---|---|
| Usuarios | VLAN 10, 20, 30, 40, 50 y 60 | Equipos de los departamentos |
| Servidores | VLAN 70 | Servicios internos corporativos |
| Voz | VLAN 80 | Telefonía IP |
| IoT | VLAN 90 | Cámaras y termostatos |
| Invitados | VLAN 100, opcional | Acceso inalámbrico solo a Internet |
| Gestión | VLAN 110 | Administración de switches, routers y AP |
| DMZ | VLAN 120, inicialmente en matriz | Servicios publicados |
| Sedes | Redes `10.10.0.0/16` y `10.20.0.0/16` | Tráfico corporativo por VPN |
| Internet | Red externa | Servicios fuera de la empresa |

## 3. Reglas entre redes internas

| Origen | Destino | Servicio | Acción | Justificación |
|---|---|---|---|---|
| VLAN de usuarios | DNS/DHCP autorizado | DNS, DHCP | Permitir | Obtener configuración y resolver nombres |
| VLAN de usuarios | VLAN 70 SERVIDORES | HTTPS, SMB/servicios requeridos | Permitir según necesidad | Usar aplicaciones corporativas |
| VLAN de usuarios | Otras VLAN de usuarios | Cualquier otro | Bloquear | Evitar movimiento lateral |
| VLAN 30 INGENIERIA | Servidores de proyectos | HTTPS/servicio del proyecto | Permitir | Trabajo técnico |
| VLAN 20 RRHH | Servidores autorizados | HTTPS | Permitir | Acceso a sistemas de personal |
| VLAN 10 ADMIN | Servidores financieros | HTTPS/servicio autorizado | Permitir | Gestión administrativa |
| VLAN 60 GERENCIA | Servicios corporativos | HTTPS, correo y servicios autorizados | Permitir | Movilidad y gestión |
| VLAN 80 VOZ | Servidor de telefonía | SIP/RTP | Permitir | Funcionamiento de telefonía IP |
| VLAN 90 IOT | Servidor de gestión | DNS, NTP, HTTPS | Permitir | Operación y administración de IoT |
| VLAN 90 IOT | VLAN de usuarios | Cualquier otro | Bloquear | Aislar cámaras y termostatos |
| VLAN 110 GESTION | Equipos de red | SSH, HTTPS, SNMP autorizado | Permitir | Administración controlada |
| Cualquier VLAN | VLAN 110 GESTION | Cualquier otro | Bloquear | Proteger la administración |

Las reglas exactas de puertos se ajustarán a los servicios que finalmente se
implementen en Packet Tracer. No se debe permitir una red completa cuando solo
se necesita un servicio específico.

## 4. Reglas entre sedes mediante VPN

| Origen | Destino | Servicio | Acción | Justificación |
|---|---|---|---|---|
| VLAN autorizadas de sucursal | Servidores autorizados de matriz | Servicios corporativos | Permitir por IPsec | Operación entre sedes |
| VLAN autorizadas de matriz | Servidores autorizados de sucursal | Servicios corporativos | Permitir por IPsec | Operación entre sedes |
| VLAN de usuarios | VLAN de gestión de la otra sede | Cualquier otro | Bloquear | La VPN no elimina las ACL |
| VLAN 90 IOT | Redes de usuarios de la otra sede | Cualquier otro | Bloquear | Reducir impacto de un dispositivo comprometido |
| Redes corporativas | Internet | HTTP/HTTPS, DNS y servicios permitidos | Salir por NAT/PAT local | Navegación sin enviar todo por la VPN |

El tráfico entre `10.10.0.0/16` y `10.20.0.0/16` debe quedar excluido de NAT
antes de aplicar IPsec. La VPN cifra el flujo, pero las ACL siguen definiendo
qué redes y servicios pueden atravesar el túnel.

## 5. Reglas desde y hacia Internet

| Origen | Destino | Servicio | Acción | Justificación |
|---|---|---|---|---|
| VLAN internas | Internet | HTTP/HTTPS, DNS y servicios permitidos | Permitir con NAT/PAT | Acceso controlado a Internet |
| Internet | Servidor web de DMZ | HTTPS; HTTP solo si se redirige | Permitir | Publicar el servicio web |
| Internet | DNS/correo de DMZ | Solo puertos definidos | Permitir si el servicio se implementa | Publicar servicios necesarios |
| Internet | VLAN internas | Cualquier otro | Bloquear | Las redes privadas no se exponen |
| Internet | VLAN de gestión | Cualquier otro | Bloquear | Impedir administración externa |
| VLAN GUEST | Redes internas, DMZ y VPN | Cualquier otro | Bloquear | Invitados solo necesitan Internet |
| VLAN GUEST | Internet | HTTP/HTTPS y DNS | Permitir con NAT/PAT | Acceso aislado para visitantes |

## 6. SSID y WLAN opcional

Wireless será una capa adicional conectada por cobre a los switches. Los SSID
no reemplazan las VLAN cableadas ni hacen depender de ellos los servicios
principales.

| SSID | VLAN | Usuarios | Política |
|---|---:|---|---|
| `ANDES-CORP` | VLAN departamental correspondiente | Usuarios autorizados | Acceso según su departamento |
| `ANDES-GERENCIA` | VLAN 60 | Gerencia | Acceso a servicios autorizados |
| `ANDES-GUEST` | VLAN 100 | Invitados | Solo Internet |

La demostración puede comenzar con `ANDES-GUEST` y `ANDES-GERENCIA`, si los
modelos disponibles en Packet Tracer permiten validar la seguridad inalámbrica.
Cada SSID debe usar WPA2/WPA3 y una clave propia. Las claves utilizadas en la
demostración serán académicas y no deben reutilizar credenciales reales.

## 7. Orden recomendado de implementación

1. Crear VLAN y gateways.
2. Configurar enlaces troncales y enrutamiento inter-VLAN.
3. Implementar DHCP y servicios internos.
4. Configurar OSPF y verificar rutas.
5. Configurar NAT/PAT y probar salida a Internet.
6. Configurar ACL internas y perimetrales.
7. Configurar VPN y excepción de NAT.
8. Agregar WLAN opcional y comprobar aislamiento de invitados.
9. Aplicar hardening y documentar evidencias.

## 8. Criterios de prueba

- Un equipo de cada VLAN obtiene una dirección correcta por DHCP.
- Un usuario accede solo a los servidores que su departamento necesita.
- Un invitado navega por Internet, pero no llega a VLAN internas ni a la DMZ.
- IoT llega a su servidor de gestión, pero no inicia conexiones hacia usuarios.
- Las sedes intercambian solo tráfico corporativo autorizado por la VPN.
- Internet llega únicamente a los servicios publicados en la DMZ.
- La gestión de equipos funciona por SSH desde VLAN 110 y falla desde otras VLAN.

## Resumen para la defensa oral

> La matriz convierte los requisitos del cliente en reglas concretas. No basta
> con conectar las sedes mediante una VPN: las ACL siguen limitando el origen,
> destino y servicio. Así la empresa puede operar, publicar servicios y dar
> acceso inalámbrico sin exponer sus redes internas.

## Bitácora

### 2026-10-25

- Se definieron políticas de tráfico entre VLAN, sedes, DMZ, IoT, gestión e
  Internet.
- Se especificó el tratamiento de SSID corporativo, gerencia e invitados.
- Se establecieron criterios de prueba antes de iniciar Packet Tracer.
