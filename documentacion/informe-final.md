# Informe técnico

## Proyecto 2: Casa matriz y sucursal seguras

**Cliente académico:** Andes Ingeniería y Servicios SpA  
**Casa matriz:** Santiago  
**Sucursal:** Valparaíso  
**Plataforma de simulación:** Cisco Packet Tracer 9.0  
**Archivo de referencia:** `packet-tracer/TRABAJO REDES-backup.pkt`  
**Fecha de versión:** 2 de noviembre de 2026

> **Estado del documento:** borrador maestro. Las afirmaciones técnicas deben
> actualizarse con capturas y salidas de comandos obtenidas durante las
> pruebas finales. La existencia de una configuración guardada en el `.pkt` no
> equivale por sí sola a una función verificada.

## 1. Resumen ejecutivo

Andes Ingeniería y Servicios SpA requiere conectar de forma segura su casa
matriz y su sucursal, separar los departamentos, proteger los servicios
publicados y facilitar la administración de la infraestructura.

La solución propuesta utiliza una arquitectura jerárquica con routers
perimetrales, dos switches multicapa por sede, switches de acceso, una DMZ
centralizada en la matriz y una red ISP simulada. La configuración contempla
VLAN, HSRP, OSPF, DHCP, NAT/PAT, ACL, IPsec, SSH v2 y controles de hardening.

El diseño reduce el movimiento lateral, separa los servicios públicos de los
usuarios y permite escalar la red. Las funciones que todavía no cuentan con
evidencia de ejecución se presentan como pendientes de validación.

## 2. Situación actual y problema del cliente

La empresa posee conectividad a Internet en ambas sedes, pero necesita
reemplazar un crecimiento poco uniforme por una arquitectura segmentada,
administrable y segura.

Los principales riesgos identificados son:

- redes y departamentos sin separación suficiente;
- exposición potencial de servicios públicos;
- comunicación entre sedes sin evidencia de cifrado operativo;
- falta de políticas uniformes para voz, IoT y administración;
- dificultad para demostrar disponibilidad, control de acceso y crecimiento.

## 3. Objetivos

1. Conectar ambas sedes mediante una solución corporativa segura.
2. Separar departamentos y servicios mediante VLAN y subredes.
3. Implementar una DMZ para los servicios publicados.
4. Proporcionar DHCP, DNS, NTP, correo, Syslog, AAA, web, voz e IoT según el
   alcance validado.
5. Controlar el tráfico mediante ACL y NAT/PAT.
6. Incorporar redundancia de gateway y enlaces dentro de cada sede.
7. Administrar los equipos mediante SSH v2 y controles de hardening.
8. Documentar pruebas, limitaciones, riesgos y costos.

## 4. Alcance y supuestos

La infraestructura principal utiliza enlaces Ethernet por cobre. La WLAN no
está presente en el archivo oficial; se implementará si la rúbrica exige SSID
o se documentará la autorización docente para omitirla.

Los routers `ISP1` e `ISP2` representan al proveedor. Las claves del entorno
son académicas y no deben reutilizarse en sistemas reales.

## 5. Diseño de la solución

### 5.1 Topología física y lógica

La topología oficial contiene 51 dispositivos y 60 conexiones Ethernet:

- `R-HQ` y `R-SUC` como routers perimetrales;
- `ISP1` e `ISP2` como red externa simulada;
- `CORE-HQ1`, `CORE-HQ2`, `CORE-S1` y `CORE-S2` como switches multicapa;
- switches de acceso en ambas sedes;
- `SW-DMZ` y siete servidores en la DMZ de la matriz.

Cada sede posee dos enlaces entre el router perimetral y los cores. Los cores
utilizan HSRP, OSPF y EtherChannel según la configuración guardada. `SW-DMZ`
posee un único uplink hacia `CORE-HQ1`, por lo que la redundancia de la DMZ es
parcial.

### 5.2 Segmentación y direccionamiento

| VLAN | Nombre | Función | Matriz | Sucursal |
|---:|---|---|---|---|
| 10 | Gerencia | Usuarios de gerencia | `10.10.1.128/28` | `10.20.1.16/28` |
| 20 | Finanzas | Administración y finanzas | `10.10.1.0/27` | `10.20.0.160/27` |
| 30 | RRHH | Recursos Humanos | `10.10.1.32/27` | `10.20.0.192/27` |
| 40 | TI | Administración técnica | `10.10.1.64/27` | `10.20.0.224/27` |
| 50 | Ventas | Área comercial | `10.10.0.0/25` | `10.20.0.0/26` |
| 60 | Operaciones | Operaciones | `10.10.0.128/26` | `10.20.0.64/26` |
| 70 | Voz | Telefonía IP | `10.10.0.192/26` | `10.20.0.128/27` |
| 80 | IoT | Cámaras, MCU y termostatos | `10.10.1.96/27` | `10.20.1.0/28` |
| 90 | DMZ | Servicios publicados | `10.10.1.144/28` | No implementada |
| 99 | Gestión | Administración de red | `10.10.1.160/28` | `10.20.1.32/28` |
| 999 | Nativa | VLAN nativa de troncales | Troncales | Troncales |

Los gateways virtuales HSRP documentados son `10.10.1.129` en la matriz y
`10.20.1.17` en la sucursal para la VLAN 10. Los demás gateways deben
comprobarse mediante `show standby brief`.

### 5.3 Enrutamiento y disponibilidad

OSPF se utiliza en el área 0 para intercambiar rutas entre routers y switches
multicapa. HSRP entrega gateways virtuales compartidos y EtherChannel agrupa
enlaces entre los cores. La disponibilidad real debe comprobarse mediante
vecindades, tablas de rutas, estados HSRP, estado del EtherChannel y pruebas de
caída controlada.

