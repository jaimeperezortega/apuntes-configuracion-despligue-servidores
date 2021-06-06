# apuntes-configuracion-despligue-servidores

##  AWS

- Servicios. EC2 -> Instancias -> Lanzar Instancias -> Ubuntu Server 20.04 64 bits (x86) --> t2micro --> (marcar protegerse contra la terminación accidental) --> 8GB de almacenamiento (Importante marcar eliminar al terminar) --> Sistema de eqtiquetas --> Ponemos por ejemplo Name: Servidor Web10 --> Configure security Group (es un cortafuegos. AWS te permita crear una regla de cortafuegos que aplicas a todos tus máquinas que quieras --> Utilizar un par de claves ya existente o crear un nuevo par de claves (Yo utilizo web10.pem que ya lo tengo en Desktop)

- Copiar Dirección IP pública (en mi caso: 54.165.217.245)

##  LINUX

 ### Navegación
 
 1. **Entrar al servidor** : ssh ubuntu@"IP" (en mi caso ssh ubuntu@54.165.217.245)
 2. **Entrar al servidor utilizando un par de claves** : ssh --i Desktop/web10.pem ubuntu@"IP" (en mi caso ssh ubuntu@54.165.217.245) --> Antes es necesario cambiar los permisos con el comando chmod 0664 Desktop/web10.pem (la clave 0600 puede ser diferente, hay que usar la que te de el servidor)
 3. **Desconectar del servidor:** --> "logout", "exit" o "CTRL + D"
 4. **Saber quien está conectado al servidor** --> who
 5. **Fecha y hora del servidor** --> date
 6. **Calendario** cal
 7. **Ubicación** pwd
 8. **Archivos que hay en la carpeta donde estoy** ls (en formato lista: ls-l)
 9. **Moverse por el sistema:** cd (change directory) 
 10. **Moverse al directorio superior** cd ..
 11. **Ruta absoluta** Indico la ruta desde el comienzo del sistema de archivo (desde /)
 12. **Ruta relativa** Indico la ruta desde donde estoy hasta el archivo o carpeta que quiero ir
 13. **Limpiar la consola** clear
 
### Gestión de ficheros

1. 
