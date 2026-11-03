# Estado del Proyecto 2 frente a la rúbrica

**Fecha de corte:** 1 de noviembre de 2026  
**Proyecto:** Casa matriz y sucursal seguras · Andes Ingeniería y Servicios SpA (caso académico)  
**Carácter:** auditoría documental de avance, no certificación de funcionamiento.

Esta carpeta es una **copia de trabajo independiente**. No modifica `documentacion/`, `Proyecto2_PT/`, los archivos `.pkt` ni los informes previos.

## Cómo leer el estado

- **Configuración documentada:** hay comandos o parámetros concretos en los 15 archivos `Proyecto2_PT/*.txt`; **no demuestra** que estén cargados ni funcionando en el `.pkt`.
- **Diseño documentado:** el equipo ha definido qué hacer, pero aún no se encontró evidencia verificable de ejecución.
- **Probado:** requiere una captura o salida reproducible de Packet Tracer correspondiente al **mismo archivo `.pkt` y fecha**. No se asigna esta categoría basándonos solo en textos.
- **Pendiente:** falta un entregable o una verificación sustancial.

## Fuente principal y resultado ejecutivo

**Christopher confirmó que Theri hizo la implementación en Cisco Packet Tracer.** El archivo [TRABAJO REDES-backup.pkt](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/packet-tracer/TRABAJO%20REDES-backup.pkt) se pudo leer directamente y se comprobó que coincide byte por byte con la copia compartida en GitHub. Contiene 51 dispositivos, 60 enlaces y configuraciones guardadas de OSPF, HSRP, DHCP, NAT/PAT, IPsec y seguridad. Esta es ahora la **fuente técnica principal**. Los textos de Nico son material auxiliar y no se dan por ciertos cuando contradicen el `.pkt`.

La entrega todavía **no puede declararse terminada**: hay diferencias entre las IP del servidor web y la publicación NAT, seis servidores sin IP estática guardada, DNS/AAA deshabilitados, ausencia de Access Points, dispositivos etiquetados como termostato cuyo tipo interno es PC, y faltan pruebas reproducibles, presupuesto con cotizaciones y defensa.

La pauta original está en **`Proyectos Redes corporativas.pptx`**, diapositivas 7, 10, 14, 15 y 16. Ahí se indica **$20.000.000–$30.000.000 CLP IVA incluido**; **Christopher confirma una autorización posterior del profesor para llegar hasta $35.000.000 CLP**. En esta V3 se acepta ese tope actualizado para planificar, dejando la diferencia registrada y recomendando adjuntar la autorización docente a la entrega.

## Documentos de esta carpeta

1. [MATRIZ_CUMPLIMIENTO.md](MATRIZ_CUMPLIMIENTO.md) — los diez criterios de la pauta, evidencia disponible y lo que falta.
2. [AUDITORIA_DIRECTA_PKT_THERI.md](AUDITORIA_DIRECTA_PKT_THERI.md) — inventario real, conexiones y seis hallazgos verificables extraídos del `.pkt`.
3. [RIESGOS_Y_CONFLICTOS.md](RIESGOS_Y_CONFLICTOS.md) — contradicciones entre documentos antiguos y la implementación real.
4. [PLAN_DE_PRUEBAS.md](PLAN_DE_PRUEBAS.md) — lista concreta de comandos, capturas y resultados que el grupo debe obtener.

## Fuentes revisadas, sin modificarlas

- [README original](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/README.md) y [requisitos del cliente](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/documentacion/01-requisitos-del-cliente.md).
- [Diseño de topología](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/documentacion/02-diseno-de-topologia.md), [VLAN/VLSM](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/documentacion/03-vlan-y-direccionamiento-ip.md), [VPN/DMZ](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/documentacion/04-seguridad-perimetral-y-vpn.md), [matriz de tráfico](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/blob/main/documentacion/05-matriz-de-trafico-y-politicas.md).
- [15 archivos de configuraciones](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/tree/main/Proyecto2_PT) y los archivos conservados en [packet-tracer](https://github.com/nguiraldes-sudo/proyecto-redes-corporativas/tree/main/packet-tracer).
- Presentación de pauta del profesor, **no incorporada físicamente al repositorio**.

**Límite de esta revisión:** el `.pkt` sí fue **decodificado y analizado estructuralmente** fuera de Cisco, en una copia de lectura, sin alterar el original. **No fue ejecutado en Cisco Packet Tracer**, por lo que no se fabrican resultados de `show`, pings, llamadas, asociaciones Wi-Fi ni capturas.