## 6. Servicios de red

La propuesta contempla:

- DHCP por VLAN;
- DNS corporativo;
- NTP;
- HTTP/HTTPS;
- correo SMTP/POP3;
- Syslog;
- AAA/TACACS+;
- telefonía IP mediante CME;
- servicio IoT.

La auditoría detectó que seis servidores no tienen IP fija comprobada y que
DNS y AAA aparecen deshabilitados en el estado guardado. Estos servicios no
deben declararse operativos hasta corregirlos y probarlos.

## 7. Seguridad

### 7.1 Perímetro, NAT/PAT y DMZ

La VLAN 90 de la matriz separa los servidores públicos de las redes internas.
NAT/PAT permite la salida de redes privadas hacia el ISP y se configuró una
excepción para el tráfico entre sedes.

Existe un conflicto que debe resolverse: R-HQ publica `10.10.1.146` mediante
NAT estático, mientras la auditoría identificó al servidor web con
`10.10.1.147/24`. La dirección y máscara definitivas deben corregirse en una
copia de trabajo y validarse desde el PC externo.

### 7.2 VPN IPsec

R-HQ y R-SUC contienen configuración ISAKMP, transform-set, claves
precompartidas y crypto maps. La configuración está almacenada, pero la
negociación no está demostrada. Se requieren:

```text
show crypto isakmp sa
show crypto ipsec sa
```

### 7.3 ACL y hardening

La política aplica mínimo privilegio: se permite solo el tráfico necesario
entre departamentos, servicios, sedes y DMZ. También se configuraron SSH v2,
usuarios locales, contraseñas cifradas, banners, VTP transparente,
port-security, PortFast y BPDU Guard.

La efectividad debe demostrarse con pruebas positivas y negativas desde VLAN
autorizadas y no autorizadas.

## 8. Matriz de pruebas y evidencias

El procedimiento completo y el registro de resultados se encuentran en
`documentacion/plan-de-pruebas-y-evidencias.md`.

| ID | Prueba | Resultado esperado | Evidencia |
|---|---|---|---|
| T01 | DHCP en una VLAN de cada sede | IP, gateway y DNS correctos | Pendiente |
| T02 | `show vlan brief` y trunks | VLAN y enlaces permitidos | Pendiente |
| T03 | `show ip ospf neighbor` | Vecindades FULL | Pendiente |
| T04 | `show standby brief` | HSRP activo/standby correcto | Pendiente |
| T05 | `show etherchannel summary` | Port-channel operativo | Pendiente |
| T06 | `show crypto isakmp sa` | Asociación IKE establecida | Pendiente |
| T07 | `show crypto ipsec sa` | Contadores de cifrado aumentan | Pendiente |
| T08 | PC externo a servidor web | Solo servicio publicado accesible | Pendiente |
| T09 | VLAN de usuarios a DMZ | Solo puertos autorizados | Pendiente |
| T10 | SSH desde VLAN 99 | Acceso permitido | Pendiente |
| T11 | SSH desde otra VLAN | Acceso bloqueado | Pendiente |
| T12 | Servicio DNS/AAA/NTP/correo/Syslog/IoT | Servicio funcional | Pendiente |
| T13 | Caída controlada de un core | Recuperación según diseño | Pendiente |

Cada evidencia debe registrar fecha, archivo `.pkt`, equipo, comando o acción,
resultado esperado, resultado real, captura y responsable.

## 9. Riesgos y limitaciones

- La publicación web tiene una discrepancia de IP y máscara.
- Seis servidores no tienen direccionamiento fijo comprobado.
- DNS y AAA no están habilitados en el estado auditado.
- No existe infraestructura WLAN en el `.pkt` oficial.
- Los dispositivos llamados `Termostato` son internamente de tipo PC.
- La DMZ tiene un único enlace al núcleo.
- La configuración IPsec aún no tiene evidencia de negociación.

## 10. Presupuesto

El presupuesto de equipamiento, cableado, licencias, materiales y accesorios
debe mantenerse entre $20.000.000 y $35.000.000 CLP con IVA, según la
autorización docente registrada. La mano de obra, ingeniería, instalación,
capacitación y soporte se presentan como servicios profesionales adicionales.
La sección se completará con dos cotizaciones chilenas por ítem relevante,
equipos vigentes para 2026, cableado Cat6, rack, patch panel y UPS.

No se deben confundir los equipos simulados del `.pkt` con una lista literal
de compras físicas.

La selección preliminar y el cálculo de referencia se encuentran en
`presupuesto/seleccion-de-equipos.md` y `presupuesto/README.md`. El total
preliminar de equipamiento y materiales es de $30.878.464 IVA incluido. La
mano de obra se agregará como un bloque separado según las tarifas y horas
definidas.

## 11. Conclusiones

La solución propuesta entrega una base segmentada y escalable para conectar
Santiago y Valparaíso. Su principal valor es combinar separación por VLAN,
redundancia interna, control perimetral, servicios corporativos y
administración segura.

La entrega final dependerá de completar las pruebas, corregir la DMZ,
habilitar los servicios pendientes y resolver el alcance de WLAN. Hasta
entonces, las funciones se deben presentar como configuradas o pendientes de
verificación, no como completamente operativas.

## 12. Anexos

- Anexo A: diagrama físico y lógico.
- Anexo B: tabla completa de direccionamiento.
- Anexo C: configuraciones seleccionadas.
- Anexo D: capturas de pruebas.
- Anexo E: presupuesto y cotizaciones.
- Anexo F: matriz de cumplimiento de la rúbrica.
