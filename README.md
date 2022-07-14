# CybersecurityProject

## Obtener la dirección IP con la utilidad Netdiscover
Obteniendo la dirección IP con el comando netdiscover, se puede observar que la dirección IP de la máquina objetivo es 192.168.16.132.
![Imagen 1](paso%201.png)

## Escaneo de puertos a través de Nmap
Se ejecuta un escaneo de puertos para identificar los puertos y servicios abiertos en la máquina destino. Se usa Nmap para escanear los puertos en la máquina destino, nmap -A -p- 192.168.16.132. La salida de Nmap muestra que ha identificado dos puertos abiertos en el escaneo, el puerto 22 que usa el servicio SSH y el puerto 80 que usa el servicio HTTP.

![Imagen 2](paso%202.png)

## Enumerar la aplicación web en ejecución e identificar la vulnerabilidad
1. Iniciaremos el CTF (captura la bandera) con el puerto 80, abrimos la dirección de la máquina destino del navegador, se observa que la página de inicio en la parte superior derecha hay una funcionalidad de inicio de sesión, por lo que iremos a inicio de sesión.
![Imagen 3](figura%203.png)

2. Dentro de la página de inicio de sesión se observa la funcionalidad de iniciar sesión y registro. Accedemos a “registrarse” para registrarnos como nuevo usuario en la aplicación.
![Imagen 4](figura%204.png)

3. Nos registramos como nuevo usuario en la máquina destino. Registro con cuenta1.
![Imagen 5](figura%205.png)

4. Asimismo, accedemos a Burp Suite para realizar la interceptación.
![Imagen 6](figura%206.png)

5. Una vez iniciada la sesión, se observa en la página qué contamos con una opción de cambio de contraseña, por lo que se procederá a cambiar la contraseña a "abc".
![Imagen 7](figura%207.png)

6. Observamos dentro de Burp Suite que hay una relación de la contraseña y el id de usuario al momento de cambiar la contraseña, por lo que se puede aprovechar esta vulnerabilidad para cambiar la contraseña de otro usuario.
![Imagen 8](figura%208.png)

7. Procedimos a cambiar la contraseña del  id 1, correspondiente al usuario admin.
![Imagen 9](figura%209.png)

8. Nos logueamos con admin para explorar más funciones como usuario administrador.
![Imagen 10](figura%2010.png)

9. Observamos que tenemos la opción de subir archivos en la aplicación destino, por lo que es vulnerable a la carga de archivos maliciosos.
![Imagen 11](figura%2011.png)

## Cargando PHP Shell y tomando la conexión inversa
1. Cargamos el archivo shell2.php, el cual nos permite hacer un shell inverso de la máquina destino.
![Imagen 12](figura%2012.png)

2. Observamos que no se cargó el archivo porque solo permite archivos con extensión .jpg, .png, .gif.
![Imagen 13](figura%2013.png)

3. Cambiamos la extensión del archivo a .phar y está vez sí se subió correctamente.
![Imagen 14](figura%2014.png)

4. Accedemos al archivo cargado en la ruta y se colocó el comando ls en el parámetro cmd para verificar que se ejecuta correctamente.
![Imagen 15](figura%2015.png)

5. Se va a ejecutar un payload en python que va a permitir hacer el reverse conexión.
![Imagen 16](figura%2016.png)

6. Ejecutamos Netcat en la máquina atacante para escuchar las conexiones entrantes a través del puerto 1234.
![Imagen 17](figura%2017.png)

7. Colocamos el código de python en url-encode y así tendremos la conexión en el servicio de netcat que se activó.
![Imagen 18](figura%2018.png)
![Imagen 19](figura%2019.png)

## Escalada de privilegios y lectura del indicador de usuario
1. Se ejecutó el siguiente comando para tener un terminal más interactivo.
![Imagen 20](figura%2020.png)

2. Observamos que no se cuenta con permisos de administrador.
![Imagen 21](figura%2021.png)

3. Se comienza a explorar el directorio que contiene el shell y se observó en /home/john un archivo toto ejecutable con acceso root.
![Imagen 22](figura%2022.png)

4. Cuando se intenta abrir toto aparece el mismo resultado que el comando id.
![Imagen 23](figura%2023.png)

5. Se va a acceder a /tmp para modificar el comportamiento del programa. 
![Imagen 24](figura%2024.png)

6. Luego se creó un archivo “id” usando el comando echo.  También se agregó el script “/bin/bash” en este archivo y le proporcionamos permisos de ejecución usando el comando chmod. Luego le establecimos la misma ruta /tmp y lo comprobamos usando which.
![Imagen 25](figura%2025.png)

7. Se vuelve a ejecutar el programa para ver los cambios, y se observa que acabamos de escalar privilegios de usuario y nos encontramos en el usuario john. Capturamos la bandera del CTF en el archivo user.txt.
![Imagen 26](figura%2026.png)

## Aumento de privilegios y obtención del shell raíz y el indicador
1. Accedimos al archivo password que contiene una contraseña “root123”.También se revisó los permisos de sudo ejecutando el comando “sudo -l”, proporcionamos la contraseña identificada y se mostró que en el directorio del usuario hay un archivo de python que se puede ejecutar con permisos root.
![Imagen 27](figura%2027.png)

2. Editamos el archivo file.py y se colocó el script '/bin/bash/. Luego, ejecutamos el archivo con permisos root y se observó que acabamos de escalar al usuario root. Esto se verificó ejecutando el comando 'id', que confirma que ahora somos root en la máquina de destino.
![Imagen 28](figura%2028.png)
![Imagen 29](figura%2029.png)

3. Finalmente, en el directorio /root abrimos el archivo “root.txt” en el que encontramos la última bandera del CTF.
![Imagen 30](figura%2030.png)
