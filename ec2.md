A continuación voy a describir los pasos que seguí para crear un servidor virtualizado en Amazon Web Services para montar un Wordpress y una apliación en Django.

Para iniciar les voy a dejar algunas [Capturas de pantalla](https://drive.google.com/drive/folders/0B3IiEkfKrDOmc0lCdU10MVZoUEU?usp=sharing) que les pueden ayudar a guiarse en los pasos que voy a ir indicando.

1) Lo primero que debemos tener es una cuenta en AWS (Amazon Web Services) en la cual necesitaremos ingresar una tarjeta de credito o debito internacional para poder acceder a la version de prueba que tiene algunas limitaciones y dura un año.

2) Luego de tener nuestra cuenta nos dirigimos a la consola de AWS y buscamos el servicio de EC2, estando en él seleccionamos lanzar instancia (launch instance).

3) Ahora en el primer paso para crear la instancia vamos a seleccionar que sistema operativo vamos a usar, en nuestro caso es Ubuntu Server 14.04 LTS de 64 bits.

4) En escogemos t2.micro como tipo de instancia.

5) En los pasos de configuración de instancia lo dejamos por defecto.

6) En la sección de almacenamiento vamos a usar 16GB para el ejemplo, aunque pueda que nuestro blog o nuestra apliación crezcan así que de ti depende que tanto espacio vas a dejar (el maximo gratis es 30GB).

7) Podemos dejar los tags de nuestra instancia vacios(como están por defecto)

8) En la sección de grupo de seguridad lo podemos dejar por defecto o le cambiamos el grupo de seguridad, esto tiene que ver con los puertos a los cuales va a escuchar el server, mas adelante vamos a necesitarlos.

9) Al terminar las configuraciones veremos un resumen de nuestra nueva instancia, para terminar le damos en lanzar (launch).

10) Para conectarnos a nuestro servidor de forma remota, vamos a nuestra terminal (en mi caso es la terminal con zsh en ubuntu MATE 16.04) dentro de nuestra terminal nos ubicamos en el directorio en donde almacenamos nuestra llave de seguridad con extensión .pem y ejecutamos el siguiente comando (desde root):
`$ ssh -i "nuestrallave.pem" ubuntu@dnsdenuestroserver`

* nota: recuerda que el archivo .pem debe tener los permisos 400 (lectura para root) con chmod puedes cambiarlo ejecutando
`$ chmod 400 nombredetullave.pem`

11) si todo va bien tendremos un mensaje de Welcome to Ubuntu 14.04.4 LTS ... nos dará un breve resumen de nuestra maquina, del uso de memoria entre otras cosas. En este punto al final de el texto de inicio veremos que nuestra terminal se situa en: 
`ubuntu@ip-privada:~$ `

12) Lo que haremos a continuación será instalar mysql con el comando:
`$ apt-get install mysql server`

13) Luego instalamos apache con la libreria de php:
`apt-get install apache2 libapache2-mod-php5`

14) Ahora nos dirigimos a nuestra consola donde tenemos nuestra instancia, en la tabla que nos describe la instancia que tenemos corriendo buscamos el grupo de trabajo que ella tiene, estando en el grupo de seguridad nos ubicamos en Inbound y agregamos las reglas de HTTP Y HTTPS. Si vamos a un navegador y ponemos nuestra ip publica con el dns de nuestro server vamos a encontrar la pagina por defecto de Apache2 diciendonos que ¡hasta ahora todo funciona!

15) Una forma de verificar que php está funcionando correctamente es dirigiéndonos al siguiente directorio: `$ cd /var/www/html` dentro de el vamos a crear un archivo llamado `index.php` en el que escribiremos usando vim o nano (como prefieran):
`<?php
   phpinfo();
?>`

Luego podremos ir en nuestro navegador a la dirección
`http://nuestraippublica/index.php` y veremos la información del php que está corriendo en nuestro servidor. 

16) Ahora vamos a dirigirnos a la pagina oficial de [Wordpress](https://wordpress.org/download/) y copiamos el enlace de descarga en formato .tar.gz, luego de eso nos situamos en el directorio `cd /var/www/` y ejecutamos el comando: 
`wget https://wordpress.org/latest.tar.gz`

Luego de que termine la descarga vamos a descomprimir y desempaquetar el paquete con el comando:
`$ tar xvfz latest.tar.gz` 

Luego de esto como pueden notar en su carpeta de html van a tener un directorio llamado wordpress.

17) Lo siguiente que vamos a hacer es instalar wordpress usando:
`apt-get install wordpress`

18) Luego de la instalación nos dirigiremos a `cd /usr/share/doc/wordpress/examples/` y buscamos el comprimido para configurar la base de datos de mysql, el archivo se llama setup-mysql.gz, lo descomprimimos con:
`gzip -d setup-mysql.gz`

