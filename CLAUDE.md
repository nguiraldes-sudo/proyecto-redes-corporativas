# Continuidad del proyecto: Redes Corporativas

## Qué es y qué debe lograrse

Proyecto 2 de Redes Computacionales: diseñar una solución segura para **Andes
Ingeniería y Servicios SpA**, con casa matriz en Santiago y sucursal en
Valparaíso. Debe presentarse como un proyecto comercial que se vende a un
cliente, no solo como una configuración de Packet Tracer.

La solución debe implementarse en **Cisco Packet Tracer 9.0** e incluir, según
lo permitido por el profesor, VPN IPsec site-to-site, VLAN, OSPF, DMZ,
NAT/PAT, ACL, DHCP, servicios corporativos, IoT y hardening con SSH v2,
usuarios locales, banners, contraseñas cifradas y port-security.

## Cómo continuar el trabajo

1. Leer este archivo y `README.md`.
2. Revisar el glosario antes de configurar conceptos nuevos.
3. Actualizar la copia local:

   ```bash
   git pull origin main
   ```

4. Trabajar en la carpeta correspondiente y explicar cada decisión en
   `documentacion/`.
5. Guardar y sincronizar:

   ```bash
   git add .
   git commit -m "Describir el cambio"
   git push origin main
   ```

No editar simultáneamente el mismo archivo `.pkt`; Packet Tracer no fusiona
automáticamente cambios internos de archivos binarios.

## Ya realizado

- Repositorio privado creado y colaboradores invitados.
- Cliente, situación actual, objetivos y requisitos documentados.
- Topología preliminar de matriz, sucursal, ISP, perímetro, core, usuarios,
  servidores, voz e IoT.
- Plan inicial de VLAN y direccionamiento:
  `10.10.x.x` para matriz y `10.20.x.x` para sucursal.
- Diseño preliminar de DMZ, VPN IPsec, NAT/PAT y ACL.
- Glosario pedagógico con explicaciones de IoT, IPsec, DMZ, NAT/PAT, ACL,
  SSH, VLAN, subredes, gateways y direccionamiento.
- Carpetas preparadas: `packet-tracer/`, `documentacion/`, `presupuesto/`,
  `presentacion/` y `diagramas/`.

## Pendientes principales

- Validar qué funciones Wireless permite demostrar Packet Tracer 9.0 y qué
  dispositivos móviles se incluirán en la demostración.
- Validar modelos y funciones disponibles en Packet Tracer 9.0.
- Completar topología física y lógica definitiva.
- Revisar VLSM, gateways, enlaces de tránsito y tabla completa de IP.
- Configurar y probar VLAN, OSPF, DHCP, VPN, NAT/PAT, DMZ, ACL y hardening.
- Crear el archivo `.pkt`, recopilar evidencias y documentar pruebas.
- Preparar dos cotizaciones por ítem, justificaciones y presupuesto entre
  **$20.000.000 y $35.000.000 CLP**, con IVA.
- Elaborar informe PDF y presentación comercial de 15 minutos.

## Decisiones importantes

- La infraestructura principal debe conectarse por cobre.
- Wireless es opcional y se incorporará como mejora de movilidad, segmentada
  mediante VLAN y protegida con WPA2/WPA3 y ACL.
- La DMZ debe aislar servicios públicos de las VLAN internas.
- La VPN protege tráfico corporativo entre sedes; NAT/PAT gestiona salida a
  Internet y ACL controla permisos. No son funciones equivalentes.
- Todo cambio importante debe quedar respaldado en Markdown y GitHub.
- No subir credenciales reales, tokens ni datos sensibles.

## Documentos de referencia

- `README.md`: checklist y bitácora.
- `documentacion/00-glosario-de-aprendizaje.md`: material pedagógico.
- `documentacion/01-requisitos-del-cliente.md`: cliente y necesidades.
- `documentacion/02-diseno-de-topologia.md`: topología.
- `documentacion/03-vlan-y-direccionamiento-ip.md`: VLAN e IP.
- `documentacion/04-seguridad-perimetral-y-vpn.md`: seguridad y VPN.
- `documentacion/05-matriz-de-trafico-y-politicas.md`: reglas de tráfico,
  SSID y criterios de prueba.

