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

## Direccionamiento IP desde cero

Una dirección IP identifica a un dispositivo dentro de una red. Es similar a
una dirección postal: permite saber a qué equipo deben llegar los datos.

Ejemplo de una dirección IP:

```text
10.10.10.25
```

La dirección completa no basta para saber qué parte identifica a la red y qué
parte identifica al dispositivo. Para eso usamos una **máscara de subred**.

### Red, máscara y host

Tomemos esta red:

```text
Red:     10.10.10.0/27
Máscara: 255.255.255.224
```

El `/27` significa que 27 bits se reservan para identificar la red. Quedan 5
bits para identificar dispositivos:

```text
2^5 = 32 direcciones totales
32 - 2 = 30 direcciones utilizables
```

Se restan dos direcciones porque una identifica la red y otra se reserva para
broadcast:

| Elemento | Dirección | Significado |
|---|---|---|
| Dirección de red | `10.10.10.0` | Identifica a toda la subred |
| Gateway | `10.10.10.1` | Puerta de salida de la VLAN |
| Hosts utilizables | `10.10.10.2` a `10.10.10.30` | Equipos de usuarios y otros dispositivos |
| Broadcast | `10.10.10.31` | Comunicación hacia todos los hosts de la subred |

Por lo tanto, no se debe asignar `10.10.10.0` ni `10.10.10.31` a un
computador. El gateway usa una dirección utilizable, en este diseño la primera.

### Ejemplo completo

Supongamos que un PC de Administración tiene:

```text
IP:      10.10.10.25
Máscara: 255.255.255.224
Gateway: 10.10.10.1
VLAN:    10
```

Esto significa:

- El PC pertenece a la VLAN 10.
- Está dentro de la red `10.10.10.0/27`.
- Puede comunicarse directamente con otros equipos de `10.10.10.1` a
  `10.10.10.30`.
- Para llegar a otra VLAN o a Internet, envía el tráfico al gateway
  `10.10.10.1`.

### ¿Por qué la sucursal usa 10.20?

La casa matriz y la sucursal deben utilizar redes diferentes. Si ambas usaran
la misma subred, los routers tendrían dificultades para distinguir el destino.

Usaremos esta convención:

```text
10.10.x.x = casa matriz
10.20.x.x = sucursal
```

Por ejemplo:

```text
VLAN 10 matriz:    10.10.10.0/27
VLAN 10 sucursal:  10.20.10.0/28
```

Ambas son Administración, pero no son la misma red. La VPN y el enrutamiento
pueden distinguirlas correctamente.

### ¿Qué significa /28 y /26?

El número después de la barra indica cuántos bits identifican la red. Mientras
mayor sea el número, menor será la cantidad de hosts disponibles:

| Prefijo | Direcciones totales | Hosts utilizables | Uso típico en nuestro diseño |
|---|---:|---:|---|
| `/30` | 4 | 2 | Enlace entre dos routers |
| `/29` | 8 | 6 | Departamento pequeño |
| `/28` | 16 | 14 | Departamento pequeño o infraestructura |
| `/27` | 32 | 30 | Departamento mediano |
| `/26` | 64 | 62 | Departamento grande |

### ¿Por qué no usamos una sola red para toda la empresa?

Si todos los equipos estuvieran, por ejemplo, en `10.10.0.0/16`, existiría una
red demasiado grande y poco controlada. Las VLAN y subredes permiten:

- Separar departamentos.
- Reducir tráfico de broadcast.
- Aplicar ACL entre áreas.
- Detectar más fácilmente el origen de un problema.
- Mantener una estructura ordenada y escalable.

### Resumen visual

```text
VLAN 10 Administración
        │
        └── Subred 10.10.10.0/27
              ├── .0   = red
              ├── .1   = gateway
              ├── .2   = primer host posible
              ├── .25  = ejemplo de PC
              ├── .30  = último host posible
              └── .31  = broadcast
```

**Regla práctica:** primero identificamos la VLAN, luego su subred, después el
gateway y finalmente asignamos IP a cada dispositivo dentro del rango válido.

## Wireless en este proyecto

**Wireless** significa conectividad inalámbrica. Permite que laptops,
celulares u otros dispositivos se conecten a la red mediante ondas de radio,
normalmente a través de un access point, en vez de utilizar un cable Ethernet.

En este proyecto será un componente opcional. La infraestructura principal
seguirá conectada por cobre, mientras que los access points podrán entregar
movilidad adicional.

Conceptos relacionados:

- **Access point:** equipo que ofrece acceso inalámbrico y se conecta por cable
  a la red cableada.
- **SSID:** nombre visible de una red inalámbrica.
- **WPA2/WPA3:** mecanismos de seguridad para proteger el acceso inalámbrico.
- **WLAN:** red local inalámbrica.

La WLAN no debe convertirse en una red plana. Cada SSID debe asociarse a una
VLAN y respetar las ACL de la red cableada. Por ejemplo, invitados deben tener
salida a Internet, pero no acceso a servidores internos.

### 2026-10-21

- Se confirmó que Wireless es opcional según el profesor.
- Se decidió mantener la infraestructura principal por cobre e incorporar WLAN
  como una mejora de movilidad.

## Repaso aplicado al proyecto

La explicación anterior se revisó paso a paso utilizando como ejemplo la red
`10.10.10.0/27`:

- La dirección de red es `10.10.10.0`.
- El gateway de la VLAN puede ser `10.10.10.1`.
- Los hosts utilizables van desde `10.10.10.2` hasta `10.10.10.30`.
- El broadcast es `10.10.10.31`.
- La casa matriz utiliza el bloque `10.10.x.x`.
- La sucursal utiliza el bloque `10.20.x.x`.
- Las redes `/30` se reservan para enlaces entre dispositivos de
  infraestructura.

La secuencia que debemos seguir es:

```text
Departamento → VLAN → subred → gateway → IP de cada dispositivo
```

### 2026-10-21

- Se solicitó guardar el último repaso sobre direccionamiento IP como respaldo
  del aprendizaje.

## Bitácora de aprendizaje

### 2026-10-21

- Se definió que el proyecto debe explicarse paso a paso al estudiante.
- Se documentaron los conceptos IoT, IPsec, DMZ, NAT/PAT, ACL y SSH.
- Se revisaron nuevamente estos conceptos con ejemplos aplicados a la solución
  de casa matriz y sucursal segura.
