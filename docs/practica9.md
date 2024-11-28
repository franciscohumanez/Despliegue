# Práctica 3.5 - Despliegue de una aplicación Flask
## Procedimiento
Instalamos el gestor de pauetes de Python `pip`:
````
sudo apt update

sudo apt install python3-pip

````
![af](img/55.png)

Instalamos `pipenv`
````
sudo apt install pipenv
````
![af](img/56.png)

Después de haberlo instalado comprobamos que está instalado correctamente con `pipenv --version`
![af](img/57.png)

Seguido de esto creamos un directorio en el que almacenaremos nuestro proyecto `sudo mkdir /var/www/nombre_mi_aplicacion
`
![af](img/58.png)

Si hemos creado el directrio con sudo, tenemos que cambiarle los permisos para que el dueño sea nuestro usuario (ud24) y pertenezca al grupo `www-data`
````
sudo chown -R $USER:www-data /var/www/mi_aplicacion

chmod -R 775 /var/www/mi_aplicacion   
````
![af](img/59.png)

Ahora, tenemos que crear un archivo oculto `.env` dentro de nuestra aplicación
![af](img/60.png)

Hay que modificar el arvhivo y añadir las variables, indicando cual es el arvhivo `.py` de la aplicación.
![alt text](img/61.png)

Iniciamos el entorno virtual `pipenv shell`
![alt text](img/62.png)

Instalamos las dependencias de nuestro proyecto `pipenv install flask gunicorn`
![alt text](img/63.png)

Vamos a crear la aplicación Flask más simple posible `touch application.py wsgi.py`
![alt text](img/64.png)

Añadimos la siguiente configuración a los archivos:
application.py:
````
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    '''Index page route'''

    return '<h1>Aplicacion desplegada</h1>'
````

wsgi.py
````
from application import app

if __name__ == '__main__':
    app.run(debug=False)
````
![alt text](img/65.png)
![alt text](img/66.png)

Lanzamos la aplicación con el comando `flask run --host '0.0.0.0'`
![alt text](img/67.png)

Podemos acceder desde nuestra máquina anfitriona accediento a la Ip de la máquina virtual y el puerto `5000`
![alt text](img/68.png)

Si ha funcionado lo anterior, vamos a comprobar que funciona correctamente usando Gunicorn. `gunicorn --workers 4 --bind 0.0.0.0:5000 wsgi:app`
![alt text](img/69.png)

Todavía dentro de nuestro entorno virtual, debemos tomar nota de cual es el path o ruta desde la que se ejecuta gunicorn para poder configurar más adelante un servicio del sistema. Podemos averigurarlo así:
![alt text](img/70.png)

Iniciamos Nginx y comprobamos que se ha activado
````
sudo systemctl start nginx

sudo systemctl status nginx
````
![alt text](img/71.png)

Ahora, fuera de nuestro entrono virtual, tnenmos que crear un archivo para que systemd corra Gunicorn como un servicio `sudo nano /etc/systemd/system/flask_app.service`
![alt text](img/72.png)

Habiltamos el servicio y se inicia
````
systemctl enable nombre_mi_servicio

systemctl start nombre_mi_servicio
````
![alt text](img/73.png)

Creamos un archivo con el nombre de nuestra aplicación y dentro estableceremos la configuración para ese sitio web. El archivo, como recordáis, debe estar en /etc/nginx/sites-available/nombre_aplicacion y tras ello lo editamos para que quede:
