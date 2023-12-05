<h1 align="center"> Proyecto DHCP - DHCP en máquina virtual </h1>

## Antes de empezar
Lo primero es ir a git y crear un nuevo repositorio. Después de esto, creamos nuestro nuevo proyecto en el Code. Una vez listo esto, creamos también un nuevo proyecto en Git y lo asociamos al repositorio creado anteriormente. 

## docker-compose.yml
```
version: '3'
services:
  dhcpd:
    container_name: isc-dhcp-server
    network_mode: "host"
    image: networkboot/dhcpd
    volumes:
      - ./data:/data
```
Usamos en network_mode: "host". De este manera, la pila de red de ese contenedor no está aislada del host de Docker (el contenedor comparte el espacio de nombres de red del host) y el contenedor no obtiene su propia dirección IP asignada. Por ejemplo, si ejecuta un contenedor que se une al puerto 80 y utiliza host la red, la aplicación del contenedor está disponible en el puerto 80 en la dirección IP del host.

## Configuración fichero dhcpd.conf
```
subnet 10.0.2.0 netmask 255.255.255.0 {    
    range 10.0.2.100 10.0.2.200;    
    option routers 10.0.2.1;    
    option domain-name-servers 10.0.2.10, 10.0.2.11;    
}


subnet 10.0.9.0 netmask 255.255.255.0 {
#  range 10.0.9.150 10.0.9.159;
  option domain-name-servers 8.8.8.8, 193.146.92.2, 193.146.96.3;
  option domain-name "asir.tv";
  option routers 10.0.9.254;
  option broadcast-address 10.0.9.255;

  default-lease-time 600;
  max-lease-time 7200;
}

host kleo {hardware ethernet 08:00:27:ce:3f:74;
fixed-address 10.0.9.28;
}

group {
    default-lease-time 604800;
    max-lease-time 691200;

}
```

Una vez lista la anterior configuración podemos arrancar el servicio. Si es la primera vez que arrancamos el servidor DHCP, fallará si no existe el archivo dhcpd.leases. Lo podemos crear mediante comando o interfaz.

Para arrancar el servicio, ejecutamos sudo docker-compose up. Si hacemos cualquier modificación en el archivo de configuración, tendremos que parar y volver a levantar el servicio para que se apliquen los cambios.

En el fichero de dhcpd.leases, podemos ver todas las IP que ha asignado nustro servidor DHCP.

