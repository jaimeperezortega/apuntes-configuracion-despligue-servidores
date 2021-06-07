# apuntes-configuracion-despligue-servidores

##  AWS

- Servicios. EC2 -> Instancias -> Lanzar Instancias -> Ubuntu Server 20.04 64 bits (x86) --> t2micro --> (marcar protegerse contra la terminación accidental) --> 8GB de almacenamiento (Importante marcar eliminar al terminar) --> Sistema de eqtiquetas --> Ponemos por ejemplo Name: Servidor Web10 --> Configure security Group (es un cortafuegos. AWS te permita crear una regla de cortafuegos que aplicas a todos tus máquinas que quieras --> Utilizar un par de claves ya existente o crear un nuevo par de claves (Yo utilizo web10.pem que ya lo tengo en Desktop)

- Copiar Dirección IP pública (en mi caso: 54.165.217.245)

- Al detener o reiniciar la instancia AWS nos va a asignar una IP distinta a no ser que elijamos el servicio adicional de contratar una IP estática (elásticas según AWS)

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
