
23) Lo que haremos a continuación es instalar Django, el cual es el framework sobre el que corren apliaciones escritas en Python para ello ejecutamos:

`$ apt-get install django`

24) Ahora crearemos un usuario el cual va a ser quien tenga a su cargo la apliacación de Django (esto nos ayuda a cuidar nuestros datos de posibles ataques, ya que si logran tener acceso solo llegarán a nuestro usuario pero no al root):

`$ adduser app` (podemos dejar los datos como Full name vacios)

25) Logueamos como nuestro nuevo usuario:
`$ sudo login app`

Nuestro prompt se verá algo como:

`app@ip-privada:~$ `

26) Creamos un nuevo proyecto en Django con el comando:

`django-admin startproject myapp`

(myapp es el nombre de la app que van a crear)

27) ahora nos ubicamos en `/myapp/` y ejecutamos la migracion de la base de datos de nuestra aplicación:

`python manage.py syncdb`

y creamos un superusuario para la apliación (asi que llenaremos los datos que nos pide luego de dar yes)

28) para poder ejecutar nuestra apliacion vamos a usar el comando:

`python manage.py runserver 0.0.0.0:8000`

Para que esto funcione lo que debemos hacer es que en el grupo de seguridad de la instancia vamos a agregar una regla tipo Custom TCP Rule la cual va a escuchar en el puerto 8000 (en total tendrémos 4 reglas la de SSH, HTTP, HTTPS y esta Custom TCP Rule)

Ahora si buscamos en nuestro navegador `nuestra-ip-publica:8000` vamos a ver una vista que nos dice 
