# apuntes-configuracion-despligue-servidores

##  AWS

- Servicios. EC2 -> Instancias -> Lanzar Instancias -> Ubuntu Server 20.04 64 bits (x86) --> t2micro --> (marcar protegerse contra la terminación accidental) --> 8GB de almacenamiento (Importante marcar eliminar al terminar) --> Sistema de eqtiquetas --> Ponemos por ejemplo Name: Servidor Web10 --> Configure security Group (es un cortafuegos. AWS te permita crear una regla de cortafuegos que aplicas a todos tus máquinas que quieras --> Utilizar un par de claves ya existente o crear un nuevo par de claves (Yo utilizo web10.pem que ya lo tengo en Desktop)

- Copiar Dirección IP pública (en mi caso: 54.165.217.245)

- Al detener o reiniciar la instancia AWS nos va a asignar una IP distinta a no ser que elijamos el servicio adicional de contratar una IP estática (elásticas según AWS)

### Firewall

Por defecto, AWS solo tiene abierto el puerto 22. Tendremos que abrir el puerto 80 (puerto por defecto http) para que nos permita acceder. En la pestaña de seguridad hay que modificar llas reglas de entradas, editando el grupo de seguridad --> Editar reglas de entrada --> agregar regla (HTTP / 0.0.0/0) Para que cualquier dirección IP se ùeda conectaralpuerto 80

### IP FIJA EN AWS

En redes y seguridad --> Direcciones IP elasticas --> Asignar IP elástica ---> Asoiarla a una de mis instancias
IMPORTANTÍSIMO: HAY QUE LIBERAR ESTA IP ELÁSTICA CUANDO ME CARGUE LA INSTANCIA (LA BASE DE DATOS LO HACE AUTOMATICAMENTE PERO NO LA IP ELASTICA Y MME COSTARÍA DINERO)

##  LINUX

 ### Navegación
 
 1. **Entrar al servidor** : ssh ubuntu@"IP" (en mi caso ssh ubuntu@54.165.217.245)
 2. **Entrar al servidor a través de la DNS** ssh ubuntu@"DNS" (en mi caso ec2-54-197-4-146.compute-1.amazonaws.com)
 3. **Entrar al servidor utilizando un par de claves** : ssh --i Desktop/web10.pem ubuntu@"IP" (en mi caso ssh ubuntu@54.165.217.245) --> Antes es necesario cambiar los permisos con el comando chmod 0664 Desktop/web10.pem (la clave 0600 puede ser diferente, hay que usar la que te de el servidor)
 4. **Desconectar del servidor:** --> "logout", "exit" o "CTRL + D"
 5. **Saber quien está conectado al servidor** --> who
 6. **Fecha y hora del servidor** --> date
 7. **Calendario** cal
 8. **Ubicación** pwd
 9. **Archivos que hay en la carpeta donde estoy** ls (en formato lista: ls-l)
 10. **Moverse por el sistema:** cd (change directory) 
 11. **Moverse al directorio superior** cd ..
 12. **Ruta absoluta** Indico la ruta desde el comienzo del sistema de archivo (desde /)
 13. **Ruta relativa** Indico la ruta desde donde estoy hasta el archivo o carpeta que quiero ir
 14. **Limpiar la consola** clear
 
### Carpetas del sistema

#### /etc

Contiene los ficheros de configuración del sistema. 

Para buscar la configuración de algo, casi siempre **vamos a encontrarlo en la carpeta /etc**

#### /home y /root

Carpeta HOME de cada usuario del sistema:

/home/goku
/home/ubuntu
/home/jaime

Cuando nos conectamos por SSh, por defecto accedemos a nuestra carpeta /home

El /root es la casa del superadministrador

#### /sbin

Archivos que solo los puede ejecutar el adminsitrador. Son archivos binarios

#### /tmp

Direcotio temporal donde se pueden almacenar archivos temporalmente

#### /usr

Contiene subdirectorios con ejecutables del sistema

 - /usr/bin: Ejecutables para cualquier usuario
 - /usr/lib: Librerías de programación (C/C++ habitualmente)
 - /usr/local : Archivos locales (desarrollados por tí)
 - /usr/sbin : Ejecutables solo para administradores
 - /ush/share : Datos compartidos (documentación)
 - /usr/src : Código fuente del kernel de linux

#### /var (Carpeta de varios)

 - /var/log: archivos de log
 - /var/mail: buzones de email de los usuarios
 - /var/run: Descriptores de procesos o sockets
 - /var/www: Archivos servidos por el servidor web


### Gestión de archivos y directorios

**Acceso directo a un directorio**
La flechita después de bin indica que en ese directorio hay un acceso directo a usr/bin
lrwxrwxrwx   1 root root     7 Apr 30 23:15 bin -> usr/bin 

1. **Comando which** --> Indica cual es la ruta donde se encuentra ese archivo/programa. Por ejemplo **which ls** te indica que la ruta donde está ese archivo (en este caso programa) es /usr/bin/ls
2. **mkdir** --> Crear carpetas (mk dir demo crea el directorio demo en el directorio desde donde estés ejecutando ese comando)
3. **mkdir -p this/is/spartha** --> Con el modificador -p creamos un directorio dentro de una carpeta. En este caso hemos creado la carpeta spartha dentro de la carpeta is, a su vez, dentro de la carpeta this
4. **rmdir** --> Borrar una carpeta (rmdir demo borra la carpeta demo). SOLO DEJA BORRAR UNA CARPETA SI ESTÁ VACÍA. 
5. **rm** --> Borrar un fichero
6. **rm -r** --> Borrar una carpeta y todo lo que hay dentro de ella (r --> recursivo) MUY UTILIZADO
7. **rm -rf** --> Misma función pero forzado (El sistema no te va a preguntar si estás seguro de borrarlo) **¡¡¡PELIGRO DONDE UTILIZARLO!!!**
8. **touch** --> Sirve para crear un archivo de texto vacío) --> En scripting suele utilizarse para crear un Flag
9. **nano** --> Editar un archivo de texto con el editor nano
10.**cat** --> Visualizar que hay en un archivo determinado
11. **q** --> Salir cuando estás usando el comando less
12. **head** --> Muestra solo las 10 primeras lineas de un archivo (Con el modificador -20,-30 le decimos cuales son las primeras lineas que queremos ver)
13. **tail -f "nombredelarchivo"** Ves el contenido del fichero en tiempo real. Para salir hay que cancelar la ejecución del comando (CTR + C)
14. **wc /direcciondel archivo** --> Cuenta el número de palabras y letras de ese documento
15. **grep** ---> Sirve para buscar cosas dentro de ficheros Ejemplo si quiero buscar donde aparece "kernel" en el archivo: grep kernel /var/log/dmseg. Si por ejemplo quiero buscar el nombre de una funcion de JS en todos los archivos con extension .js (**grep loadAds *js**)
16. **cp** --> copiar/pegar archivos--> Ejemplo cp hello hola, me copia el contenido de hello y lo pone en un archivo que se llame hola
17. **mv** --> me permite renombrar un archivo o mover un archivo o una carpeta (mv hola hello), renombro el archivo hola a hello
18. **find** --> Es como el spotlight de linux. Busca un archivo por un nombre en una carpeta concreta
19. **man<command name>** --> Para consutar la documentación de un comando en concreto
20. 20. **<command>--help** --> Para buscar información adicional sobre un comando en particular