## Estado actual de Packet Tracer

El archivo `packet-tracer/redes-corporativas-etapa-01-lan.pkt` contiene una
primera etapa funcional de la casa matriz: un router, un switch 2960 y VLAN
10, 20, 30, 40, 50, 60, 70, 80, 90, 100 y 110 con enrutamiento inter-VLAN.
La VLAN 90 fue probada temporalmente con un PC porque la webcam IoT utilizada
no aprendió una dirección MAC en el switch. Antes de continuar, guardar el
archivo `.pkt` en Packet Tracer.

También se configuró SSH en el router y el switch, con administración desde la
VLAN 110. Se creó una ACL para bloquear el acceso de la VLAN 100 a las redes
internas, manteniendo disponible su gateway y dejando una excepción para DHCP.
DHCP está configurado y probado en las VLAN 10, 20, 30, 40, 50, 60, 80 y 100.
El teléfono IP recibió la dirección `10.10.80.11` mediante DHCP. La VLAN 100
también obtiene dirección después de permitir DHCP en su ACL. La VLAN 70 de
servidores y la VLAN 110 de gestión conservan direccionamiento manual. La VLAN
90 todavía usa un PC temporal para la prueba y puede requerir DHCP cuando se
seleccione un dispositivo IoT compatible.

La topología también incluye `ISP-1`, `ISP-2` y `R-SUCURSAL`. OSPF formó
vecindades FULL entre los routers y el ping entre `PC-SUCURSAL` y la red
`10.10.10.0/27` funciona. La VPN IPsec no pudo configurarse en `R-SUCURSAL`
porque su IOS solo ofrece `crypto key` y no soporta `crypto isakmp` ni
`crypto ipsec`; esta limitación debe explicarse en el informe.

## Convención de nombres de la topología completa

La versión 2 se guardará como
`packet-tracer/redes-corporativas-topologia-completa.pkt`, conservando la
versión inicial como respaldo. Los equipos existentes de la matriz se
identifican como `R-MATRIZ`, `SW-M-ACC0`, `PC-M-ADMIN1`,
`PC-M-ADMIN2`, `PC-M-RRHH`, `PC-M-INGENIERIA`, `PC-M-OPERACIONES`,
`PC-M-VENTAS`, `PC-M-GERENCIA`, `SRV-SERVICIOS-M`, `PHONE-M-ADMIN`,
`IOT-M-PRUEBA`, `PC-M-GUEST` y `PC-M-GESTION`.

Los nuevos equipos de la matriz usarán `SW-M1-CORE`, `SW-M2-CORE`,
`SW-M-ACC1`, `SW-M-ACC2`, `SRV-DMZ-M`, `PHONE-M-GERENCIA`,
`IOT-M-CAMARA`, `AP-M-GENERAL` y `PC-M-SOPORTE`. En la sucursal se usarán
`SW-S1-CORE`, `SW-S2-CORE`, `SW-S-ACC0`, `SW-S-ACC1`, `SW-S-ACC2`,
`PC-S-ADMIN`, `PC-S-RRHH`, `PC-S-ING`, `PC-S-OPER`, `PC-S-VENTAS`,
`PC-S-GERENCIA`, `SRV-SERVICIOS-S`, `PHONE-S-GERENCIA`,
`IOT-S-CAMARA` y `AP-S-GENERAL`.

## Punto de pausa: 2026-10-26

- La conectividad y DHCP de los equipos de la sucursal fueron verificados.
- `PHONE-S-GERENCIA` recibió `10.20.80.3` en la VLAN 80.
- El siguiente paso es aplicar hardening en `SW-S-ACC0`.
- No se aplicó todavía la configuración de port-security, PortFast ni BPDU
  Guard en ese switch.
