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

- **Fuente oficial actual:** `packet-tracer/TRABAJO REDES-backup.pkt`, revisado
  el 2026-11-01. Las configuraciones auxiliares solo respaldan los datos que
  coinciden con este archivo.
- [x] Seleccionar el Proyecto 2.
- [x] Definir el enfoque comercial.
- [x] Confirmar el rango presupuestario.
- [x] Definir el cliente y su giro empresarial.
- [x] Levantar requisitos del cliente.
- [x] Diseñar la topología lógica y física.
- [x] Definir VLAN, departamentos y direccionamiento IP.
- [x] Diseñar DMZ, VPN, NAT y ACL.
- [x] Definir la matriz inicial de tráfico y políticas de acceso.
- [x] Definir un metrado preliminar de cableado para 42 PCs: 4 cajas de 305 m,
  42 puntos RJ45/keystone, patch panel de 48 puertos y patch cords.
- [x] Implementar la primera etapa de VLAN y enrutamiento inter-VLAN.
- [x] **Actualizar configuraciones con archivo Proyecto2_PT (nueva versión con VPN, port-security, VLAN 99 gestión, HSRP v2).**
- [x] Completar la estructura física principal en Packet Tracer, incluyendo
  routers, cores, switches de acceso, DMZ y enlaces redundantes de acceso.
- [ ] Verificar en ejecución HSRP, STP, EtherChannel, OSPF y las pruebas de
  caída de enlaces.
- [ ] Resolver la discrepancia de publicación web (`10.10.1.146` frente a
  `10.10.1.147/24`) y documentar las IP fijas de los servidores DMZ.
- [ ] Activar y probar DNS, AAA/ACS y los demás servicios corporativos.
- [ ] Implementar WLAN/SSID o dejar registrada una autorización docente para
  omitirla; el `.pkt` oficial no contiene Access Point.
- [ ] Verificar la negociación real de IPsec con `show crypto isakmp sa` y
  `show crypto ipsec sa`.
- [ ] Ejecutar pruebas y recopilar evidencias.
- [x] Preparar presupuesto preliminar de equipamiento y materiales, separado de
  la mano de obra.
- [x] Redactar informe técnico base en Markdown y generar versión DOCX formal.
- [ ] Confirmar mediante visita técnica las cantidades reales de cable,
  canaleta, conectores, patch panel y accesorios.
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

### 2026-10-26

- Se verificó la conectividad DHCP de la sucursal, incluyendo el teléfono IP
  en la VLAN 80 con la dirección `10.20.80.3`.
- Se confirmó que los equipos de la sucursal están operativos.
- Queda pendiente aplicar el hardening de port-security, PortFast y BPDU Guard
  en `SW-S-A1`; todavía no se aplicó esa configuración.

### 2026-10-27

- Se revisó una captura de una versión de trabajo de la topología física.
- Se confirmó la presencia de `R-HQ`, `ISP1`, `ISP2`, `R-SUC`, dos switches
  core por sede, `SW-DMZ`, switches de acceso, servidores, usuarios,
  telefonía e IoT.
- La captura aún no es final: faltan algunos cables de cobre y deben
  verificarse los enlaces antes de validar la redundancia y comenzar las
  pruebas definitivas.

### 2026-11-01

- Se tomó `packet-tracer/TRABAJO REDES-backup.pkt` como fuente oficial de la
  topología actual.
- La auditoría confirmó 51 dispositivos y 60 conexiones Ethernet, con dos
  cores por sede, uplinks redundantes de acceso y una DMZ centralizada en la
  matriz.
- Se detectaron contradicciones entre la documentación antigua y el `.pkt`:
  el esquema vigente usa VLAN 10–80, 90 DMZ, 99 gestión y 999 nativa.
- La configuración de IPsec, NAT/PAT, OSPF, DHCP, HSRP y hardening está
  almacenada, pero debe probarse en ejecución antes de declararla operativa.
- Quedaron pendientes la corrección de IPs de la DMZ, la activación de
  servicios, la WLAN y la recopilación de evidencias.

### 2026-11-03

- Se confirmó que todos los switches CORE de la topología deben ser
  multicapa/Layer 3.
- Se actualizó el informe y el presupuesto para distinguir el caso base de la
  rúbrica de la topología ampliada de dos sedes.
- Se incorporó el presupuesto preliminar de equipamiento y materiales por
  $30.878.464 IVA incluido, sin mezclar mano de obra.
- Se agregó un metrado preliminar de cableado: 42 puntos, 42 keystones o
  conectores RJ45 hembra, un patch panel de 48 puertos, 84 patch cords y
  4 cajas de cable de 305 m.
- La canaleta y las cantidades definitivas quedan condicionadas a una visita
  técnica y a un plano de recorridos.
- Se generó `documentacion/informe-final.docx` con portada, índice, estilos,
  encabezado, pie de página y tablas.

### Convención de nombres

La versión actual utiliza los nombres visibles en Packet Tracer:
`R-HQ`, `R-SUC`, `ISP1`, `ISP2`, `CORE-HQ1`, `CORE-HQ2`, `CORE-S1`,
`CORE-S2`, `SW-HQ-A1`, `SW-HQ-A2`, `SW-HQ-A3`, `SW-S-A1`, `SW-S-A2` y
`SW-DMZ`. Los dispositivos finales conservan nombres predeterminados como
`Server-PT`, `PC-PT`, `Printer-PT`, `IP Phone`, `Webcam` y `Thermostat`.
La documentación no debe usar nombres planificados que todavía no existan en
el archivo `.pkt`.
