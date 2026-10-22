# Requisitos del cliente

## 1. Cliente propuesto

**Nombre:** Andes Ingeniería y Servicios SpA  
**Giro:** empresa chilena de ingeniería, soporte técnico y gestión de proyectos industriales.  
**Casa matriz:** Santiago.  
**Sucursal:** Valparaíso.  
**Perfil:** empresa mediana con información comercial, técnica y administrativa que necesita conectar sus sedes sin exponer sus sistemas internos.

El nombre es una referencia académica para construir el caso de negocio; no representa a una empresa real.

## 2. Situación actual

La empresa posee conectividad a Internet en ambas sedes, pero las redes crecieron sin un diseño uniforme. Existen redes planas, acceso inalámbrico sin separación suficiente, servicios publicados directamente y poca visibilidad sobre los dispositivos conectados.

Esta situación genera los siguientes riesgos:

- Un usuario de un departamento puede alcanzar recursos que no necesita.
- La comunicación entre sedes depende de Internet sin un túnel corporativo seguro.
- Los servidores públicos y los equipos internos no están correctamente separados.
- No existe una política uniforme para invitados, IoT, teléfonos y usuarios corporativos.
- La administración de equipos de red no está suficientemente protegida.
- Un crecimiento de usuarios o servicios puede provocar rediseños costosos.

## 3. Objetivos del proyecto

1. Conectar la casa matriz y la sucursal mediante una VPN IPsec site-to-site.
2. Separar usuarios, servidores, voz, invitados e IoT mediante VLAN y políticas de acceso.
3. Publicar solo los servicios necesarios desde una DMZ.
4. Permitir acceso a Internet mediante NAT/PAT sin exponer las redes internas.
5. Entregar conectividad inalámbrica segura por departamento y una red separada para invitados.
6. Centralizar servicios de infraestructura y facilitar la administración.
7. Incorporar controles de hardening y evidencias verificables.
8. Dejar una arquitectura escalable para aumentar usuarios, dispositivos y servicios.

## 4. Usuarios y departamentos de referencia

| Departamento | Casa matriz | Sucursal | Necesidad principal |
|---|---:|---:|---|
| Administración y Finanzas | 12 | 5 | Acceso a sistemas administrativos |
| Recursos Humanos | 8 | 3 | Información laboral confidencial |
| Ingeniería y Proyectos | 28 | 12 | Archivos técnicos y aplicaciones corporativas |
| Operaciones | 20 | 10 | Sistemas operacionales y comunicación |
| Ventas y Comercial | 12 | 6 | CRM, correo y servicios web |
| Gerencia | 5 | 2 | Acceso prioritario y movilidad |

**Total de referencia:** 85 usuarios en la casa matriz y 38 en la sucursal. Las cifras se utilizan para dimensionar el diseño académico.

## 5. Servicios y dispositivos requeridos

- DHCP por VLAN.
- DNS corporativo.
- NTP para sincronización.
- HTTP/HTTPS corporativo en DMZ.
- Correo corporativo en DMZ.
- Syslog para registro centralizado.
- AAA para autenticación administrativa.
- Telefonía IP para áreas de atención y administración.
- Cámaras IP y termostatos IoT en una red aislada.
- Conectividad Ethernet por cobre como criterio preliminar de implementación.
- Red inalámbrica corporativa por departamento: pendiente de confirmación.
- Red inalámbrica de invitados: pendiente de confirmación.
- Red móvil exclusiva para gerencia: pendiente de confirmación.

## 6. Criterios de aceptación

La solución se considerará aceptada cuando:

- La VPN permita comunicación entre las redes autorizadas de ambas sedes.
- Las VLAN tengan direccionamiento y DHCP funcional.
- OSPF aprenda las rutas internas previstas.
- NAT/PAT permita navegación simulada hacia Internet.
- La DMZ sea alcanzable solo mediante los puertos publicados.
- Las ACL bloqueen tráfico no autorizado y permitan el tráfico justificado.
- Los SSID estén asociados a redes separadas y con seguridad inalámbrica.
- SSH v2, usuarios locales, banners, contraseñas cifradas y port-security estén configurados.
- Las pruebas incluyan comandos `show`, pings, comprobación de rutas y evidencias gráficas.

## 7. Supuestos académicos

- La topología se implementará en Cisco Packet Tracer 9.0.
- Se priorizarán enlaces Ethernet por cobre entre los dispositivos de la
 solución.
- No se asumirá que celulares, laptops o dispositivos IoT puedan conectarse
 de forma inalámbrica hasta confirmar el criterio con el profesor.
- Los routers ISP representan la conectividad del proveedor y no serán administrados por el cliente.
- Los servicios se simularán con los dispositivos disponibles en Packet Tracer.
- La cantidad de usuarios sirve para justificar el diseño y no implica desplegar un equipo final por cada usuario.
- Las claves utilizadas en la simulación serán académicas y no se reutilizarán en sistemas reales.

## 8. Decisiones pendientes de confirmación

1. ¿La exigencia de conexión por cobre aplica a todos los dispositivos,
   incluidos celulares, laptops, cámaras y termostatos?
2. ¿Se permite conservar WLAN como diseño opcional o debe eliminarse por
   completo de la topología?
3. Si se mantienen dispositivos móviles o IoT, ¿se acepta una conexión
   inalámbrica simulada o se exige una conexión Ethernet?
