# Despliegue de Microservicios con Vagrant
## Objetivo
El objetivo de este laboratorio es implementar un sistema de microservicios en un entorno virtualizado utilizando Vagrant. Se desplegarán varias máquinas virtuales que alojarán diferentes servicios, todos ejecutándose dentro de contenedores Docker. La red privada entre las máquinas permitirá que los microservicios se comuniquen entre ellos, simulando un entorno distribuido.

## Preparación de Entorno
1. Instalar Vagrant: Asegúrate de tener Vagrant instalado.

2. Instalar VirtualBox: Este laboratorio utiliza VirtualBox como proveedor. Puedes descargarlo aquí.

3. Acceso a Docker Hub: Asegúrate de tener conexión a Docker Hub para poder descargar las imágenes.

## Estructura del Vagrantfile
Este laboratorio configura 6 máquinas virtuales:

* Auth API: Maneja la autenticación de usuarios.
* Users API: Proporciona información sobre usuarios.
* Log Message Processor: Gestiona los mensajes de log y se conecta a Redis.
* Todos API: Maneja las tareas pendientes de los usuarios.
* Frontend: Interfaz gráfica que consume los otros servicios.
* Redis: Almacén de datos en memoria para la comunicación entre servicios.

Cada máquina se aprovisiona utilizando Docker para descargar y ejecutar las imágenes correspondientes a los microservicios. La red entre las máquinas es privada, lo que permite la comunicación interna sin exponer servicios a Internet.

## Descripción de los Servicios

Cada uno de los microservicios está configurado con una IP estática y se despliega en su propio contenedor Docker. A continuación, se detallan algunos servicios clave:

#### Auth API
* IP: 192.168.50.2
* Puerto: 8000
* Imagen Docker: jorgeeduardo24/auth-image:0.1.0
* Descripción: Servicio encargado de manejar la autenticación y los tokens JWT.

#### Users API
* IP: 192.168.50.3
* Puerto: 8083
* Imagen Docker: jorgeeduardo24/users-image:0.1.0
* Descripción: Proporciona información de los usuarios y se comunica con el Auth API.

#### Todos API
* IP: 192.168.50.6
* Puerto: 8082
* Imagen Docker: jorgeeduardo24/todos-image:0.1.0
* Descripción: Maneja las tareas pendientes de los usuarios y se conecta a Redis para la comunicación de logs.

## Verificación de los Microservicios
Una vez que las máquinas estén corriendo, abre tu navegador y accede a los siguientes puertos para verificar que los servicios estén funcionando:

* Frontend: http://192.168.50.7:8080
* Auth API: http://192.168.50.2:8000
* Users API: http://192.168.50.3:8083

## Archivos importantes
* Vagrantfile: Configuración de las máquinas virtuales.
* Logs de arranque: Puedes encontrar los logs de arranque de las máquinas virtuales en el directorio .vagrant.