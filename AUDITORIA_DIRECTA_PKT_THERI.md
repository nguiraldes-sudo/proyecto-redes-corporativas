# Auditoría directa de la topología creada por Theri

**Fuente principal:** `packet-tracer/TRABAJO REDES-backup.pkt`  
**Identidad del archivo:** SHA-256 `18a404cadd2dbc8e4e0a38c9d44903ecf0512b60391ad5a59cbfd0b0a644460d`  
**Coincidencia con GitHub:** el identificador Git de la copia local es `d535577f118e2f147385b79ac9b6a91010cb6db7`, igual al del archivo ubicado en `packet-tracer/` dentro del repositorio.  
**Fecha de revisión:** 1 de noviembre de 2026.

Christopher confirmó que **Theri desarrolló el trabajo práctico en Cisco Packet Tracer**. Los datos de esta página se extrajeron de la estructura guardada del `.pkt`, no de las descripciones de Nico. La extracción es **de solo lectura**: ningún dispositivo, clave ni cable fue modificado. No se publican las configuraciones completas ni el XML interno porque contienen credenciales de demostración.

## Inventario real del archivo

| Familia del dispositivo según Packet Tracer | Cantidad | Descripción |
|---|---:|---|
| Router | 4 | R-HQ, R-SUC, ISP1 e ISP2 |
| Switch multicapa | 4 | CORE-HQ1, CORE-HQ2, CORE-S1, CORE-S2 |
| Switch | 6 | SW-HQ-A1/A2/A3, SW-DMZ, SW-S-A1/A2 |
| Servidor | 7 | `Server HTTP` y `Server1` a `Server6` |
| PC | 15 | Incluye PC0–PC12 **y dos dispositivos llamados `Termostato` y `Termostato2`, que internamente son PC** |
| Laptop | 1 | Laptop0 |
| Impresora | 4 | Printer0 a Printer3 |
| Teléfono IP | 5 | IP Phone0 a IP Phone4 |
| Componente MCU/IoT | 2 | IoT1, IoT2 |
| Distribuidor de energía | 3 | Power Distribution Device0/1/2 |
| **Total** | **51** | Equipos guardados en el archivo |

Se encontraron **60 conexiones de cable Ethernet**. El atributo interno `FUNCTIONAL=true` aparece en ellas, pero ese indicador guardado **no equivale a un ping exitoso, una VPN negociada o a ver todos los puertos verdes** en Packet Tracer.

## Conexiones troncales y de infraestructura observadas

| Desde | Hacia | Observación |
|---|---|---|
| R-HQ | ISP1 | Uplink WAN de matriz |
| ISP1 | ISP2 | Red de tránsito simulada |
| ISP2 | R-SUC | Uplink WAN de sucursal |
| R-HQ | CORE-HQ1 y CORE-HQ2 | Dos enlaces enrutados, uno por núcleo |
| R-SUC | CORE-S1 y CORE-S2 | Dos enlaces enrutados, uno por núcleo |
| CORE-HQ1 | CORE-HQ2 | Dos enlaces físicos previstos para EtherChannel |
| CORE-S1 | CORE-S2 | Dos enlaces físicos previstos para EtherChannel |
| CORE-HQ1/HQ2 | SW-HQ-A1/A2/A3 | Cada switch de acceso tiene un uplink a cada core |
| CORE-S1/S2 | SW-S-A1/A2 | Cada switch de acceso tiene un uplink a cada core |
| CORE-HQ1 | SW-DMZ | **Único uplink de SW-DMZ** registrado |
| SW-DMZ | Siete servidores | Un enlace por servidor |
| SW-HQ-A3 / SW-S-A2 | IoT1/IoT2 y Termostato/Termostato2 | Dispositivos etiquetados como IoT y termostatos; los dos últimos son de tipo PC |

**Conclusión:** ya está cableada la estructura principal de matriz, ISP y sucursal, incluidos uplinks duplicados en switches de acceso. La disponibilidad real queda pendiente de las pruebas de HSRP, STP, EtherChannel, OSPF y caída de enlaces.

## Configuraciones realmente guardadas en el `.pkt`

| Bloque | Resultado de lectura estática |
|---|---|
| R-HQ y R-SUC | Ambos contienen OSPF, pools DHCP, NAT/PAT, políticas ISAKMP/IPsec, crypto map aplicado en WAN y SSH v2 |
| Cuatro switches core | Contienen `ip routing`, OSPF, interfaces SVI, direcciones virtuales HSRP, EtherChannel y SSH v2 |
| Seis switches de acceso/DMZ | Contienen port-security, PortFast, BPDU Guard, VTP transparent y SSH v2 |
| VLAN de matriz | VLAN 10, 20, 30, 40, 50, 60, 70, 80, 90 (DMZ), 99 (gestión) y 999 (nativa) |
| VLAN de sucursal | VLAN 10, 20, 30, 40, 50, 60, 70, 80, 99 y 999; sin DMZ 90 local |
| Wi-Fi | **No hay ningún dispositivo Access Point/Wireless Router** en el inventario del `.pkt` guardado |

