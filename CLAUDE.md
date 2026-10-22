# Instrucciones del proyecto

## Contexto

Este repositorio corresponde al Proyecto 2 de Redes Computacionales:
**Casa matriz y sucursal seguras**. El trabajo se presenta como una solución
de redes y telecomunicaciones que debe venderse a un cliente empresarial.

## Objetivo técnico

Diseñar y documentar una solución implementable en Cisco Packet Tracer 9.0
con VPN IPsec site-to-site, VLAN, OSPF, DMZ, NAT/PAT, ACL, DHCP, servicios
corporativos, IoT y hardening.

## Reglas de trabajo

- Explicar al estudiante qué se está haciendo y por qué.
- Mantener la documentación en español.
- Actualizar los archivos Markdown cuando se tome una decisión importante.
- No presentar como confirmado algo que aún esté pendiente de validar con el
  profesor.
- Mantener el presupuesto entre $20.000.000 y $35.000.000 CLP.
- Usar equipos vigentes y justificar las decisiones técnicas y comerciales.
- No incluir credenciales reales, tokens ni datos sensibles.
- Usar conexiones Ethernet por cobre como criterio preliminar.
- Mantener WLAN, celulares, laptops y dispositivos IoT inalámbricos pendientes
  hasta confirmar la instrucción del profesor.

## Archivos importantes

- `README.md`: resumen, checklist y bitácora principal.
- `CONTRIBUTING.md`: flujo de colaboración con Git.
- `documentacion/00-glosario-de-aprendizaje.md`: conceptos explicados paso a paso.
- `documentacion/01-requisitos-del-cliente.md`: cliente y necesidades.
- `documentacion/02-diseno-de-topologia.md`: diseño inicial.
- `documentacion/03-vlan-y-direccionamiento-ip.md`: VLAN y direccionamiento.
- `documentacion/04-seguridad-perimetral-y-vpn.md`: DMZ, VPN, NAT/PAT y ACL.

## Sincronización

Antes de trabajar:

```bash
git pull origin main
```

Al terminar:

```bash
git add .
git commit -m "Describir el cambio"
git push origin main
```

No editar simultáneamente el mismo archivo `.pkt` entre integrantes.

## Estado actual

El cliente académico es **Andes Ingeniería y Servicios SpA**, con casa matriz
en Santiago y sucursal en Valparaíso. El repositorio ya contiene la
documentación inicial de requisitos, topología, VLAN, direccionamiento y
seguridad perimetral.
