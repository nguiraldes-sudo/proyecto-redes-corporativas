# Hallazgos que el equipo debe resolver

Esta lista compara el **`.pkt` construido por Theri** con la documentación de Nico y los archivos de configuración `Proyecto2_PT`; **no cambia ninguno de ellos**. La [auditoría directa](AUDITORIA_DIRECTA_PKT_THERI.md) muestra pruebas de estos hallazgos. Ordenada por riesgo de perder puntos o presentar afirmaciones contradictorias.

## Prioridad máxima: discrepancias observadas en el `.pkt` de Theri

- **H01:** NAT web en R-HQ publica **10.10.1.146**, pero el objeto `Server HTTP` tiene **10.10.1.147/24** y la DMZ opera en **10.10.1.144/28**. Revisar IP/máscara y repetir acceso desde PC externo.
- **H02:** Seis servidores (Server1–Server6) sin IPv4 fija almacenada en sus interfaces; los valores de la guía auxiliar todavía no están acreditados en la topología.
- **H03:** DNS y AAA/ACS aparecen deshabilitados en los siete servidores. Completar configuración y verificar cada servicio.
- **H04:** No se encontró ningún Access Point en los 51 dispositivos. WLAN/SSID exigidos por la pauta sin implementar en este archivo.
- **H05:** Los dispositivos llamados `Termostato` y `Termostato2` son internamente tipo PC. No declararlos termostatos IoT funcionales sin demostrar su función.
- **H06:** SW-DMZ tiene un único uplink a CORE-HQ1. La alta disponibilidad del núcleo no implica redundancia de la DMZ.

## 1. Esquema de VLAN desactualizado — prioridad alta

La descripción antigua de `documentacion/03` presenta **VLAN 120 = DMZ**, **VLAN 90 = IoT** y **VLAN 80 = voz**; `documentacion/05` conserva **VLAN 110 = gestión** en varias reglas. Sin embargo, **la configuración guardada en los switches de Theri** indica:

| VLAN | Configuración en el `.pkt` de Theri |
|---:|---|
| 10 | Gerencia |
| 20 | Finanzas |
| 30 | RR. HH. |
| 40 | TI |
| 50 | Ventas |
| 60 | Operaciones |
| 70 | Voz |
| 80 | IoT |
| 90 | DMZ (solo matriz) |
| 99 | Gestión |
| 999 | Nativa/no usada para usuarios |

**Acción:** tomar el `.pkt` de Theri como fuente principal y considerar `Proyecto2_PT` auxiliar solo cuando coincida; **estas dos carpetas V3 no tocan los originales**. No combinar los dos esquemas en una misma tabla final.

## 2. IPsec aparece como implementada y no implementada — prioridad alta

`documentacion/04` indica en una sección que R-HQ y R-SUC ya tienen IPsec configurada y en otra que R-SUC no reconoce comandos `crypto`. **El `.pkt` de Theri guarda** IKE/ISAKMP, transform-set y crypto maps en los dos extremos, lo que concuerda en buena parte con `07_R-HQ.txt` y `10_R-SUC.txt`.

**Conclusión exacta:** **hay configuración guardada en el `.pkt`**, pero no capturas de negociación ni prueba reproducible actual en los archivos revisados. En el informe usar «configuración presente, funcionamiento sin verificar» hasta obtener `show crypto isakmp sa` y `show crypto ipsec sa` en Packet Tracer.

## 3. Rango presupuestario contradictorio — prioridad alta

`README.md` y `CLAUDE.md` señalan hasta **$35 millones**. La pauta original `Proyectos Redes corporativas.pptx`, diapositiva 10, fija **$20–$30 millones con IVA**. **Christopher informó el 1 de noviembre de 2026 que el profesor autorizó al grupo un máximo de $35 millones**, actualización que prevalece en el presupuesto de trabajo, aunque todavía no se adjuntó un comprobante escrito.

**Acción:** trabajar con el máximo **$35 millones IVA incluido** confirmado por Christopher, conservar constancia del permiso docente para evitar observaciones y conseguir **dos cotizaciones chilenas por ítem relevante**; no presentar precios históricos como ofertas vigentes.

## 4. Evidencia de WLAN/SSID — prioridad alta

La pauta original exige mínimo seis departamentos con VLAN y **SSID/WPA2 o WPA3 por departamento en cada sede**. La documentación de Nico la describe en ciertos lugares como opcional; **en el `.pkt` de Theri no hay puntos de acceso**, por lo que la implementación inalámbrica está pendiente.

**Acción:** acordar y registrar explícitamente con el profesor el alcance Wireless; si no hay dispensa, implementar y mostrar SSID y seguridad por departamento. Conservar la red cableada funcionando por separado.

## 5. Resiliencia parcial de la DMZ — prioridad media

En el `.pkt` de Theri, `SW-DMZ` solo tiene **una conexión física de subida a `CORE-HQ1`**; los textos auxiliares concuerdan. Aunque los núcleos tienen configuraciones HSRP/EtherChannel, la DMZ no tiene la misma redundancia.

**Acción:** no asegurar tolerancia total a fallas para servidores DMZ; ensayar caída de `CORE-HQ1` y documentar el resultado. Diseñar un segundo camino solo si lo permiten Packet Tracer y el alcance acordado.

## 6. Publicación web y controles de acceso — prioridad media

El `.pkt` de Theri muestra NAT estático **HTTP TCP/80 a 10.10.1.146**, aunque `Server HTTP` guarda **10.10.1.147/24**. No se comprobó publicación HTTPS equivalente. Hay que validar el efecto real de las ACL, además de reparar la publicación web en una copia.

**Acción:** probar HTTP desde PC externo, intentar conexiones prohibidas hacia la LAN y documentar acceso a NTP/Syslog/AAA únicamente desde redes permitidas. No declarar que HTTPS externo funciona sin su prueba.

## 7. Contraseñas de demostración en textos del repositorio — prioridad media

Los `.txt` incluyen contraseñas y una clave precompartida con fines académicos; el repositorio está visible públicamente. **No reutilizar** esas credenciales en equipos reales ni copiarlas al nuevo informe. Dejar evidencia de autenticación y SSH sin fotografiar secretos.

## 8. Identidad de la versión Packet Tracer — prioridad alta

Hay varios `.pkt` en `packet-tracer/`, además de `TOPOLOGIA1.pkt` en la raíz. Sus nombres y tamaños difieren. Sin abrir la versión elegida en Packet Tracer y cotejarla con los 15 `.txt`, no se puede saber si los comandos corresponden al archivo que se entregará.

**Acción:** designar un único `.pkt` final y registrar su nombre, fecha, versión de Packet Tracer y capturas de `show` asociadas. Las pruebas históricas de la etapa LAN no sustituyen las pruebas de la V3.
