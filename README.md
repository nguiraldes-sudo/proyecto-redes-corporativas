# Proyecto de Redes Corporativas

## Proyecto seleccionado

**Proyecto 2: Casa matriz y sucursal seguras**

## Enfoque comercial

Este trabajo se presentará como una solución de redes y telecomunicaciones que debe venderse a un cliente empresarial. No se mostrará únicamente una configuración de Cisco Packet Tracer, sino una propuesta integral orientada a resolver necesidades de seguridad, conectividad, disponibilidad y crecimiento.

La propuesta deberá demostrar el problema del cliente, la solución técnica, sus beneficios comerciales, la implementación funcional, las pruebas y evidencias, la justificación de equipos y servicios, y el valor de la inversión.

## Requisitos técnicos

- Casa matriz y sucursal.
- VPN IPsec site-to-site a través de dos routers ISP simulados.
- NAT/PAT hacia Internet.
- DMZ con servidores HTTP, DNS, NTP, correo, Syslog y AAA.
- ACL para publicar únicamente los servicios necesarios.
- Mínimo seis departamentos por sede, cada uno con su VLAN y SSID.
- WLAN con WPA2 o WPA3 y claves propias.
- Dos switches multicapa por sede.
- OSPF interno y DHCP por VLAN.
- Telefonía IP, cámaras, termostatos IoT y red móvil para gerencia.
- Hardening con SSH v2, usuarios locales, port-security, banners y contraseñas cifradas.

## Presupuesto

- Rango: **$20.000.000 a $35.000.000 CLP**.
- Precios con IVA.
- Equipos vigentes y disponibles para 2026.
- Mínimo dos cotizaciones chilenas por ítem relevante.
- Justificación técnica y comercial de cada equipo.
- Inclusión de cableado Cat6, rack, patch panel, UPS y licencias.

## Entregables

- Archivo funcional de Cisco Packet Tracer 9.0 (`.pkt`).
- Informe técnico en PDF.
- Planilla de presupuesto con cotizaciones.
- Presentación comercial y técnica de 15 minutos.
- Demostración en vivo.

## Organización del repositorio

- `packet-tracer/`: archivos `.pkt` y versiones de la topología.
- `documentacion/`: informe, requisitos, diseño y evidencias.
- `presupuesto/`: planilla, cotizaciones y justificación de equipos.
- `presentacion/`: diapositivas y material para la defensa.
- `diagramas/`: topologías y diagramas de red.
- `documentacion/01-requisitos-del-cliente.md`: perfil, necesidades y criterios de aceptación.
- `documentacion/00-glosario-de-aprendizaje.md`: explicaciones paso a paso de los conceptos técnicos.
- `documentacion/02-diseno-de-topologia.md`: diseño lógico y físico preliminar de ambas sedes.
- `documentacion/03-vlan-y-direccionamiento-ip.md`: VLAN, subredes, gateways y redes de tránsito.
- `documentacion/04-seguridad-perimetral-y-vpn.md`: DMZ, VPN IPsec, NAT/PAT y ACL.
- `documentacion/05-matriz-de-trafico-y-politicas.md`: flujos permitidos,
  bloqueos, SSID y criterios de prueba.
- `packet-tracer/redes-corporativas-etapa-01-lan.pkt`: primera etapa funcional
  de la casa matriz con VLAN y enrutamiento inter-VLAN.
- `packet-tracer/redes-corporativas-topologia-completa.pkt`: segunda versión
  planificada para integrar ambas sedes y todos los equipos.
- `CONTRIBUTING.md`: flujo de trabajo para colaborar en el repositorio.
- `CLAUDE.md`: instrucciones comunes para asistentes de IA que trabajen en el proyecto.

## Trabajo colaborativo con IA

Cada integrante puede trabajar con su propia herramienta de IA sobre una copia local
del repositorio. Antes de modificar archivos debe ejecutar `git pull origin main`,
y al terminar debe crear un commit descriptivo y ejecutar `git push origin main`.
Las tareas deben coordinarse para evitar que dos integrantes editen simultáneamente
el mismo archivo, especialmente los archivos binarios de Packet Tracer.

## Estado del proyecto

- [x] Seleccionar el Proyecto 2.
- [x] Definir el enfoque comercial.
- [x] Confirmar el rango presupuestario.
- [x] Definir el cliente y su giro empresarial.
- [x] Levantar requisitos del cliente.
- [x] Diseñar la topología lógica y física.
- [x] Definir VLAN, departamentos y direccionamiento IP.
- [x] Diseñar DMZ, VPN, NAT y ACL.
- [x] Definir la matriz inicial de tráfico y políticas de acceso.
- [x] Implementar la primera etapa de VLAN y enrutamiento inter-VLAN.
- [ ] Construir la topología completa en Packet Tracer.
- [ ] Ejecutar pruebas y recopilar evidencias.
- [ ] Preparar presupuesto y cotizaciones.
- [ ] Redactar informe técnico.
- [ ] Preparar presentación y defensa.

## Bitácora

### 2026-10-21

- Se seleccionó el Proyecto 2.
- Se definió el enfoque de presentación como una solución para vender a un cliente.
- Se confirmó el presupuesto entre $20.000.000 y $35.000.000 CLP.
- Se preparó la carpeta local para colaboración mediante GitHub.
- Se definió como cliente académico a Andes Ingeniería y Servicios SpA.
- Se documentaron los objetivos, departamentos, servicios y criterios de aceptación.
- Se confirmó que Wireless es opcional. La infraestructura principal seguirá
  conectada por cobre y WLAN se incorporará como componente adicional.
- Se definió un plan preliminar de VLAN, subredes y redes de tránsito.
- Se documentó el diseño preliminar de DMZ, VPN IPsec, NAT/PAT y ACL.
- Se solicitó verificar y crear el archivo `CLAUDE.md` con instrucciones
  comunes para las distintas IA que utilizarán los integrantes.

### 2026-10-25

- Se implementó la primera etapa de la casa matriz en Packet Tracer.
- Se configuraron y probaron las VLAN 10, 20, 30, 40, 50, 60, 70, 80, 90,
  100 y 110.
- Se verificó el enrutamiento inter-VLAN con subinterfaces del router.
- La VLAN 90 quedó validada temporalmente con un PC mientras se selecciona un
  dispositivo IoT compatible.
- DHCP quedó probado en las VLAN de usuarios, invitados y voz; el teléfono IP
  recibió `10.10.80.11`.
- OSPF formó vecindades entre los routers ISP y ambas sedes; el ping entre la
  sucursal y la matriz funciona.
- La VPN IPsec quedó como diseño documentado, pero no se pudo implementar con
  el IOS disponible porque no soporta `crypto isakmp` ni `crypto ipsec`.
- Aún faltan completar pruebas, evidencias, presupuesto, informe y
  presentación.

### Convención de nombres

Los equipos se nombran con sede y función: `R-MATRIZ`, `R-SUCURSAL`,
`SW-M*-CORE`, `SW-S*-CORE`, `SW-M-ACC*`, `SW-S-ACC*`, `PC-M-*`, `PC-S-*`,
`SRV-*`, `PHONE-*`, `IOT-*` y `AP-*`. La versión completa se construirá sin
sobrescribir el archivo de la primera etapa.