### Usuarios, grupos y permisos

- Los usuarios pueden ser personas y servicios
- Las cuentas de usuario, pueden pertenecer a uno o varios grupos
- Hay unos grupos especiales de administrador (admin o sudo)
- Suele haber una cuenta de administrador (root) pero está deshabilitado en UBUNTU para acceder como root
- Cada archivo o carpeta pertenece a un usuario y a un grupo

#### Tres tipos de acceso en Linux:
  - Lectura
  - Escritura y/o borrado
  - Ejecución (para archivos o scripts ejecutables

#### Tres niveles que definen los tipos de acceso:
  - Propietario del archivo o carpeta
  - Usuarios del mismo grupo del archivo o carpeta
  - Otros usuarios fuera del grupo

#### Ver los permisos de los diferentes ficheros
- **ls -l** --> lrwxrwxrwx user group index.html (1º Si es un archivo pone un guión, si es una carpeta pone d y si es un acceso directo pone l, 2º permisos para el propietario / 3º permisos para otros usuarios del mismo grupo / 4º Permiso para otros usuarios)

#### Modificar permisos de archivos y carpetas

- **chmod "target"+-rwx"filename"** ---> Ejemplo: ( a = all, u=usuario, a = all, g= group) 
 - Ejemplos:
 - 1. chmod u+wrx backup -->Da(+) todos(rwx) los permisos para el propietario (u)
 - 2. chmod a+x backup --> Da(+) permisos de ejecución (x) para todos (a)
 - 3. chmod g+rx backup --> Da(+) permisos de lectura y ejecución (x) para todos los usuarios del grupo (g)
 - 4. chmod o+rw backup --> Quita (-) permisos de lectura y escritura (rw) a los otros usuarios (o)
 
 ### Crear un usuario
 
 - **adduser "username"** --> Crear un usuario y un grupo con el mismo nombre del usuario y mete al usuario en dicho grupo. Esto lo registra en /etc/passwd y /etc/group. Para poder ejecutar este comando tengo que ser administrador, solo el administrador puede crear nuevos usuarios. **sudo "comando"** Al ajecutar algo con sudo delante, ese comando se ejecuta con permisos de administrador
 - **deluser "username"** --> Borra un usuario
 - **adduser "username" sudo** --> Añadimos un usuario con privilegios de administrador
 - **addgroup "groupname"** --> Crear un nuevo grupo
 - **adduser "usuario" "groupname"** --> Para añadir un usuario a un grupo
 - **sudo chown -R web:web /home/web/nodepop** --> Cambiar el dueño de una carpeta o fichero (en este caso el dueño de la carpeta nodepop pasa a ser el usuario web) 
 - - **sudo passwd -l "usuario** --> Bloquear a un usuario para que ya no pueda acceder a nuestro  sistema
 - **Cambiar usuario** --> sudo -u "nombredelusuario" -i
 
 ### Conectarse al servidor con contraseña desde un usuario distinto a Ubuntu
 
 Por defecto aws no permite que un usuario que no sea Ubuntu acceda con una contraseña. Hay que modificar la configuración en etc desde el super usuario Ubuntu
 
 - En la carpeta ssh, hay varios archivos config. Commo nosotros queremos tocar el fichero de configuración del servidor será (sshd_config). La d es de daemon.
 - **nano sshd_config** --> WARNING!!!! OPERACIÓN DELICADA --> Aquí cambiamos la configuración del servidor ssh. Para poder modificar ese archivo hay que agregar sudo al comando para tener permisos de administrador . Donde pone PasswordAuthentication no lo cambio por "yes"
 - Despué
 - **nano sshd_config** --> WARNING!!!! OPERACIÓN DELICADA --> Aquí cambiamos la configuración del servidor ssh. Para poder modificar ese archivo hay que agregar sudo al comando para tener permisos de administrador . Donde pone PasswordAuthentication no lo cambio por "yes"

 ### Recargar el servidor ssh
 - Después de cambiar  la configuración RECARGAR EL SERVICIO (debemos actualizar en caliente su configuración), **OJO NO REINICIAR PORQUE NOS QUEDARÍAMOS SIN ACCESO AL SERVIDOR!!**
 - **sudo systemctl reload ssh** --> Coomando que utilizamos para controlar todos los servicios del sistema. En este caso queremos recargar el servidor - NO REINICIAR!!!- 
 - - Después de  hacer esto ya sí deberíamos ser capaces de entrar al servidor con la cuenta de otro usuario ((ahora ya nos pedirá la password)

 ### Administrar permisos de otros usuarios
 
 **chmod o+w hello** Le doy permiso a  todos los usuarios (o) para que escriban en el archivo hello
 **chmod o-r hello** --> Le quito permiso de lectura a otros usuarios sobre el archivo hello
 
 ### Cambiar el grupo de un archivo
 
 **chgrp "grupo" "archivo"**
 
 ### Cambiar el "propietario" de  un archivo (solo puede hacerlo el usuario administrador)
 
 **sudo chown "nuevo propietario "archivo"
 
 ### Eliminar un archivo
 
 **rm "archivo** --> Es necesario tener permisos de escritura sobre ese archivo y la carpeta que lo contiene
 
 ### Cambiar contraseña
 
 **passwd** Para cambiar mi contraseña desde mi usuario
 **sudo passwd "usuario**" ---< El administrador puede imponer la contraseña que quiera a cualquier usuario
 
 ## Instalación y desistanlación de software en LINUX
 
  - En las distribuciones basadas en Debian (como Ubuntu), se puede instalar y desisinatalr software a través del gestor de paquetes : **apt-get**
  - Este gestor se descarga paquetes ya compilados desde internet para tu distribución y los inatala en el sistema (descargando también otras dependencias si hiciera falta).
  - 
  - **apt-get install "package"** --> Instalar el paquete
  - 
 
 
 ##  Nginx
 
 - **Instalar  ngnix** -- > sudo apt install nginx (Si no funciona bien hacer antes sudo apt update)
 - **Comprobar si se está ejecutando** --> sudo service nginx status / sudo systemctl status nginx
 - **Recargar Nginx** --> sudo service nginx reload
 - **Parar nginx** --> sudo systemctl stop nginx
 - **Arrancar nginx** --> sudo systemctl start nginx
 
 ### Configuración de Nginx
 
 - El fichero de configuración está  en: **/etc/nginx/nginx/conf**
 - En **/etc/nginx/conf.d/** podemos incluir configuración personalizada que sobreescriba parámetros por defecto (si no queremos tocar el /etc/nginx/nginx/conf

 **nginx.conf**
 
    **user www-data** --> Usuario que ejecuta el proceso nginx
    **worker_processes 4** --> Tantos como cores tenga el servidor
    **pid /run/nginx.pid;**
    
    events {
     
      worker_connections 768; --> Máximas conexiones por worker
      # multi_accept on; Aceptar más de una conexión nueva ala vez
    
    }
    

 ### Configuración de nuesttros sitios web
 
 - Con una sola instalación, podemos dar servicio  a varios dominios o subdominios

- En **/etc/nginx/sites-available/** podemos incluir configuración de  otros sitios que queramos configurar

- Una vez configurados, **debemos hacer un acceso directo (ln -s) del archivo a /etc/nginx/sites-enabled/** para que nginx los sirva.
 
 ### Despliegue de sitios web
 
 - Es necesario copiar todo el contenido de la carpeta "dist" y pegarlo en el directorio "html" **cp -rf startbootstrap-stylish-portfolio/dist/* html**
 - Se trata de copiar todo el contenido que hay en la carpeta dist (que está  diseñada para contener tofos los ficheros necesarios para el despliegue) en la carpeta de html que es desde la que sirve el contenido ngnix.
 - Es una buena práctica en los archivos de configuración  **/etc/nginx/sites-enabled**  poner un acceso directo a **sites-available/default**
 - **sudo nano /etc/nginx/sites-available/default t** ---> Para poder modificar este archivo, hay que hacerlo con permisos de administrador. Cambiamos a usuario ubuntu y con el comando sudo En este archivo root apunta por defecto a /var/www/html. Comentamos esa línea y lo sustituimos por root /home/web/html
 - Después de hacer ese cambio en el archivode configuración es necesario recargar nginx. Antes de hacerlo  Nginx tiene un comprobador de configuración muy útil **sudo nginx -t** que debería confirmar que la syntax es ok y la configuración es succesful. Si hay algun error, te  indica la linea en la que está y puedes  volver a editar el fichero para arreglaro
 - **sudo service nginx reload** --> Para recargar nginx
 
 ### Desplegar un aplicación de React
 
 - En la terminal, ubicado en la carpeta donde se encuentre el repo de React lo primero que hay que hacer es un build de React porque antes de poner una aplicación en producción hay que hacer un build primero fuera del servidor para luego subir al servidor solo los archivos de la carpeta build
 - **npm run build** --> Me crea una carpeta build en el repo de React que quiera desplegar en el servidor y esa es la carpeta que debo mover ahi (En una aplicación debbotstrap por ejemplo la carpeta que hemos tenido que desplegar es dist)
 - **PUBLIC_URL=/ npm run build** --> Tenemos que hacer el build de la aplicación de React publicando esta variable de entorno para que luego funcione bien en ningx
 -  **scp "origen" "destino"** Comando para copiar por ssh. Ejemplo (scp -r -i ~/Desktop/web10.pem build ubuntu@54.197.4.146:~/)Esto indica el HOME del usuario (~/)
 -  Desde la carpeta donde se encuentra la carpeta build --> **scp -r -i ~/Desktop/web10.pem build ubuntu@50.17.106.193:~/**

- Es una buena práctica tener tantos usuarios como webs o aplicaciones tengas desplegadas


### Crear un sitio nuevo de NGINX desde cero

- En **etc/sites-available** --> Los fichheros de configuración siempre  los creamos en esta carpeta. Una vez  creado el fiichero en esta carpeta, para que ese sitio comience a estar activo, el sitio tiene que estar en la carpeta de sites-enabled. Se podríacopiar el archivo de sites available a sites enabled. El problema de eso es que si cambio la configuración se pueden desincronizar los archivos. Por eso la buena práctica es crear un acceso directo en sites-enabled y así están siempre sincronizados.
- Creamos en sites-avaibalbe un fichero con **sudo nano nodepopconfig**
server { 

        # Puerto de escucha
        listen 80;

        # En qué carpeta tiene que buscar los archivos de la web
        root /home/web/nodepop

        # Indicar qué  archivos actuan como indice                                   
        index index.html;

        #Cuando la ruta empiece  por /, intenta buscar el archivo que te piden, y si no, 404
        location / { 
                try_files $uri $uri/ /=404;

                # Busca la uri que te pidan, si no encuentras la  uri busca esa carpeta y si no la encuentra tamppoco, de>
        }

}

  - Para poder aplicar este archivo de configutación, hay que crear un acceso directo a este archivo en sites-enabled
  - **sudo ln -s "meter la ruta completa de origen(/etc/nginx/sites-available/nodepopconfig" "meter la ruuta completa de destino(/etc/nginx/sites-enabled/nodepopconfig"** --> Para crear un acceso directo en sites-enabled de  nuestro archivo de configuracion creado en sites available
  - **Comprobar que el archivo es correcto con sudo nginx -t**
  - **Recargamos el servidor** sudo service nginx reload
  - **Cambiar laconfiguración de default server en el archivo default y ponerla en el nuevo que hemos creado**


### Configuración de nginx para que funcionen las apps de React con React Router 

server {

        # Configuro este archivo para que responda a la DNS de amazon:
        server_name ec2-54-197-4-146.compute-1.amazonaws.com;

        # Puerto de escucha
        listen 80;

        # En qué carpeta tiene que buscar los archivos de la web
        root /home/web/nodepop;

        # Indicar qué  archivos actuan como indice
        index index.html;

        #Cuando la ruta empiece  por /, intenta buscar el archivo que te piden, y si no, 404
        location / {
                ##try_files $uri $uri/ /=404;


        # Busca la uri que te pidan, si no encuentras la  uri busca esa carpeta y si no la encuentra tamppoco, devuelve un 404

        # Configuración para app React con React Router
        try_files $uri $uri/ /index.html;
        }

}

### IMPORTANTE DEJAR SIEMPRE DOCUMENTADO COMO SE DESPLIEGA UNA APLICACIÓN . SI ES MUY COMPLICADO ES MEJOR INCLUSO CREAR UN SCRIPT QUE LO HAGA TODO

## INSTALAR MONGO

1. Descargar el paquete de  instalación para Ubuntu (https://www.mongodb.com/try/download/community) ---> Elegir **Ubuntu 20.04** y **Server**
2. **wget https://repo.mongodb.org/apt/ubuntu/dists/focal/mongodb-org/4.4/multiverse/binary-amd64/mongodb-org-server_4.4.6_amd64.deb** 
3. **sudo dpkg -i mongodb-org-server_4.4.6_amd64.deb** --> Para instalar el paquete que me acabo de descargar. Esta opeeración ha creado un usuario mongo en un grupo mongo
4. **sudo systemctl status mongod** --> Para comprobar que mongo está instalado correctamente
5. **sudosystemctl start mongod** ---> Para arrancar mongod
6. **sudo systemctl status mongod** --> Para comprobar que mongo está arrancado (debe aparecer active ruunning)
7. **sudo systemctl enable mongod** --> Para asegurarse de que mongod arranca solo cuando se reinicia el sistema 
                
### INSTALAR CLIENTE DE MONGO
https://www.mongodb.com/try/download/community

1. Descargar el paquete ---> **Ubuntu 20.04 y shell(deb)
2. **wget https://repo.mongodb.org/apt/ubuntu/dists/focal/mongodb-org/4.4/multiverse/binary-amd64/mongodb-org-shell_4.4.6_amd64.deb**
3. **sudo dpkg -i mongodb-org-shell_4.4.6_amd64.deb** --> Instalamos el cliente de mongo
4. **mongo --version** --> Confirma que el cliente mongo está instalado con la versión que sea
5. **mongo** ---> Con este comando utilizamos el cliente para conectar ala base de datod de mongo. Desde aquí podemos ponernos a hacer queries
6. **exit** --> Para sallir del cliente de mongo

## DESPLEGAR APLICACIONES DE NODE

Es necesario que cuando desplegamos una aplicación de node en un servidor, utilizar una herramienta que es un gestor de procesos que está vigilando que todos los procesos que yo le indique estén arrancados. En el momento que detecta que un proceso está caido lo vuelve a arrancar de inmediato.



### Pasos para desplegar una aplicación en Node

 1. **Crear un usuario para la aplicación** --> sudo adduser "nombredeusuarioquevaacontrolaresaaplicacion"
 2. **Bloqueamos la cuenta de ese usuario** --> passwd -l "nombredeusuarioquevaacontrolaresaaplicacion"
 3. **Convertirnos en ese usuario "dueño" de esa aplicación** ---> sudo -u "nombredeusuarioquevaacontrolaresaaplicacion" -i
 4. **Instalar nvm para ese usuario para instalar la versión de node que yo quiera** ---> curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
5. **Salir del usuario dueño de la aplicación**
6. **Volver a aconvertirse en el usuario dueño de laaplicación**
7. **Inicar nvm** --> nvm
8. **Instalar laúltima vversión de node LTS (o la que nos convenga según las necesidades de la aplicación** ---> nvm install --lts
9. **Comprobamos que tenemos instalado node y qué versión** ---> node --version
10. **Clonar el repositorio de la aplicación que queramos instalar** ---> git clone https://github.com/jaimeperezortega/recuperacion-practica-backend-avanzado2.git
11. **Instalar las dependencias del repo** ---> npm install
12. **Ejecutar script para arrancarla aplicación ppara comprobar que funciona** --> npm start  (**SI NECESITA CONECTARSE A UNA BASE DE DATOS, HABRÁ QUE INSTALAR ANTES MONGO**)
13. **Habilitar el cortafuegos para que abra el puerto en el que está corriendo la aplicación (en mi caso puerto 3001)** --> Security --> Security Groups --> Editar reglas de entrada  --> Añadir regla --> Puerto TC personalizado --> Puerto 3001
14. **Comprobar que la aplicación está desplegada** --> URL:3001

### INSTALAR SUPERVISOR PARA NO NECESITAR ESTAR HACIENDO NPM START PARA EJCUTAR LA APLICACIÓN

1. **Instalar supervisor**--> sudo apt install supervisor
2. **Comprobar status de supervisor** --> sudo service supervisor status
3. **Crear un fichero de configuración** --> En la ruta /etc/supervisor/conf.d/ crear un fichero de configuración con el nombre que quieras pero con extensión .conf (sudo nano nodepop.conf)
   [program:nodepop]
   command=/home/nodepop/.nvm/versions/node/v14.17.1/bin/node ./bin/www (**fichero que ejecuta el arranque de la aplicación-- se puede buscar con el comando "which node"** Hay que iral package.json de esa aplicación, encontrar cuál esel comando arranca esa aplicacion en "start" y localizar la ruta absoluta de ese ejcutable con which y añadir el script que arranca la aplicación. En mi caso : /home/nodepop/.nvm/versions/node/v14.17.1/bin/node ./bin/www)
   user=nodepop (**usuario dueño de esa aplicación**)
   directory=/home/nodepop/recuperacion-practica-backend-avanzado2 (**directorio donde está instalada la aplicación**)
   autostart=true
   autorestart=true
 3. **Recargar supervisor** --> sudo service supervisor force-reload // sudo systemctl reload supervisor

#### Comando top (El  Ctrl + Alt + Supr de Linux)
Muestra en tiempo real información delos procesos ordenados por conaaumo de CPU (de mayor a menor)

####  Comando ps aux

Para ver todos los procesos en ejecución con mucha información

**Buscar un proceso en concreto** --> ps aux | grep "comando"

####  Comando para matar un proceso sudo kill "pid"

Para que mi aplicación esté funcionando de manera autónoma sin tener que estar yo conectandome y haceer el npm start vamos a utilizar **SUPERVISOR** que es un gestor de procesos que se encarga de monitorizar que la aplicación esté corriendo, y si no es así, activarla.
- **sudo apt install supervisor** ---> Se encarga de instalar supervisor en el sistema. El problema de instalar algo a ttravés de apt es que se va a instalar la versión de ese software que esté en el "app store" de ubuntu
- **Crear un fichero de configuración en /etc/supervisor/conf.d/** --> Lo ideal es crear un archivo por programa 

### NGINX COMO PROXY INVERSO

Permire a máquinas desde fuera acceder a recursos de nuesto servidor. En funcion del server_name que le pongamos a cada aplicación, nginx redigirá  las peticiones con ese nombre al puerto correspondiente. De manera que los usuarios en función del dominio que entren no tienen que saber el puerto al que tienen que acceder sino que nginx se encarga de enrutarlo en funcion del nombre del dominio.

#### Fichero de configuración de sitios 

  server {
  
    location ~ ^/(css/|img/|js/|sounds/) {
    root /var/www/app/public;
    access_log off;
    expires max;
   }
   
   location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:3000;
    proxy_redirect off;
   }
  } 
  

## PAQUETES "OBLIGATORIOS" PARA INSTALAR SIEMPRE  QUE SE VAYA A INSTALAR NODE  EN UNA MÁQUINA PARA EVITAR CONFLICTOS DE DEPENDENCIAS

**sudo apt install build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev python3** ---> Instala ciertas librerías que son requeridas muy frecuentemente por otras librerías útiles 

