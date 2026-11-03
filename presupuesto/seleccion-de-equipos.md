# Selección preliminar de equipos

## 1. Criterio de selección

La selección comercial traduce la topología de Packet Tracer a equipos
disponibles para una empresa mediana. Se priorizan:

- soporte de VLAN, seguridad y administración centralizada;
- capacidad PoE para teléfonos, access points y cámaras;
- redundancia en el núcleo y el perímetro;
- disponibilidad de repuestos y soporte;
- crecimiento sin reemplazar toda la infraestructura.

Los precios son referencias públicas consultadas el 2 de noviembre de 2026,
con IVA cuando la fuente lo indica. Deben confirmarse mediante cotización
formal antes de entregar el presupuesto. El tope de $35.000.000 se aplica al
equipamiento, materiales, licencias y accesorios; la mano de obra se informa
por separado.

## 2. Arquitectura comercial propuesta

| Bloque | Selección | Cantidad | Motivo |
|---|---|---:|---|
| Perímetro | Fortinet FortiGate 60F | 2 | VPN IPsec, firewall, NAT/PAT y operación para sucursales |
| Núcleo | Cisco Catalyst C9200L-24P-4X-E | 4 | Dos por sede, PoE+, VLAN, OSPF, HSRP y uplinks 10G |
| Acceso | TP-Link TL-SG2428P | 5 | 24 puertos PoE+, VLAN, teléfonos, cámaras y AP |
| DMZ | Ubiquiti UniFi USW-24 | 1 | Switch administrable separado para servidores publicados |
| Wireless | HPE Aruba AP-515 | 4 | Wi-Fi 6, WPA2/WPA3 y soporte de IoT |
| Servidores | HPE ProLiant DL320 Gen11 | 1 | Host de virtualización para servicios corporativos |
| Energía | APC SRV3KRIRK-E, 3 kVA | 2 | UPS online rack, una por sede |
| Gabinetes | Rack 42U x 600 x 1000 mm | 2 | Uno por sede, con espacio de crecimiento |

### Decisión sobre servidores

La propuesta base utiliza un servidor físico con máquinas virtuales para DNS,
NTP, correo, Syslog, AAA, web e IoT. Es una alternativa ajustada al límite
presupuestario. Para producción se recomienda agregar un segundo host o una
solución de respaldo, porque un único servidor representa un punto único de
falla.

### Decisión sobre access points

Se consideran cuatro AP como punto de partida: dos para la matriz y dos para
la sucursal. La cantidad definitiva debe confirmarse con un levantamiento de
cobertura y densidad de usuarios. Los SSID deben asociarse a VLAN y usar
WPA2/WPA3.

## 3. Referencias de precios

| Ítem | Fuente | Precio IVA incluido | Observación |
|---|---|---:|---|
| FortiGate 60F | [ZT Network](https://www.ztnetwork.cl/fortigate-60f) | $763.013 | Precio referencial; licencia FortiGuard no incluida |
| C9200L-24P-4X-E | [Tecnomas](https://www.tecnomas.cl/producto/catalyst-9200l-24-port-poe-4-x-10g-network-esse) | $2.151.990 | Fuente indica agotado; requiere cotización formal |
| C9200L-24P-4X-E | [Zona Industrial](https://www.zonaindustrial.cl/shop/switch-cisco-catalyst-9200l-24-puertos-poe-4x10g-uplink-network-essentials-c9200l-24p-4x-e-54179) | $4.675.508 | Alternativa de referencia; diferencia importante de precio |
| TL-SG2428P | [ComparaFácil](https://www.comparafacil.cl/producto/tp-link-tl-sg2428p-tl-sg2428p) | $285.926 | 24 puertos PoE+ y 4 SFP |
| Aruba AP-515 | [Zona Industrial](https://www.zonaindustrial.cl/shop/hpe-aruba-access-point-ap-515-wi-fi-6-dual-radio-68392) | $719.488 | Wi-Fi 6, WPA3 y PoE |
| Cisco C9120AXE-A | [Tecnomas](https://www.tecnomas.cl/producto/c9120ax-external-802-11ax-4x4-4-mimo-iot) | $2.322.990 | Alternativa Cisco; mayor costo |
| HPE DL320 Gen11 | [Zona Industrial](https://www.zonaindustrial.cl/c/servidores) | $10.151.922 | 1U, Xeon, 32 GB; ampliar memoria según virtualización |
| Dell PowerEdge R250 | [Elite Center](https://servidores.elitecenter.cl/servidores-rack/) | Desde USD 1.190 | Requiere cotización en CLP e IVA |
| APC SRV3KRIRK-E | [ComparaFácil](https://www.comparafacil.cl/producto/apc-easy-ups-on-line-3kva2700w-rackmount-2u-srv3krirk-e) | $1.098.900 | 3 kVA, 2U, online |
| Allsai W3k Pro | [Metacom](https://www.metacom.cl/ups-allsaiw-3kva-pro-rt) | $591.309 | Alternativa 3 kVA rack/torre |
| Rack 42U | [Metacom](https://www.metacom.cl/conectividad-organizada/gabinetes/42u) | $617.307 | 600 x 1000 mm, puerta de vidrio |
| Rack 42U | [LK Distribuidor](https://lk.cl/busqueda/RACK-42U) | Desde $1.547.000 IVA aprox. | Precio publicado neto de $1.300.000 |
| Cable Cat6A 305 m | [Metacom](https://www.metacom.cl/) | $233.517 | Precio publicado con IVA |

## 4. Mano de obra separada

La referencia de cobros entregada contempla siete servicios:

1. Diseño, ingeniería y gestión del proyecto.
2. Instalación física de cableado.
3. Configuración y hardening de equipos activos.
4. Certificación, pruebas y documentación as-built.
5. Despliegue, imaging y puesta en marcha.
6. Capacitación al personal de TI.
7. Garantía, soporte y contingencia post-instalación.

La imagen de referencia no fija precios. Los valores de estos servicios deben
agregarse después de definir tarifas, horas/hombre, cantidad de visitas y
duración del soporte. No se suman al subtotal de $30.878.464 de equipamiento.

## 5. Observaciones comerciales

1. El precio del C9200L varía demasiado entre proveedores y uno aparece
   agotado; no debe cerrarse el presupuesto sin una cotización formal.
2. FortiGuard, soporte, transceptores, licencias y renovación anual pueden
   cambiar significativamente el costo del firewall.
3. La alternativa Cisco para AP aumenta el presupuesto; Aruba AP-515 es la
   opción base por costo y WPA3.
4. El switch TP-Link se usa en acceso, no como core: el enrutamiento dinámico,
   HSRP y la redundancia quedan en los Catalyst.
5. El servidor único es una decisión presupuestaria, no la arquitectura de
   máxima disponibilidad.