Esta lectura sí prueba que la **configuración está almacenada** en el `.pkt`, avance más sólido que un `.txt` independiente. No prueba que cada comando tenga efecto en ejecución ni que todos los servicios estén operativos.

## Hallazgos críticos comprobados contra el `.pkt`

### H01 — Publicación web: IP y máscara no coinciden

La configuración guardada del router **R-HQ** publica HTTP TCP/80 mediante NAT estático hacia **10.10.1.146**. Sin embargo, la interfaz de red guardada en el dispositivo llamado **`Server HTTP`** tiene **10.10.1.147** y máscara **255.255.255.0 (/24)**, mientras la DMZ del núcleo es **10.10.1.144/28**, gateway **10.10.1.145**.

El desacuerdo de dirección y máscara impide afirmar que la publicación web funciona. Hay que comprobar qué IP debe tener el servidor, configurar una máscara coherente con la VLAN y repetir la prueba desde el PC externo. **No se modificó nada aquí.**

### H02 — Seis servidores no tienen IP de interfaz guardada

En los campos de las interfaces de **Server1–Server6** no aparece una IPv4 estática guardada. La guía `Proyecto2_PT/15_Servidores_y_hosts.txt` propone siete IP fijas entre .146 y .152, pero eso **no demuestra que se hayan aplicado** a los dispositivos de la topología.

Hay que revisar cada servidor en Packet Tracer y documentar su IP, máscara, gateway, DNS y función real, además del resultado de su prueba.

### H03 — Servicios DNS y AAA no habilitados en el estado guardado

Los campos de servicio del archivo muestran **DNS deshabilitado** y **AAA/ACS deshabilitado** en los siete objetos de tipo servidor inspeccionados. Otros servicios aparecen habilitados por defecto, lo cual no garantiza que estén configurados con cuentas, registros o pruebas de funcionamiento.

Este hallazgo contradice el estado «listo» que podría interpretarse del documento auxiliar de servidores. Deben habilitarse y probarse los servicios exigidos por la rúbrica.

### H04 — Falta infraestructura inalámbrica

No se encontró un Access Point ni un Wireless Router como dispositivo independiente en la topología guardada. La rúbrica original pide SSID/WPA2-WPA3 por seis departamentos de cada sede. La WLAN está **pendiente**, salvo autorización expresa del docente que permita otra implementación.

### H05 — Los «termostatos» son PC renombrados

`Termostato` y `Termostato2` están registrados como tipo interno **Pc**. No se deben contar automáticamente como termostatos IoT efectivos o como estaciones de trabajo reales de la empresa. Los objetos IoT1/IoT2 figuran como componentes MCU; su función y registro deben demostrarse.

### H06 — Redundancia incompleta de la DMZ

Los accesos de usuario tienen doble conexión al núcleo; `SW-DMZ` solo está conectado a CORE-HQ1. Una falla de ese core podría cortar la DMZ aunque las VLAN de usuario se recuperen por HSRP. Requiere prueba y decisión de diseño.

## Regla de uso para el informe y el presupuesto

1. **Fuente de verdad:** el `.pkt` de Theri, con nombre/hash de la versión ensayada.
2. **Descripción y explicaciones:** documentos de Nico **solo donde coincidan con el `.pkt`**; las diferencias se anotan como pendientes.
3. **Informe de cliente:** describir lo que se instala, lo que se protege, riesgos que resuelve y criterios de aceptación demostrables.
4. **Presupuesto:** usar el inventario real para justificar capacidades, pero separar equipos **simulados** de equipos **comerciales**. ISP1/ISP2 representan al proveedor, no necesariamente compras del cliente; siete servidores lógicos no implican comprar siete torres físicas.
5. **Pruebas:** ejecutar dentro de Cisco Packet Tracer para distinguir configuración guardada de servicio funcional. No crear pings, capturas ni porcentajes ficticios.

**Presupuesto:** pauta original $20M–$30M CLP IVA incluido; **Christopher confirma autorización posterior del profesor para máximo $35M con IVA**. Se requieren equipos actuales y dos cotizaciones chilenas por ítem relevante. No hay presupuesto validado en esta auditoría.
