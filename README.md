# smart_kitchen

Esta es una aplicacion web basada en microservicios, que consta de 5 microservicios, la cual permite generar alertas para evitar inciendos por medio de mediciones de temperatura en la cocina.

### Requisitos previos
***
Lista de tecnologias usadas en el proyecto:
* Azure virtual machines
* Docker swarm
* Apache Spark

### Desarrollo
#### Crear el cluster Docker-swarm
Crear un cluster de Docker Swarm con un nodo corriendo en la maquina de azure vm1 (master) y otro en la maquina de azure vm2 (worker). 
```
$ swarm init --advertise-addr 10.2.0.4
$ sudo docker node ls
```
En la maquina vm2 como worker. 
```
$ sudo docker swarm join --token SWMTKN-1-
4qt4bp8o1jeakj6xtgfsa62esrgb8mq6fyip25444653jv1c2b-cqdk5hl7yf17xi1a943ntw3zo
10.2.0.4:2377
```
***
#### Ejecute en el servidor
Clonar el repositorio que contiene la aplicacion. 
```
$ git clone https://github.com/sslo12/smart_kitchen.git
```
#### Crear las imagenes 
Crear las imagenes de cada servicio en la ruta donde este el Dockerfile. 
```
azureusert@vm1:~/smart_kitchen/microweb1$
$ sudo docker build -t microweb1 .
```
Imagen microautenticacion. 
```
azureuser@vm1:~/smart_kitchen/$ cd microautenticacion
$ sudo docker build -t microautenticacion .
```
Imagen microusuarios. 
```
azureuser@vm1:~/smart_kitchen/$ cd microusuarios
$ sudo docker build -t microusuarios .
```
Imagen micropaquetes. 
```
azureuser@vm1:~/smart_kitchen/$ cd micropaquetes
$ sudo docker build -t micropaquetes .
```
Imagen microdatos. 
```
azureuser@vm1:~/smart_kitchen/$ cd microdatos
$ sudo docker build -t microdatos .
```
Imagen micronotificaciones. 
```
azureuser@vm1:~/smart_kitchen/$ cd micronotificaciones
$ sudo docker build -t micronotificaciones .
```
***
Ejecutar el Docker Swarm. 
```
azureuser@vm1:~/smart_kitchen/$
$sudo docker stack deploy -c docker-swarm.yml stack1
```
Verificar las imagenes creadas. 
```
$sudo docker ps
```
Verificar los servicios. 
```
$sudo docker stack ls
$sudo docker service ls
```
Escalar los servicios. 
```
$sudo docker service scale stack1_db=5
$sudo docker service scale stack1_microautenticacion=2
$sudo docker service scale stack1_microdatos=3
$sudo docker service scale stack1_micronotificaciones=2
$sudo docker service scale stack1_microweb1=4
```
Verificar los servicios replicados. 
```
$sudo docker service ls
```
***
#### Comprobar el funcionamiento en el navegador

```
http://10.2.0.4:1080/microweb
```
