### DNS (domain name server) sistema que cambia una ip a un nombre que podamos recordar
Recuerda que una ip nos identifica en la red

##¿Puedo usar dos ip para un mismo dominio?

Si, con un DNS tipo A cuando asociamos mas de una ip a un domino el va a empezar a multiplexar, es decir, cuando hacemos una request (puede ser un ping) primera vez nos arroja una ip y la segunda vez nos arroja otra, esto puede ayudar a balancear cargas en el sistema.

#Recuerda

cada 60 segundos (Tiempo de vida) todos los servidores y todos los navegadores van a volver a renovar el domino.

subdominio.dom.ino

##¿Que es un DNS tipo CNAME?

Un CNAME es un alias a cualquier dominio en internet, por ejemplo, asociamos jhon.io como CNAME a fredy.com, lo primero que se va a resolver es a que apunta el dominio de jhon.io despues de ver que jhon.io apunta a fredy.com se va a resolver a que apunta fredy.com. El registro CNAME no permite tener multiples apuntadores.