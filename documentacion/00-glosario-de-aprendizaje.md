# Glosario de aprendizaje

Este documento reúne las explicaciones de los conceptos técnicos que iremos
utilizando durante el proyecto. Se actualizará a medida que aprendamos nuevos
elementos.

## IoT

**IoT** significa *Internet of Things*, o **Internet de las cosas**. Se refiere
a dispositivos físicos que tienen conectividad de red y pueden enviar o
recibir información.

Ejemplos para este proyecto:

- Cámaras IP.
- Termostatos inteligentes.
- Sensores.
- Dispositivos de control.

En una empresa, los dispositivos IoT no deberían quedar mezclados con los
computadores de los empleados. Por eso los ubicaremos en una VLAN separada y
aplicaremos reglas que limiten a qué servicios pueden acceder.

**Idea sencilla:** IoT es un dispositivo físico que se conecta a la red para
medir, controlar o informar algo.

## IPsec

**IPsec** significa *Internet Protocol Security*. Es un conjunto de protocolos
que protege comunicaciones IP mediante autenticación y cifrado.

En nuestro proyecto se utilizará para crear una **VPN site-to-site** entre la
casa matriz y la sucursal. La comunicación viajará por una red externa, como
Internet, pero los datos estarán protegidos dentro de un túnel lógico.

IPsec aporta principalmente:

- **Confidencialidad:** terceros no deberían poder leer los datos.
- **Integridad:** permite detectar si los datos fueron alterados.
- **Autenticación:** los extremos verifican que se están comunicando con el
  dispositivo correcto.

**Idea sencilla:** IPsec es la protección que convierte una conexión común
entre sedes en una conexión privada y segura sobre Internet.

## DMZ

**DMZ** significa *Demilitarized Zone*, o **zona desmilitarizada**. Es una red
separada donde se colocan servicios que deben ser accesibles desde redes
externas, sin exponer directamente la red interna.

En nuestro proyecto, la DMZ puede contener:

- Servidor web.
- Servidor DNS público.
- Servidor de correo.
- Otros servicios que deban publicarse.

La DMZ queda entre la red externa y las redes internas. Las ACL y las reglas
del firewall controlan qué tráfico puede entrar y salir.

**Ejemplo:** un cliente de Internet puede acceder al sitio web publicado, pero
no debería poder entrar a la VLAN de Recursos Humanos.

**Idea sencilla:** la DMZ es una zona intermedia que protege la red interna
cuando necesitamos publicar servicios.

## NAT/PAT

**NAT** significa *Network Address Translation*, o **traducción de
direcciones de red**. Permite transformar direcciones IP privadas internas en
una dirección pública para comunicarse con Internet.

**PAT** significa *Port Address Translation*. Es una forma de NAT que permite
que muchos dispositivos internos compartan una sola dirección IP pública
utilizando diferentes números de puerto.

**Ejemplo:**

- Un computador interno usa `10.10.10.25`.
- El router traduce esa dirección a una IP pública.
- El puerto de origen permite distinguir sus conexiones de las de otros
  computadores.

Ventajas:

- Ahorra direcciones IPv4 públicas.
- Oculta las direcciones privadas internas.
- Permite que usuarios internos naveguen hacia Internet.

NAT/PAT no reemplaza a un firewall: ocultar direcciones no significa que una
red esté completamente protegida.

**Idea sencilla:** NAT/PAT permite que los equipos internos salgan a Internet
sin usar una dirección pública individual cada uno.

## ACL

**ACL** significa *Access Control List*, o **lista de control de acceso**. Es
un conjunto ordenado de reglas que permite o deniega tráfico según criterios
como:

- Dirección IP de origen.
- Dirección IP de destino.
- Protocolo.
- Puerto TCP o UDP.
- Interfaz y dirección del tráfico.

**Ejemplo:** podríamos permitir que la VLAN de Ventas acceda al servidor web,
pero bloquear su acceso a los servidores de Recursos Humanos.

Las ACL se procesan en orden. Normalmente se aplica la primera regla que
coincide con el tráfico. Si ninguna regla coincide, existe un `deny` implícito
al final de la lista.

**Idea sencilla:** una ACL funciona como un guardia que revisa cada conexión
y decide si puede pasar o no.

## SSH

**SSH** significa *Secure Shell*. Es un protocolo que permite administrar
dispositivos de red mediante una conexión cifrada.

En vez de administrar un router con Telnet, que transmite credenciales de
forma insegura, usaremos SSH versión 2.

Para configurar SSH normalmente se necesita:

- Un nombre de host.
- Un dominio.
- Un usuario local.
- Una clave o contraseña protegida.
- Claves criptográficas RSA.
- `transport input ssh`.
- SSH versión 2.

**Idea sencilla:** SSH es la forma segura de entrar remotamente a routers y
switches para administrarlos.

## Cómo se relacionan en nuestro proyecto

Estos conceptos cumplen funciones distintas:

| Concepto | Función en la red |
|---|---|
| IoT | Conectar cámaras, sensores y termostatos |
| IPsec | Proteger la comunicación entre las dos sedes |
| DMZ | Separar los servidores publicados de la red interna |
| NAT/PAT | Permitir salida a Internet usando traducción de direcciones |
| ACL | Controlar qué tráfico está permitido o bloqueado |
| SSH | Administrar routers y switches de forma segura |

## Resumen para la defensa oral

> La solución separa los dispositivos IoT en una red controlada, conecta las
> sedes mediante una VPN IPsec, ubica los servicios públicos en una DMZ,
> utiliza NAT/PAT para la salida a Internet, aplica ACL para controlar el
> tráfico y usa SSH para administrar los equipos de forma segura.

## Bitácora de aprendizaje

### 2026-10-21

- Se definió que el proyecto debe explicarse paso a paso al estudiante.
- Se documentaron los conceptos IoT, IPsec, DMZ, NAT/PAT, ACL y SSH.
- Se revisaron nuevamente estos conceptos con ejemplos aplicados a la solución
  de casa matriz y sucursal segura.
