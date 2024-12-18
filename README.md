# DEAW-DUAL-prac5

Este proyecto utiliza Vagrant para configurar y provisionar una máquina virtual con Debian Bookworm 64-bit. La VM instala y configura NGINX, clona un repositorio para servir como sitio web estático, y configura un segundo sitio web con autenticación básica. A continuación, se describen los pasos y configuraciones.

# Requisitos Previos

Software necesario:

Vagrant instalado en tu máquina local.
VirtualBox o cualquier otro proveedor compatible con Vagrant.
Acceso a internet para descargar las dependencias.

# Estructura de archivos: Asegúrate de que los siguientes archivos y carpetas estén en el mismo directorio que el Vagrantfile:

```
.
├── Vagrantfile
├── .htpasswd
├── html/
│   └── [archivos del sitio perfecto]
├── pedro
└── perfectweb
```

- .htpasswd: Archivo de autenticación básica para el sitio perfectweb.
- html/: Carpeta que contiene los archivos HTML del sitio perfectweb.
- pedro: Configuración del servidor NGINX para el sitio pedro.
- perfectweb: Configuración del servidor NGINX para el sitio perfectweb.

# Descripción del Vagrantfile

El Vagrantfile realiza las siguientes acciones:

1. Configura la máquina virtual:

- Asigna la caja base debian/bookworm64.
- Configura una red privada con la IP 192.168.57.103.
- Sincroniza la carpeta actual del host con /vagrant dentro de la VM.

2. Máquina virtual:

- Instala nginx y openssl.
- Crea directorios en /var/www/ para los sitios web pedro y perfectweb.
- Clona el repositorio Static Website Example en /var/www/pedro/html.
- Copia el contenido del directorio html/ desde la carpeta sincronizada del host al directorio /var/www/perfectweb/html en la VM.
- Configura permisos para que los directorios sean accesibles por el usuario del servicio web (www-data).
- Copia un archivo .htpasswd para habilitar la autenticación en perfectweb.
- Configura NGINX para servir ambos sitios:
  - pedro: Sin autenticación.
  - perfectweb: Con autenticación básica.
- Reinicia NGINX para aplicar los cambios.

# Uso

1. Levantar la Máquina Virtual
   Ejecuta el siguiente comando en la terminal desde el directorio que contiene el Vagrantfile:

```
vagrant up
```

2. Acceso a los Sitios Web
   Sitio pedro:
   - URL: `http://192.168.57.103`
   - Contenido: Sitio web clonado del repositorio GitHub.
     Sitio perfectweb:

- URL: `http://192.168.57.103/perfectweb`
- Requiere autenticación básica. (Las credenciales se definen en el archivo .htpasswd.)

```
vagrant ssh
```

3. Acceso a la Máquina Virtual
   Para acceder a la VM por SSH:

```
vagrant ssh
```

4. Detener y Eliminar la Máquina Virtual

- Detener la máquina:

```
vagrant halt
```

- Destruir la máquina

```
vagrant destroy
```

5. Configuración Personalizada

- Autenticación Básica
  El archivo .htpasswd define las credenciales para perfectweb. Puedes crear o modificar este archivo usando htpasswd:

```
htpasswd -c .htpasswd usuario
```

- Configuración de NGINX
  Los archivos de configuración de NGINX (pedro y perfectweb) se encuentran en la carpeta del host. Puedes modificarlos y aplicar los cambios reiniciando NGINX dentro de la VM:

```
vagrant ssh
sudo systemctl restart nginx
```

# Notas

1. Si necesitas modificar la configuración, edita el Vagrantfile o los archivos asociados, y reprovisiona la máquina:
2. Si los sitios web no son accesibles, verifica:

- La red privada configurada en 192.168.57.103.
- El estado de NGINX:

```
vagrant ssh
sudo systemctl status nginx
```

3. La autenticación básica solo se aplica al sitio perfectweb.
