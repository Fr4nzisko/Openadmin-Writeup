1) Reconocimiento

sudo nmap -sC -sV -o nmap.openadmin 10.10.10.171

Como se muestra en la imagen

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.001.png)

Entrega un total de dos puestos, 22 de SSH y 80 de HTTP, por lo que procederé hacer una búsqueda de directorios por defecto con dirbuster.

Usaré el diccionario del siguiente directorio.

/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

con 30 threeds

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.002.png)


![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.002.png)

Me dirigo al navegador e ingreso las rutass que me entrega dirbuster, para ello he encontrado un index la siguiente ruta.

http://10.10.10.171/music/

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.003.png)

analizaré todo los apartados de la paguina, incluso revisando el código fuente en busca de alguna pista.

encontrando algo interesante en el “Login”

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.003.png)





me indica la versión “v18.1.1”, por lo que buscaré la vulnerabilidad para esa versión con serachploit.

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.004.png)

indica dos resultados, por lo que utilizaré la versión “remote code execution”

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.005.png)

Se puede ver dos vulnerabilidades en la actual versión,

Guarde el script Bash anterior en Kali y ejecute el comando, puede obtener un shell de rebote:

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.006.png)

se puede ver que se encuentra con la ip de la máquina, lo que toca ahora es escalar privilegios.

Después de la prueba, se descubre que el usuario actual www-data no puede ejecutar el cdcomando para abandonar el directorio actual y find / -type d -user www-dataver el directorio con permiso de acceso.

find: es un comando de Linux para buscar cualquier cosa como un archivo o directorio.

El primer argumento /es el lugar para realizar la búsqueda.

-type - Toma fo se dparece a lo que estamos buscando.

f - Para archivos

d - Para directorios

-user- Esto indica en relación con qué usuario. Este comando buscará todos los filesque tienen permiso para www-databajo /(sistema de archivos completa)

Si hay demasiado contenido, el resultado no se publicará aquí. Básicamente, es /opt/ona/www/con /var/www/ona/dos directorios. Primero, mire el configarchivo de configuración:


![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.007.png)

acá nos entrega una credencial de contraseña en “DB\_PASSWD”

n1nj4W4rri0R!

enumeración de usuarios.

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.008.png)

Encontró la contraseña del usuario de inicio de sesión de mysql :, n1nj4W4rri0R!y luego miró el /var/www/directorio, y descubrió que un internal directorio pertenece al usuario Jimmy, inicie sesión en SSH con la contraseña de Jimmy y mysql.

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.009.png)

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.010.png)

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.011.png)

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.012.png)

Guardamos la clave rsa en un archivo nano

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.013.png)




pasamos la clave rsa a u formato que john reconozca

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.014.png)


le damos permisos de ejecución

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.015.png)

desencriptar clave rsa con john, en kali 2020.1

nota: hay que descomprimir el archivo rockyou.txt.gz

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.016.png)

bloodninjas

clave joanna

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.017.png)

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.018.png)

![](Aspose.Words.fc468124-4b63-48be-ad8f-a66ea29f34c1.019.png)

ctrl+r+x

acá pedirá ejecutar un comando

cat root/root.txt











