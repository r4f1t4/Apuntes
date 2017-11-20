Comandos de Docker
==================
Los comandos de Docker empiezan todos con el comando `docker` y a continuación el _subcomando_ que realiza la tarea concreta.

Comandos generales
------------------
 - `docker info` Muestra información acerca de docker en el sistema.
 - `docker events` Este comando se queda corriendo en el terminal y muestra a partir de ese momento los eventos que se producen en el sistema docker. (start, connect, create, kill die, disconnect, stop...)
 - `docker events --since '1h'` Idéntico al comando anterior, pero muestra los eventos que han sucedido en la última hora. El comando no termina solo, se queda mostrando los siguientes eventos que suceden por consola.
 - `docker events --filter event=attach` filtra los eventos que se muestran. En este caso se filtra por evento y de entre todos los eventos por "attach."
 - `docker login --username user` Comando para loguearte en el dockerhub. Necesario para hacer push o pull de repositorios privados.
 - `docker logout` Te desloguea del dockerhub.


Comandos de imágenes
---------------------
En Docker, una _imagen_ es un paquete de una aplicación con sus correspondientes librerías, mas las librerías del S.O. necesarias para comunicarse con el kernel linux que corre en el sistema operativo host. Las imágenes se pueden ejecutar, al hacerlo se crea un _container_ que inicialmente es una copia de la imagen, aunque luego se puede ir modificando. 
 - `docker search foo` Busca en el docker hub, _El repositorio oficial de imágenes de Docker,_ una imagen con nombre _foo_
 - `docker pull foo` Se descarga la imagen _foo_ desde el repositorio. Si no se especifica la versión por defecto descarga la versión _latest_.
 - `docker pull foo:latest` Este comando resulta equivalente al anterior.
 - `docker pull foo:bar` Se descarga la versión _bar_ de la imagen _foo_
 - `docker pull user/foo:bar` Se descarga la versión _bar_ de la imagen _foo_ del usuario _user_. Cuando subimos nuestras imágenes a DockerHub, ya sean públicas o privadas, el nombre de cada imagen irá precedido de nuestro nombre de usuario. Las imágenes que hay en DockerHub sin nombre de usuario son imágenes oficiales.
 - `docker images` Lista las imágenes que hay en el sistema.
 - `docker run foo` Ejecuta la imagen _foo._
 `docker inspect image` Saca un JSON con todas las variables y el estado de una imagen.
 - `docker rmi image:tag` Borra una imagen que no tenga container asociados En vez de _image:tag_ se puede usar el _ImageID_
 - `docker rmi -f image:tag` Con -f forzamos el borrado de la imagen aunque tenga containers asociados. Esto no borraría los containters, y se podrían seguir usando, pero quedarían huérfanos.
 - `docker rmi -f ID` Si borramos imágenes por ID y hay más de una imagen con el mismo ID necesitamos poner -f; realmente sería la misma imagen con distinto nombre: ej. latest & centos6
 - `docker build -t usuario/nombre:tag path` Crea una nueva imagen con el nombre dado usando el dockerfile que se encuentre en el path "directorio"
 - `docker commit -m Comentario -a maintainerNAme containerNAme newImagename` Crea una nueva imagen a partir del container especificado. El _newImageName_ puede tener la forma _repo/imageName:Version_
 - `docker save --output outputImageFile.tar image:version` Guarda la imagen _"image:version"_ en el archivo tar _"outputImageFile.tar"_.
 - `docker save image:version >  outputImageFile.tar` Igual que el comando anterior guarda la imagen _"image:version"_ en el archivo tar _"outputImageFile.tar"_
 - `docker load --input  inputImageFile.tar` Carga la imagen previamente salvada. El comando output no comprime las imágenes, pero el comando input es capaz de leer imágenes comprimidas como gz o como bz2.
 - `docker load < inputImageFile.tar` Equivalente al comando anterior.
 - `docker history --no-trunc image` Muestra la historia de la imagen. Como fue creada y los comandos que se ejecutaron para crearla. El modificador `--no-trun` nos permite ver los comandos y los id de los containers al completo, sin ellos solo aparecería el principio.
 - `docker tag imageId newimage:version` Este comando nos deja crear nuevos tags para imágenes que existan en el sistema. No borra la imagen anterior. Puede servir para marcar una imagen como _latest_ o para etiquetar una imagen en función del host donde va a ser instalada, etc.
 - `docker push imageName:version` Comando para hacer push de una imagen a un repositorio en el que estemos logueados. Es necesario que el _imagename_ coincida con el nombre del repositorio.

Opciones de run
---------------
Las siguientes opciones modifican el comando `docker run foo`. Muchas de las opciones son también válidas para otros comandos, como para `docker exec contanier` que veremos en el siguiente punto.
 - `docker run -d foo` Ejecuta la imagen _foo_ en el background.
 - `docker run -it foo cmd` Ejecuta la imagen _foo_ en modo interactivo `-i` la conecta al terminal `-t` y ejecuta el comando _cmd_
 - `docker run -itd foo cmd` Ejecuta lo mismo que el comando anterior, pero en vez de conectarla al terminal que estamos usando deja la conexión abierta para que nos podamos conectar manualmente con el comando `docker attach containerNAme` mas tarde.
 - `docker run --name="bar" foo` Ejecuta la imagen _foo_ y le da el nombre _bar_ al container creado. Si no ponemos nombre a los containers, Docker les asigna un nombre aleatorio.
 - `docker run -P foo` Ejecuta la imagen foo y si esta tiene definidos puertos a exponer los publica en la ip del servidor aleatoriamente a partir del definidos por defecto en el servidor.
 - `docker run -p localip_port1:containerip_port1,localport2,containerport2/udp... foo` Ejecuta la imagen foo y publica sobre la ip y puerto especificados del servidor los puertos especificados del container.
 - `docker run --dns=8.8.8.8 foo` Ejecuta la imagen foo y configura el dns con la ip especificada en _/etc/resolv.conf_
 - `docker run -v /local/path:/remote/path` Monta el path local en la ruta remota. Si solo se indica un path, será el path remoto, y el path local se autogenera dentro de la carpeta del container en el sistema.
 - `docker run --net netname --ip ip.address foo` Ejecuta la imagen foo usando la red _netname_. Además, si estamos usando una red creada por nosotros podemos usar la opción `--ip` que nos permite especificar la ip que tendrá el container

Comandos de Containers
----------------------
En docker se genera un container cada vez que se ejecuta una imagen. Una vez un container se ha generado, podemos modificarlo, conectarnos a el, ejecutar procesos sobre el, e incluso salvarlo como si fuese una nueva imagen tras haberlo personalizado a nuestro gusto.
 - `docker ps` Muestra los containers en ejecución
 - `docker ps -a` Muestra todos los containers que existen en el sistema.
 - `docker ps -aq` Muestra sólo los id _-q_ de los containers que existen en el sistema.
 - `docker ps -aqf ancestor=foo` _-f_ filtra los resultados y solo muestra los id de los containers que han sido creados a partir de la imagen _foo._
 - `docker create -it --name="myContainer" foo cmd` Crea un container con nombre _myContainer_  a partir de la imagen _foo._ Pero no lo ejecuta. Podemos arrancarlo con `docker start myContainer`
 - `docker inspect containerName` Muestra por pantalla las variables y el estado de un container en formato json.
 - `docker stop containerName` Para el container con el nombre "containerName", también se puede usar el container id en vez del nombre.
 - `docker attach containerName` Se conecta el proceso que está en ejecución en el container seleccionado.
 - `docker start containerName` Arranca el container especificado.
 - `docker restart  containerName` Reinicia un container en ejecución, o lo inicia estaba parado.
 - `docker exec -it containerName cmd` Ejecuta el comando cmd en el container en ejecución y conecta la consola a el.
 - `docker exec -u 0 -it containerName cmd` El modificador -u nos permite ejecutar un comando usando otro usuario distinto especificando el UID. En este caso _0_, por lo tanto como usuario root.
 - `docker rm containerName` Borra el container "containerName". Debe estar parado.
 - `docker rm -f containerName` Borra el container "containerName" aunque esté corriendo en este momento
 - `docker rm $(docker ps -aq)` Borra todos los containers. Si nos fijamos estamos usando shell expansion para ejecutar el comando `docker ps -aq` que nos devolvería todos los id de los containers del sistema. Podemos refinar mas la búsqueda usando por ejemplo la opción _-f ancestor=foo_
 - `docker port containerName` Devuelve los puertos del container y su relación con los puertos del host, si la hay.
 - `docker port containerName containerPort[udp/tcp]` Muestra en que puerto del host está disponible el puerto del container especificado. Se puede especificar el tipo de puerto. Da error si el puerto no está disponible.
 - `docker logs containerName` Muestra información de los comandos ejecutados en el container. Incluso cuando ya está parado.
 - `docker top containerName` Muestra una lista de los procesos corriendo en el container ordenados por uso de cpu. _Solo muestra un instante, no se queda corriendo_
 - `docker stats container1Name container2Name...` Muestra el estado del uso de cpu, memoria, red y disco para los containers especificados en vivo.
 - `docker stats container1Name --no-stream` Sólo muestra una iteración y sale.
 - `docker rename oldc_ntnrname new_cntnrname` Cambia el nombre de un container especificando el nombre antiguo, o el id y el nuevo nombre. No es necesario que el container esté parado para renombrarlo.

Comandos de red
---------------
Por defecto al instalar docker, se crea una red en la que se alojan los distintos containers, pero podemos crear redes personalizadas que nos permitirán asignar IPs a containers, definir los rangos de IP admisibles, puertas de enlace, etc. También nos permitirían hacer clusters de containers, etc. A continuación unos comandos básicos.
 - `docker network ls` Muestra las redes habilitadas para Docker
 - `docker network inspect net` Muestra información detallada de la red _net_. Podemos llamar a la red por nombre o por ID, un ejemplo puede ser: `docker network inspect bridge`
 - `docker network create --subnet 10.0.0.0/24 --gateway 10.0.0.1 newNet01` Crea una nueva red llamada _mybridge01_ usando la subnet y el gateway indicados. El resto de opciones, como el tipo de red, se crean por defecto, aunque podrían ser especificadas.
 - `docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 --ip-range=10.1.4.0/24 --driver=bridge mybridge01` Añadimos las opciones `--ip-range` que nos permite especificar el rango de IPs que docker asignará a los containes automáticamente y `--driver` que indica el driver que usará la red. Ejemplos de drivers son `bridge` (por defecto) y `overlay` que lo podemos usar para hacer clustering

Dockerfiles
-----------
Para crear nuevas imagenes podemos crear un fichero "dockerfile" este fichero nos permite indicar las instrucciones de creación paso a paso de la imagen para luego generarla con el comando `docker build`. El fichero ha de empezar con  