Luego de tenerlo descomprimido lo ejecutamos de la siguiente forma:

`./setup-mysql -n wordpress localhost`

19) Ahora crearemos un link desde la carpeta `/usr/share/wordpress/` a nuestra carpeta `/var/www/html`, para ello usaremos:
`ln -s /usr/share/wordpress/ .` (estando dentro de nuestra carpeta `/var/www/html`. 

Esto lo hacemos para poder tener acceso a las diferentes vistas de wordpress desde el navegador.

20) Cuando nos conectemos desde nuestro navegador a:
`dns_o_ippublicadelserver/wordpress/` nos puede un error que nos dice que no encuentra un archivo config, lo que podemos hacer para solucionarlo es lo siguiente:

Primero nos ubicamos en la carpeta `/etc/wordpress/`, luego vamos al archivo `/etc/wordpress/config-localhost.php` y lo dejamos de la siguiente manera

`<?php
 #Created by ./setup-mysql 
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'ODwEVg8s');
define('DB_HOST', 'localhost');
define('SECRET_KEY', 'v6YAEt78qMPXZa8HzrpIbpjnkZs1D9VINGdV0hpQNA');
define('WP_CONTENT_DIR', '/var/www/wordpress/wp-content/localhost');
?>`

y lo guardamos, luego creamos un link llamado como el archivo config que nos indica el error es decir mas o menos:
`$ ln -s config-localhost.php config-ippublica.php`

21) Luego de eso en el navegador nos va a direccionar a `dns_o_ippublicadelserver/wordpress/wp-admin/install` en donde vamos a asignar el titulo de nuestro blog y nuestro usuario. y si todo sale bien vamos a poder usar nuestro wordpress desde nuestro servidor.

22) Si aun con lo que hemos seguido no pueden pasar el login pueden leer el readme que incluye wordpress en la siguiente direccion: `dns_o_ippublicadelserver/wordpress/readme.html` en donde nos dice lo siguiente:

> If for some reason this doesn’t work, don’t worry. It doesn’t work on all web hosts. Open up wp-config-sample.php with a text editor like WordPad or similar and fill in your database connection details.
Save the file as wp-config.php and upload it.
Open wp-admin/install.php in your browser.

23) Lo que haremos a continuación es instalar Django, el cual es el framework sobre el que corren apliaciones escritas en Python para ello ejecutamos:

`$ apt-get install django`

24) Ahora crearemos un usuario el cual va a ser quien tenga a su cargo la apliacación de Django (esto nos ayuda a cuidar nuestros datos de posibles ataques, ya que si logran tener acceso solo llegarán a nuestro usuario pero no al root):

`$ adduser app` (podemos dejar los datos como Full name vacios)

25) Logueamos como nuestro nuevo usuario:
`$ sudo login app`

Nuestro prompt se verá algo como:

`app@ip-publica:~$ `

26) Creamos un nuevo proyecto en Django con el comando:

`django-admin startproject myapp`

(myapp es el nombre de la app que van a crear)

27) ahora nos ubicamos en `/myapp/` y ejecutamos la migracion de la base de datos de nuestra aplicación:

`python manage.py syncdb`

y creamos un superusuario para la apliación (asi que llenaremos los datos que nos pide luego de dar yes)

28) para poder ejecutar nuestra apliacion vamos a usar el comando:

python manage.py runserver 0.0.0.0:8000

Para que esto funcione lo que debemos hacer es que en el grupo de seguridad de la instancia vamos a agregar una regla tipo Custom TCP Rule la cual va a escuchar en el puerto 8000 (en total tendrémos 4 reglas la de SSH, HTTP, HTTPS y esta Custom TCP Rule)

Ahora si buscamos en nuestro navegador nuestra-ip-publica:8000 vamos a ver una vista que nos dice It worked!

29) Ahora vamos a instalar gunicorn que es un servidor WSGI HTTP para Python:

$ apt-get install gunicorn_django

30) Ahora vamos a instalar supervisor para poder mantener nuestro servidor de gunicorn arriba en todo momento, primero volvemos a root usando $ logout:

$ apt-get install supervisor

31) Ahora vamos a crear un script para que siempre que inicie la instancia (si es que llega a ocurrir un problema o realizamos un cambio que lo requiera) se levante el servidor con la app de Django, para esto nos vamos a dirigir al directorio /etc/supervisor/conf.d y creamos un nuevo archivo llamado myapp.conf, dentro de el escribiremos:

[program:myapp] directory=/home/app/myapp command = /usr/bin/gunicorn_django -b 0.0.0.0:8000 user = app 

y ejecutamos los siguientes comandos, uno despues del otro:

supervisorctl reread

supervisorctl update

supervisorctl restart all

git commit -m "Hemos terminado"

