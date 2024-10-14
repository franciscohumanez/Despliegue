# Práctica 2.2 Autenticación en Nginx

## Paquetes necesarios
Para esta práctica podemos utilizar la herramienta openssl para crear las contraseñas.
En primer lugar debemos comporbar si el paquete esta instalado `dpkg -l | grep openssl`. Y si no, instalarlo.

## Creación de usuarios y contraseñas para el acceso web
Creamos un archivo oculto llamado `.htpasswd` en el directorio de configuración `/etc/nginx` donde guardar nuestros usuarios y contraseñas:
~~~
sudo sh -c "echo -n 'vuestro_nombre:' >> /etc/nginx/.htpasswd"
~~~
![Creacion del archivo .htpasswd](img/17.png)

Creamos una contraseña para el usuario:
~~~
sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"
~~~
![Creacion de la contraseña](img/18.png)