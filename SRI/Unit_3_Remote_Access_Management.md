**Instalación de un Servidor DNS**


**1. Para arreglar el problema debemos de cambiar el nombre del dominio, con el siguiente comando.**

**sudo mv /etc/bind/db.midominio.local /etc/bind/db.midominio.lan**

**2. Actualizar el contenido del archivo**

Abre el archivo recién renombrado con nano, para cambiar el nombre del dominio:

**sudo nano /etc/bind/db.midominio.lan**

$TTL    604800

@       IN      SOA     ns1.midominio.lan. admin-midominio.lan. (

                         4         ; Serial (recuerda subir el serial)
                         
                    604800         ; Refresh
                    
                     86400         ; Retry
                     
                   2419200         ; Expire
                   
                    604800 )       ; Negative Cache TTL
                    
;

@       IN      NS      ns1.midominio.lan.

ns1     IN      A       172.16.5.40

equipo1 IN      A       172.16.5.42

equipo2 IN      A       172.16.5.43

equipo3 IN      A       172.16.5.44



**3. Actualizar el archivo named.conf.local**

No olvides actualizar también la ruta en tu archivo de configuración de zonas:

**sudo nano /etc/bind/named.conf.local**

Modifica la línea del archivo para que apunte al nuevo nombre:

zone "midominio.lan" {

    type master;
    
    file "/etc/bind/db.midominio.lan";
    
};


Ahora reiniciamos el bind9

**sudo systemctl restart bind9**

**Configuración de la zona inversa en  mi servidor DNS**

1. Declarar la zona inversa en named.conf.local:

**sudo nano /etc/bind/named.conf.local**

Ahora añadimos el bloque de la zona inversa usando los primeros 3 octetos de nuestra red:

namedzone "5.16.172.in-addr.arpa" {

    type master;
    
    file "/etc/bind/db.172.16.5";
    
};

2. Crear el archivo de zona inversa

**sudo nano /etc/bind/db.172.16.5**

Poblá el archivo con la siguiente estructura, adaptando las IPs de tus equipos al último octeto (PTR):

$TTL    604800

@       IN      SOA     ns1.midominio.lan. admin-midominio.lan. (

                             1         ; Serial
                             
                        604800         ; Refresh
                        
                         86400         ; Retry
                         
                       2419200         ; Expire
                       
                        604800 )       ; Negative Cache TTL

@       IN      NS      ns1.midominio.lan.

; Mapeo de la IP al nombre (último octeto)

40      IN      PTR     ns1.midominio.lan.

42      IN      PTR     equipo1.midominio.lan.

43      IN      PTR     equipo2.midominio.lan.

44      IN      PTR     equipo3.midominio.lan.

3. Reiniciar el servicio BIND9:

Aplica los cambios reiniciando el dominio:

**sudo systemctl restart bind9**


**Registro de Dominio**
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/d93141e7-be07-431e-8ec2-e8fca9111cd1" />
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/d741b2c6-5dbb-4385-8157-851c30f82d6f" />
