**Instalación y configuración de servicio DHCP**













**Instalación y configuración de servicio DHCP en Windows Server**
Empezamos con instalar el servicio DHCP, para ello seguiremos estos pasos:

      1. Abriremos el Administrador del Servidor.
      
      2. Cuando ya lo tengamos abierto, nos vamos a la parte superior derecha en **Administrar**.
      
      3. Le das a agregar roles y características.



Ahora empezamos a instalar el servicio DHCP.
      1. Le damos a siguiente hasta llegar Roles del Servidor.
      
      2. Agregamos **Servidor DHCP**.
      
      3. No tocaremos nada y al llegar al final le damos a la opción de isntalar.

**Configuración de la IP en el servicio DHCP**
      1. Primero nos vamos a la parte superior derecha y le damos a la opción de Herramientas.

      2. 
















**1. Parámetros Globales**
Definen la configuración por defecto para todos los clientes que soliciten red si no se especifica una regla particular en una subred.

- default-lease-time <segundos>;: Tiempo en segundos que se le asigna la dirección IP a un cliente por defecto (ej. 600).

- max-lease-time <segundos>;: Tiempo máximo en segundos que un cliente puede retener una dirección IP (ej. 7200).

- option domain-name "<nombre>";: Nombre del dominio local (ej. "ejemplo.com").

- option domain-name-servers <IP1, IP2>;: Direcciones IP de los servidores DNS que se entregarán a los clientes (ej. 8.8.8.8, 1.1.1.1).


**2. Declaración de Subred (subnet)**
Es la sección obligatoria donde se define el rango de direccionamiento IP que asignará el servidor.

Plaintext
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.150;
    option routers 192.168.1.1;
    option subnet-mask 255.255.255.0;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 192.168.1.1;
}
subnet ... netmask ...: Define la red de trabajo y su máscara.

range <IP_inicio> <IP_fin>;: Especifica el rango dinámico de IP que el servidor asignará a los equipos.

option routers <IP_gateway>;: Dirección IP de la puerta de enlace (router) por defecto.

option subnet-mask: Máscara de red entregada a los clientes.

option broadcast-address: Dirección de broadcast de la subred.

Esto es un ejemplo perfecto  de como sería a la hora de configurarlo.



      

