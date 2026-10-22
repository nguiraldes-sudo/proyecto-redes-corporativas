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
- [ ] Implementar la red en Packet Tracer.
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
- Se registró como criterio preliminar el uso de conexiones por cobre y se dejó
  la conectividad inalámbrica pendiente de confirmación docente.
- Se definió un plan preliminar de VLAN, subredes y redes de tránsito.
- Se documentó el diseño preliminar de DMZ, VPN IPsec, NAT/PAT y ACL.
- Se solicitó verificar y crear el archivo `CLAUDE.md` con instrucciones
  comunes para las distintas IA que utilizarán los integrantes.
