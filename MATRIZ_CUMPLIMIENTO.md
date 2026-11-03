# Matriz de cumplimiento de la rúbrica oficial

**Pauta:** `Proyectos Redes corporativas.pptx`, diapositivas 14 y 15. Los pesos suman 100%. La clasificación de abajo es **estado de evidencias**, no nota ni porcentaje de logro. La **fuente principal** es el `.pkt` construido por Theri, analizado de forma estática; los archivos de Nico solo sirven de respaldo.

| Criterio de la rúbrica | Peso | Qué está disponible | Qué falta para demostrar cumplimiento excelente | Estado verificable |
|---|---:|---|---|---|
| Diseño y topología | 10% | El `.pkt` de Theri contiene **51 dispositivos y 60 enlaces**: dos sedes, dos routers ISP, cuatro core, seis switches de acceso/DMZ | Captura final y validación de puertos, cableado y funcionamiento físico | **Estructura confirmada en `.pkt`**, operación por probar |
| Direccionamiento VLSM | 10% | SVI, gateway HSRP y DHCP están **guardados en el `.pkt`** | Tabla unificada, IP reales de hosts, corregir servidor web **/24 frente a DMZ /28** y seis servidores sin IP fija guardada | **Parcial; inconsistencia comprobada** |
| VLAN, troncales e inter-VLAN | 10% | VLAN 10–80/99 en ambos core y 90 DMZ en HQ; trunks y EtherChannel **guardados en `.pkt`** | `show vlan brief`, `show interfaces trunk`, ping autorizado y estado de puertos | Configuración en `.pkt`; conectividad sin acreditar |
| Enrutamiento OSPF | 15% | Router-ID, OSPF área 0 y redes **guardados en routers y core del `.pkt`** | `show ip ospf neighbor`, `show ip route ospf`, conectividad y rutas en ambas sedes | Configuración en `.pkt`; convergencia sin acreditar |
| Servicios DHCP/DNS/VoIP/servidores | 10% | DHCP/CME guardados; siete objetos servidor en la DMZ | Corregir IP: seis servidores sin IPv4 fija; DNS y AAA aparecen deshabilitados; capturar pruebas de cada servicio, registro/llamadas VoIP e IoT | **Parcial con problemas comprobados** |
| Seguridad | 10% | SSH v2, VTP transparent, port-security, PortFast y BPDU Guard **guardados en el `.pkt`** | Pruebas positivas/negativas de ACL/SSH y controles de acceso; cuentas por área | Configuración en `.pkt`; efectividad sin acreditar |
| Requisito central P2: IPsec/NAT/DMZ | 15% | Dos extremos con ISAKMP/IPsec, PAT, NAT exemption y publicación web **guardados en `.pkt`** | SAs/contadores IPsec, traducciones PAT, bloqueo de LAN y **corregir NAT web a .146 vs servidor web en .147** | **Parcial con conflicto crítico** |
| Presupuesto y equipos 2026 | 10% | Hay propuesta de costeo preliminar en informes anteriores, **no una planilla de cotizaciones verificadas**; Chris confirma **permiso docente para hasta $35M con IVA** frente a $30M en PPTX | Justificar el tope excepcional, equipos actuales, dos cotizaciones chilenas por ítem relevante, fechas, links, licencias y costo IVA incluido | Pendiente |
| Pruebas e informe técnico | 5% | Documentación de proyecto y borrador V3 separado | Evidencias reales, capturas fechadas, resultados positivo/negativo, anexos y PDF final coherente con `.pkt` | En elaboración |
| Defensa oral | 5% | Resúmenes pedagógicos en documentación | Presentación 15 minutos, reparto de roles, demostración ensayada y respuestas a preguntas | Pendiente |

## Requisitos propios del Proyecto 2

| Requisito | Evidencia en archivos | Decisión de auditoría |
|---|---|---|
| Matriz y sucursal más dos routers ISP | Conexiones WAN y equipos presentes en `.pkt` | **Estructura comprobada**; comportamiento por probar |
| Seis departamentos, VLAN y SSID por sede | VLAN 10–60 guardadas en cuatro core; **no hay Access Point** en `.pkt` | VLAN documentada en la configuración; WLAN **pendiente** |
| Dos switches multicapa por sede | Cuatro core físicos y conexiones en `.pkt` | **Estructura comprobada**; HSRP/redundancia por probar |
| VPN IPsec entre sedes | Crypto maps guardados en R-HQ y R-SUC del `.pkt` | Configurada estáticamente; túnel negociado **no demostrado** |
| NAT/PAT y excepción VPN | ACL-NAT y PAT guardados | Falta traducción operativa y corregir publicación web |
| DMZ con HTTP, DNS, NTP, correo, Syslog, AAA | Siete servidores físicamente conectados a SW-DMZ | Seis sin IP fija guardada, DNS/AAA apagados; funciones sin demostrar |
| DHCP por VLAN, VoIP, cámaras, termostatos | Pools, teléfonos y dos MCU en `.pkt` | Los dos objetos `Termostato` son tipo PC; no dar por cumplido IoT sin prueba |
| OSPF interno y hardening | Comandos guardados en `.pkt` | Configuración visible; validación viva faltante |

## Lo que **no** debe marcarse como terminado todavía

- No afirmar "IPsec operativa" solo porque los comandos están guardados: verificar negociación y contadores de paquetes cifrados.
- No dar por activos DNS/AAA ni la DMZ web: hay conflictos confirmados entre IPs, máscaras y servicios guardados.
- No usar el plan VLAN antiguo (DMZ 120 / IoT 90 / Voz 80 / Gestión 110) como si describiera la configuración `Proyecto2_PT` de hoy.
- No confundir la lista de servicios que se deben activar manualmente en Packet Tracer con servicios ya comprobados.
- No asignar nota estimada sin las pruebas del `.pkt` y las cotizaciones.
- No considerar opcional la WLAN por el solo README: la **diapositiva 7** de la pauta exige SSID por departamento. Si el docente autorizó omitirlo, dejar constancia de ese cambio.

La evidencia de los hallazgos está en [AUDITORIA_DIRECTA_PKT_THERI.md](AUDITORIA_DIRECTA_PKT_THERI.md), sin divulgar el XML interno que contiene credenciales de práctica.

## Próximo cierre de revisión

Al agregar evidencias, cada fila debe registrar: fecha, `.pkt` exacto, equipo, comando/prueba, resultado esperado, resultado real, archivo de captura y responsable. Con esos datos se podrá cambiar "documentado" por "probado" sin especulación.
