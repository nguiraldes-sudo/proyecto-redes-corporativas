# Seguridad perimetral y VPN

## 1. ¿Qué problema resolvemos?

La empresa necesita que sus dos sedes se comuniquen, que algunos servicios
sean visibles desde Internet y que los usuarios puedan navegar. Sin controles,
esto podría exponer las redes internas.

La solución separa estas funciones:

- **VPN IPsec:** protege el tráfico entre sedes.
- **DMZ:** contiene servicios que deben publicarse.
- **NAT/PAT:** permite la salida de usuarios internos a Internet.
- **ACL:** decide qué tráfico está permitido o bloqueado.

## 2. DMZ

La DMZ será una red separada de las VLAN de usuarios. En la casa matriz se
reservará inicialmente:

```text
Red DMZ:     10.10.120.0/28
Gateway DMZ: 10.10.120.1
```

Los servidores públicos podrán utilizar, por ejemplo:

```text
Servidor web:    10.10.120.10
Servidor correo: 10.10.120.11
Servidor DNS:    10.10.120.12
```

El gateway no significa que todos los servicios estén abiertos. Las ACL o las
reglas del firewall deberán permitir solo los puertos necesarios:

| Servicio | Puerto habitual | Motivo |
|---|---:|---|
| HTTP | TCP 80 | Sitios web sin cifrado |
| HTTPS | TCP 443 | Sitios web cifrados |
| DNS | UDP/TCP 53 | Resolución de nombres |
| SMTP | TCP 25 o 587 | Correo saliente o autenticado |

La implementación final dependerá de los dispositivos disponibles en Packet
Tracer y del alcance que confirme el profesor.

## 3. VPN IPsec site-to-site

Una VPN site-to-site conecta redes completas, no solo un computador. En nuestro
caso, el router de la matriz y el router de la sucursal negociarán un túnel
IPsec a través de los routers ISP.

```text
Red matriz ─ Router matriz ═══ túnel IPsec ═══ Router sucursal ─ Red sucursal
                         \___ red ISP externa ___/
```

Cuando un equipo de la sucursal accede a una red autorizada de la matriz, el
tráfico debe:

1. Llegar al gateway local.
2. Ser identificado como tráfico interesante de la VPN.
3. Ser cifrado por IPsec.
4. Atravesar la red ISP.
5. Ser descifrado en el router de la matriz.
6. Llegar al destino permitido.

La VPN no significa que todo el tráfico de Internet viaje por el túnel. En el
diseño se debe distinguir entre:

- **Tráfico corporativo entre sedes:** protegido por IPsec.
- **Tráfico hacia Internet:** sale mediante NAT/PAT local, según la política.

## 4. NAT/PAT

Las redes internas usan direcciones privadas que no se enrutan directamente
por Internet. El router perimetral traduce esas direcciones.

```text
PC matriz 10.10.30.25
      ↓
Router matriz: NAT/PAT
      ↓
Dirección pública simulada
      ↓
ISP / Internet
```

PAT permite que varios computadores compartan una dirección pública usando
puertos diferentes.

Una regla importante es que el tráfico destinado a la VPN no debe traducirse
antes de cifrarse. En una configuración real se usa una excepción de NAT
(`NAT exemption` o política equivalente) para el tráfico entre las redes de
ambas sedes.

## 5. ACL

Las ACL aplicarán el principio de **mínimo privilegio**: permitir solo lo
necesario y bloquear lo demás.

Ejemplos de políticas:

- Administración puede acceder a servicios administrativos autorizados.
- Ingeniería puede acceder a servidores de proyectos.
- Invitados, si se aprueban, solo pueden salir a Internet.
- IoT puede comunicarse con su servidor de gestión, pero no con las VLAN de
  usuarios.
- Internet puede acceder al servidor web de la DMZ únicamente por HTTPS.
- La administración de routers y switches solo se permite desde la VLAN de
  gestión mediante SSH.

La política debe documentar siempre:

```text
Origen → Destino → Servicio/puerto → Acción → Justificación
```

## 6. Flujo de tráfico permitido

### Usuario interno hacia Internet

```text
VLAN de usuario → switch multicapa → router perimetral
→ ACL → NAT/PAT → ISP → Internet
```

### Sucursal hacia un servidor interno de la matriz

```text
VLAN sucursal → router sucursal
→ IPsec → ISP → router matriz
→ ACL → servidor autorizado
```

### Internet hacia el servidor web

```text
Internet → router/firewall matriz
→ ACL/publicación del servicio → DMZ → servidor web
```

### Internet hacia una VLAN interna

```text
Internet → router/firewall → bloqueo por política
```

## 7. Diferencia entre las funciones

| Función | Pregunta que responde |
|---|---|
| DMZ | ¿Dónde colocamos los servidores que deben publicarse? |
| IPsec | ¿Cómo protegemos la comunicación entre las sedes? |
| NAT/PAT | ¿Cómo salen muchos equipos internos hacia Internet? |
| ACL | ¿Qué tráfico permitimos o bloqueamos? |

No son funciones intercambiables. NAT no reemplaza una ACL, una ACL no cifra
el tráfico y una DMZ no protege por sí sola los servidores.

## 8. Decisiones pendientes

- **VPN IPsec:** Ya configurada en R-HQ y R-SUC con IKE/AES-256/SHA/DH2/PSK. 
  El router IOS simulado en Packet Tracer 9.0 soporta ahora `crypto isakmp`
  y `crypto ipsec` (a diferencia de la versión previa). Ver documento 04 para
  detalles de configuración.
- Definir si la DMZ será solo en la casa matriz o en ambas sedes.
- Confirmar los servicios que realmente se simularán.
- Definir las IP públicas simuladas de los routers perimetrales.
- Especificar la matriz final de ACL.
- Validar cómo se implementará la excepción de NAT para la VPN (NAT exemptión
  para tráfico entre sedes).

## 9. Resultado de la implementación en Packet Tracer

Se comprobó conectividad entre la matriz y la sucursal mediante rutas
aprendidas por OSPF. Sin embargo, el router de sucursal utilizado en Packet
Tracer no reconoce `crypto isakmp` ni `crypto ipsec`; al ejecutar `crypto ?`
solo ofrece operaciones de claves (`crypto key`).

Por esta limitación del IOS simulado, IPsec queda como parte del diseño
requerido y documentado, pero no como una función demostrada en el archivo
`.pkt`. La conectividad entre sedes se mantiene mediante OSPF para validar el
enrutamiento. En una implementación real se debe seleccionar un equipo con
soporte explícito para IKE/ISAKMP e IPsec.

## Resumen para la defensa oral

> La red separa los servicios públicos en una DMZ, protege la comunicación
> privada entre sedes mediante IPsec, usa NAT/PAT para la salida controlada a
> Internet y aplica ACL basadas en mínimo privilegio. Así evitamos exponer las
> VLAN internas y justificamos cada control de seguridad.

## Bitácora

### 2026-10-21

- Se documentó el diseño preliminar de DMZ, VPN IPsec, NAT/PAT y ACL.
- Se explicó la diferencia entre tráfico entre sedes y tráfico hacia Internet.
- Se dejó pendiente validar la implementación exacta en Packet Tracer.
