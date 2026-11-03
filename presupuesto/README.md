# Presupuesto y cotizaciones

## Alcance

El presupuesto debe representar una implementación comercial para dos sedes,
con precios en CLP e IVA incluido. El límite de $35.000.000 corresponde al
equipamiento, cableado, licencias, materiales y elementos físicos. La mano de
obra, ingeniería, instalación, capacitación y soporte se presentan como
servicios profesionales adicionales.

Los equipos del archivo `.pkt` son una referencia funcional y no obligan a
comprar exactamente los mismos modelos. La selección comercial debe justificar
capacidad, soporte, seguridad, disponibilidad y crecimiento.

## Categorías que se deben cotizar

| Categoría | Cantidad inicial | Requisitos |
|---|---:|---|
| Routers o firewalls perimetrales | 2 | VPN IPsec, NAT/PAT, ACL, alta disponibilidad o respaldo |
| Switches multicapa | 4 | VLAN, OSPF, HSRP/VRRP, EtherChannel, PoE según necesidad |
| Switches de acceso | 5 o 6 | VLAN, PoE, port-security, STP, gestión SSH |
| Access points | Por definir | WPA2/WPA3, SSID, VLAN, gestión centralizada |
| Servidores | Por definir | Web, DNS, NTP, correo, Syslog, AAA e IoT |
| Rack y organización | 2 sedes | Rack, patch panel, bandejas y organizadores |
| Cableado | Según levantamiento | Cat6, jacks, patch cords, canalización y certificación |
| Energía | 2 sedes | UPS, PDU y protección eléctrica |
| Licencias y soporte | Por equipo | Suscripción, garantía y soporte del fabricante |
| Materiales e instalación física | 2 sedes | Cableado, patch panel, canalización y montaje |
| Mano de obra | 2 sedes | Se presupuesta separadamente de los equipos |

## Reglas de cotización

- Buscar al menos dos proveedores chilenos por ítem relevante.
- Registrar modelo, proveedor, URL, fecha de consulta, precio neto, IVA y total.
- Preferir equipos vigentes y disponibles para 2026.
- Separar hardware, licencias, instalación y soporte.
- No contar `ISP1` e `ISP2` como equipos comprados por el cliente: representan
  al proveedor en la simulación.
- No asumir que siete servidores lógicos implican siete servidores físicos.

## Plantilla

| Categoría | Ítem/modelo | Cant. | Proveedor | URL | Precio neto | IVA | Total | Justificación |
|---|---|---:|---|---|---:|---:|---:|---|
| Pendiente | Pendiente | 0 | Pendiente | Pendiente | $0 | $0 | $0 | Pendiente |

## Presupuesto preliminar de equipamiento y materiales

Este cálculo utiliza la selección documentada en
`seleccion-de-equipos.md`. Es una estimación con precios públicos, no una
cotización formal. No incluye mano de obra.

| Categoría | Modelo de referencia | Cant. | Unitario IVA incl. | Subtotal |
|---|---|---:|---:|---:|
| Firewalls | FortiGate 60F | 2 | $763.013 | $1.526.026 |
| Switches core | Cisco C9200L-24P-4X-E | 4 | $2.151.990 | $8.607.960 |
| Switches acceso | TP-Link TL-SG2428P | 5 | $285.926 | $1.429.630 |
| Switch DMZ | Ubiquiti USW-24 | 1 | $418.492 | $418.492 |
| Access points | HPE Aruba AP-515 | 4 | $719.488 | $2.877.952 |
| Servidor | HPE ProLiant DL320 Gen11 | 1 | $10.151.922 | $10.151.922 |
| UPS | APC SRV3KRIRK-E 3 kVA | 2 | $1.098.900 | $2.197.800 |
| Rack | Rack 42U x 600 x 1000 mm | 2 | $617.307 | $1.234.614 |
| Cableado | Cable Cat6A 305 m | 4 | $233.517 | $934.068 |
| Licencias, soporte y accesorios | Provisión inicial | 1 | $1.500.000 | $1.500.000 |
| **Total equipamiento y materiales** |  |  |  | **$30.878.464** |

El subtotal queda dentro del límite de $35.000.000 CLP con IVA. Existe un
margen preliminar de **$4.121.536** para completar patch panels, PDU,
transceptores, canalización, certificación de materiales y ajustes de
cotizaciones sin superar el límite de equipamiento.

## Mano de obra y servicios profesionales

Estos conceptos se cobran aparte del límite de equipamiento. La imagen
entregada se utiliza únicamente como referencia de los ítems y alcances; no
contiene precios que deban incorporarse al presupuesto. Los valores se
definirán posteriormente según horas/hombre, tarifas, visitas y duración del
soporte:

| Ítem | Alcance | Rol/responsable | Precio IVA incluido |
|---|---|---|---:|
| Diseño, ingeniería y gestión del proyecto | Levantamiento, diagramas, VLAN/IP, selección y gestión | Ingeniero de redes / jefe de proyecto | Pendiente |
| Instalación física de cableado | 49 enlaces, canalización, rack y terminaciones | Técnico instalador certificado | Pendiente |
| Configuración y hardening | Core L3, VLAN, SVI, routing, port-security y hardening | Técnico certificado en redes | Pendiente |
| Certificación, pruebas y documentación as-built | Certificación de puntos, pruebas y registro | Técnico de redes | Pendiente |
| Despliegue, imaging y puesta en marcha | Instalación, unión a la red, pruebas y etiquetado de 42 equipos | Técnico de soporte | Pendiente |
| Capacitación al personal de TI | Topología, VLAN, direccionamiento y troubleshooting | Ingeniero de redes | Pendiente |
| Garantía, soporte y contingencia postinstalación | 6 meses, visitas presenciales y contingencia | Servicio de soporte técnico | Pendiente |
| **Total servicios profesionales** |  |  | **Pendiente** |

El número de 49 enlaces y 42 equipos debe validarse contra el levantamiento
final. No se debe incorporar mano de obra al subtotal de equipamiento.

## Subtotales

| Grupo | Total IVA incluido |
|---|---:|
| Equipamiento activo | $14.860.060 |
| Servidores y almacenamiento | $10.151.922 |
| Infraestructura física y cableado | $2.168.682 |
| Energía y respaldo | $2.197.800 |
| Licencias, soporte y accesorios | $1.500.000 |
| **Total equipamiento y materiales** | **$30.878.464** |
| Servicios profesionales | Pendiente |
| **Total general del proyecto** | $30.878.464 + servicios profesionales |
