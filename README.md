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

