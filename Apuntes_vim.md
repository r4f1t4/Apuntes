FORMAS DE ABRIR UN FICHERO
==========================

- `vi nombrefichero`
- `vi -c comando nombrefichero` De este modo se ejecuta un comando, por ejemplo una busqueda vi -c /foo fichero
- `vi + fichero` Con el símbolo + se abre el fichero en la última línea
- `vi +5 fichero` Con el símbolo + y un número se abre el fichero en la línea n
- `vi +/foo fichero` Con el símbolo + Se abre el fichero en la primera aparición
- `vi -R fichero` Se abre en modo lectura.
- `view fichero` Igual que vi -R
- `vi -r` Lista los ficheros con archivos temporales de vi (posible recuperación)
- `vi -r fichero` Recupera el buffer salvado del fichero especificado.

FORMAS DE SALVAR (o no) UN FICHERO
==================================
- **ZZ** Salva un fichero y sale.

Los siguientes comandos son de ex, pero al ser muy básicos los dejo al principio
- `:w` Guarda los cambios del fichero sin salir de el.
- `:w filename` Guarda los cambios realizados en el nuevo fichero "filename"
- `:w %.new` El % almacena el nombre del fichero actual, por lo tanto el comando crearía un fichero nuevo con el mismo nombre que el actual pero terminado en .new.
- `:wa` Guarda todos los ficheros abiertos.
- `:10,20 w filename` Guarda las líneas de la 10 a la 20 en el archivo "filename"
- `:30,39 w >>filename` Añade las líneas al final del archivo "filename".
- `:r filename` Inserta el contenido de "filename" a partir del cursor en el archivo que se está editando actualmente.
- `:q` Sale del fichero en edición sin guardar cambios. Si hay cambios vi no nos dejará salir sin salvar cambios o sin forzar la salida.
- `:q!` Fuerza la salida sin salvar cambios. Esto se usa cuando se han hecho cambios y se quieren descartar
- `:wq` Guarda y sale del archivo. Escribe el fichero aunque no se hayan hecho cambios.
- `:x` Guarda el fichero solo si hay cambios y sale del mismo.
- `:w!` y `:wq!` Guardan el fichero aunque hayamos entrado en solo lectura. También guardaría un fichero sobre el que no tuviesemos permisos de escritura si está en un directorio sobre el que sí que tenemos permisos.
- `:e!` deshace los cambios hasta el anterior guardado.
- `:e` filename Cierra el archivo actualmente abierto y abre el archivo "filename"
- `:e #` Edita el fichero anteriormente abierto.

        > # Equivale al fichero anterior.
        > % Equivale al fichero actual.

- `:ar` Muestra los ficheros actualmente abiertos, equivalente a :args
- `:n` Salta al siguiente archivo abierto (si lo hay).
- `:n file1 file2` Abre los dos nuevos ficheros file1 y file2 en vi.
- `:prev` Vuelve al anterior fichero en edición.

EJECUTAR COMANDOS DE SHELL
==========================

- `:!cmd` Ejecuta el comando "cmd" y muestra el resultado.
- `:r!cmd` El comando r visto anteriormente nos dejaría introducir el contenido de otro fichero en la posición actual. Si en vez de poner el nombre de un fichero, ponemos !cmd ejecutamos el comando cmd y la salida del comando aparecerá escrita en el fichero. por ejemplo:

      :r!date Insertaría la fecha dentro del fichero.
- `:sh` Abre una shell, cuando hacemos ^d volvemos a vi. También podemos hacer ^z y poner vi en bacground


MOVIMIENTO
==========
- h se mueve a la izquierda
- l se mueve a la derecha
- j se mueve hacia abajo
- k se mueve hacia arriba
- w se mueve hacia adelante de palabra en palabra
- W se mueve hacia adelante de espacio a espacio
- b se mueve hacia atrás de palabra en palabra
- B se mueve hacia atrás de espacio a espacio
- e se mueve hasta el último caracter de una palabra
- 0 se mueve al principio de la línea
- $ se mueve al final de la línea
- gg se mueve al principio de la página
- G se mueve al final de la página
- 7G se mueve a la línea 7 (y 7gg también)
- ^F Avanza una pantalla
- ^B Retrocede una pantalla
- ^D Avanza media pantalla
- ^U Retrocede media pantalla
- z[intro] Coloca la línea donde está el cursor arriba del todo
- z. Coloca la línea en la que está el cursor en el medio.
- z. Coloca la línea en la que está el cursor abajo.
- z10 Coloca la línea donde está el cursor como la décima fila.
- 10z Coloca la línea 10 del texto en la primera fila.
- H Mueve el cursor a la primera línea de la pantalla.

      10H Mueve el cursor diez líneas por debajo de la primera visible.
- L Mueve el cursor a la última línea de la pantalla.
      
      10L Mueve el cursor diez líneas por encima de la última visible.
M Mueve el cursor a la línea del medio de la pantalla.
+ Mueve el cursor al primer carácter de la siguiente línea (intro también)
- Mueve el cursor al primer carácter de la anterior línea
^ Mueve el cursor al primer carácter no vacío de la línea
3| mueve el cursor a la tercera columna de una línea.
( Mueve al principio de la frase.
) Mueve al principio de la siguiente frase.
{ Mueve al principio del párrafo
} Mueve al principio del siguiente párrafo
`[[` Mueve al principio de la sección.
`]]` Mueve al principio de la siguiente sección.
`%` Si el cursor se encuentra sobre un bracket de inicio `(, [, { o <` al pulsar % se mueve automáticamente hasta el correspondiente bracket de cierre y viceversa. Si el cursor se encuentra en una línea donde más adelante hay un bracket, se mueve al bracket de inicio y si el único bracket que hay mas adelante en la misma línea es un bracket de final se mueve a el.
mx Crea un marcador y lo almacena en la letra x.
'x Lleva el cursor al primér caracter de la línea donde está el marcador x
`` `x ``Lleva el cursor al marcador x
- \`\`  Vuelve a la posición anterior (anterior edición, busqueda, marcador...)
'' Vuelve al principio de la línea de la posición anterior.

BUSQUEDAS
=========
/ Buscar hacia adelante.
? Buscar hacia atrás.
* Busca la palabra que se encuentra debajo del cursor.
n Repetir la anterior busqueda en la misma dirección.
N Repeir la busqueda anterior en dirección opuesta.
fx Busca el siguiente caracter x en la misma línea hacia adelante.
Fx Busca el siguiente caracter x en la misma línea hacia atrás.
tx Mueve el cursor al caracter anterior al carácter x hacia adelante.
Tx Mueve el cursor al caracter anterior al carácter x hacia atrás.
; Repite el anterior comando de busqueda en linea.
, Repite el anterior comando de busqueda en linea en dirección opuesta.
dfx Borra hasta la próxima aparición del caracter x.
d/palabra Borra hasta la próxima aparición de la palabra "palabra".

AÑADIR Y REEMPLAZAR TEXTO
=========================
i inserta texto desde la izquierda del cursor.
I inserta texto al principio de la línea.
a inserta texto desde la derecha del cursor.
A inserta texto al final de la línea.
o inserta texto una línea por debajo del cursor (añade)
O inserta texto una línea por encima del cursor (añade)
r reemplaza el caracter bajo el cursor.
R reemplaza multiples caracteres empezando por el de debajo del cursor
c cambia el texto, dependiendo del segundo caracter puede cambiar:
	cw hasta el final de la palabra
	c2b hacia atrás, dos palabras
	c$ hasta el final de la línea
	C es equivalente a c$
	c0 hasta el principio de la línea
	c ...
	cc cambia una línea completa
s substituye caracteres, borra el carácter de debajo y entra en insert
S substituye la línea completa y entra en insert.
~ intercambia un caracter entre mayusculas y minusculas.
d borra texto, con los modificadores standard, (D y dd línea, dw palabra, etc)
	de borra desde el cursor hasta el final de la palabra dejando el espacio
x borra el carácter de debajo del cursor.
y copia texto.
yy copia una línea
p pega texto, vi guarda un buffer, por lo que 3p pegaría lo que se hubiese
	copiado hace tres copiados.
	xp transpone dos letras.
P Pega texto antes del cursor.
J une dos líneas (substituye el retorno de carro por un espacio)

REPETIR COMANDOS Y DESHACER CAMBIOS
===================================
u deshace el último comando.
. repite el anterior comando.


TRABAJAR CON BUFFERS
====================
Los buffers permiten tener almacenado texto copiado o cortado anteriormente
y pegarlo en el lugar o momento en el que se prefiera.
Los últimos 9 borrados se almacenan en los buffers del 1 al 9. Además, a la hora
de copiar o de borrar se pueden definir hasta 26 bufferes designados con una
letra. Al escribir en el buffer, si la letra está en minusculas lo sobreescribe,
pero si la letra está en minúsculas, lo añade al final.

"1p Recupera el último borrado del buffer. "2p recuperaría el anterior, etc...
	"1pu.u. En este caso se están usando varios comandos, primero, se
		recupera el último buffer. Luego la u deshace la recuperación
		y el . repite el comando, pero en este caso vi automáticamente
		recupera el buffer anterior. Esto se puede hacer hasta llegar
		l buffer 9 y nos permite ver que hay almacenado en el buffer.
"zyy Copia la línea actual y la almacena en el buffer z.
"a7yy Copia las siete líneas siguientes y las almacena en el buffer a.
"zp Pega el buffer z.
"idw Corta la palabra actual y la almacena en el buffer i. 
"Iy) Añade al buffer I desde el cursor hasta el final de la frase.

COMANDOS SET
============
:set commando activa y :set nocomando desactiva
:set wm=10 Activa el retorno de carro auto en el caracter 10 caracteres antes
	del final de la línea.
:set window=80 Define el ancho de la pantalla como de 80 caracteres.
:set list Muestra los carácteres ocultos, como por ejemplo $ por cada final de
	línea, etc.
:set nu	Activa el mostrado de números de línea 
:set ic Insensitive case, hace caso omiso de las mayúsculas en las busquedas
:set ai Activa el indentado autmático.
	Con el autoindentado activado, dentro del modo de inserción, las
	combinaciones Control-T y Control-D aumentan y disminuyen el nivel de
	indentación automáticamente, aunque ya haya texto en la línea y no estes
	al principio de la misma.
	Del mismo modo, desde el modo comando, los comandos << y >> reducen y.
	aumentan la indentación de una línea.
:set shiftwidth=4 Cambia el número de espacios que se mueve una línea usando
	los comandos de modificación de indentación <<, >>, ^T y ^D. En este
	caso estableciendola en 4 espacios. Esta opción tiene sentido si se
	modifica la forma en la que vi maneja las tabulaciones, para mantener
	cierta consistencia.
:set tabstop=4 Cambia el ancho del tabulado por 4 espacios. Por defecto son 8.
	Esto no cambia la forma en la que se introduce el texto, un tabulador
	sigue insertando el mismo caracter de tabulación en el texto. Solo
	cambia como se muestra en vi.
:set textwidth Define el ancho del texto. Una vez se supere ese texto, al pulsar
	espacio vi automáticamente cambiará el espacio anterior a la palabra por
	un retorno de carro.
:set colorcolumn=+1 Colorcolumn colorea una columna de rojo en toda la altura
	del documento. Si se especifica un número colorea la columna del número
	dado. Si se especifica un número relativo (+1) colorea una columna
	relativa al textwidth definido. Se puede abreviar como :set cc=80.
:set all muestra todas las opciones que vi está usando en este momento.
:set spell spelllang=es_es activa chequeo de ortografía en español.
:set muestra las opciones que se hayan modificado explicitamente durante la
	sesión o se hayan introducido en un fichero .exrc
:set comando? Muestra el valor actual del comando.

OTROS COMANDOS
==============
Vi permite definir abreviaturas que se extenderan automáticamente en el momento
en el que se escriban, por ejemplo:
	:ab rhce RedHat Certified Engineer
A partir de haber incluido esa abreviatura cada vez que se escriba rhce y se
pulse espacio, un signo de puntuación o escape se expandirá. Para deshacer la
abreviatura:
	:unab rhce
Y por último para ver todas las abreviaturas definidas actualmente:
	:ab

Vi permite almacenar secuencias de comandos para volver a ejecutarlas con una
sola pulsación, por ejemplo:
:map q secuencia Maperaría la secuencia de comandos "secuencia" en la letra q
:unmap q Desmapearía la secuencia q.
:map Lista todos los mapeos definidos hasta el momento.
Hay que tener en cuenta que si se mapean teclas que tengan ya un significado en
vi. Las letras y símbolos que están disponibles son: g, K, q, V, v, _, *, \ y =.
Aunque vim ya se reseva algunas de ellas, como v y V.
También se puede mapear sobre teclas de control, como ^A, ^K, ^O, ^W y ^X. Para
introducir las teclas de control puede ser necesario pulsar primero control-v
y luego control-tecladeseada. Esto también sirve para insertar teclas especiales
como el escape o el intro dentro del mapeo.
Del mismo modo se pueden crear mapeos sobre teclas de función.

Otra posibilidad es la creación de funciones. Para crear una función escribirla
en un documento como si fuese texto. Si la función va a incluir alguna tecla
especial como escape para ejecutar comandos, etc hay que insertar los carácteres
de escape correspondientes del mismo modo que al crear mapeos. Es decir, ctrl-v
y a continuación la tecla de escape a mapear. Una vez tenemos el comando escrito
en una línea hay que guardarlo en un buffer con nombre. Para ejecutar el comando
basta con llamarlo con la @. Ejemplo:
	Escribimos en una línea el siguiente texto:
		i<ea>
	Lo guardamos en el buffer i con la siguiente secuencia:
		"idd
	Y a continuación nos colocamos en la primera letra de una palabra y lo
	ejecutamos:
		@i
El comando descrito escribe un caracter < va hasta el final de la palabra y le
añade otro >. Podría ser útil para rellenar tags de html, o xml.

Las funciones con @ también pueden almacenar comandos de ex, para ello escribir
el comando sin los ':' iniciales y guardarlo en un buffer con nombre del mismo
modo. Para ejecutarlo, escribir :@l suponiendo que lo hayamos introducido en el
buffer l.
	
COMANDOS BÁSICOS EN ex
======================

El comando ex es un editor de líneas sobre el que está montado vi. Cuando desde
el modo comando ponemos el caracter : estamos especificando que lo que vamos a
introducir es un comando de ex.
:1 Indica que el comando se va a ejecutar sobre la línea 1. Si no se escribe
   nada mas va a la línea indicada.
:4,7 Indica que el comando se va a ejecutar con las líneas que van desde la 4 a
     la 7.
:% El comando se ejecutará sobre todo el archivo.
:. Línea actual.
:$ El final del archivo.
:% s/foo/bar/ Substituye la primera ocurrencia de foo que encuentre por bar.
:d Delete
:co copy
:t copy
:m move
:j une las líneas especificadas.
EJEMPLOS:
	:7,9d Borra desde la línea 7 hasta la 9.
	:3m8 Mueve la línea 3 y la pone en la línea 8.
	:7,9t15 Copia las líneas de la 7 a la 9 y las pega a partir de la 15.
	:5,.co$ Copia desde la línea 5 hasta el cursor y lo pega al final.
	:.,+3m1 Mueve la línea actual (.) y las tres siguientes (+3) a la
		línea 1. En realidad sería (.+3), pero no es necesario escribir
		el . cuando se usan los modificadores + y -. Cuando + y - van
		después de un ; la posición en vez de ser relativa al cursor,
		es relativa a la primera posición.
	:23,30t-2 Copia las líneas de la 23 a la 30 y las pega dos filas mas
		arriba.
	:---,++m0 Corta desde 3 líneas antes (---) hasta dos líneas después
		(++) y las pega al principìo del fichero. La posición 0 y la
		posición 1 son equivalentes.
	:5,10j Cambia las newlines ente las líneas 5 y la 10 por espacios.
:1,10# Muestra las líneas del 1 al 10 numeradas.
:= Muestra el número total de líneas que tiene el fichero.
:.= Muestra el número actual de la línea.
:/foo/= muestra el número de línea en el que primero aparece el patrón foo.

BUSQUEDAS Y REEMPLAZOS EN ex:
:/foo/d Borra la siguiente línea que contenga foo
:/foo/+d Borra la siguiente línea que contenga foo, pero empieza a buscar a
	partir de la línea siguiente a la que está el cursor.
:/foo/,/bar/d Borra desde la siguiente línea que encuentre con el patrón foo
	hasta la siguiente línea en la que encuentre el patron bar.
:.,/foo/t$ Copia desde la línea actual hasta la primera línea en la que
	encuentre foo y lo pega al final del todo.
:/foo/;+3d Borra desde la primera línea que contenga foo hasta tres líneas
	más adelante. Ojo al ; usado como separador, si hubiese sido una ,
	se habría referido a 3 líneas mas alante del cursor.

El comando g de ex se usa para realizar busquedas globales.
:g/foo/p Muestra todas las líneas que contengan el texto foo.
:g/foo/nu Muestra numeradas todas las líneas que tengan el texto foo.
:g!/foo/p Muestra todas las líneas que NO contengan el texto foo.
:v/foo/p El comando :v es equivalente a :g! y selecciona lo que no contiene el
	patron a buscar.
:7,25g/foo/p Muestra todas las apariciones de foo entre la línea 7 y la 25.
:/bar/;+25g/foo/nu Muestra numeradas las líneas en las que aparece el texto foo
	desde la primera línea en la que aparece bar hasta 25 líneas después.
:1/bar/;+25g/foo/nu Este comando es identico al anterior, pero especifica que
	se empiece a buscar desde la línea 1.

En la línea de comandos de ex se pueden insertar mas de un comando al mismo de
 modo que se ejecuten secuencialmente. Para hacer esto hay que separarlos
mediante el carácter |. Hay que tener en cuenta que el primer comando puede 
afectar al segundo comando si por ejemplo modifica los números de líneas, es
decir, si queremos mover un texto que está entre las líneas 3 y 6 al final del
documento, y reemplazar parte de el, si ejecutamos:
:3,6 m $ | 3,6s/foo/bar/g el comando de modificación no se ejecutaría sobre
las líneas deseadas, en este caso el orden correcto sería el inverso:
:3,6s/foo/bar/g | 3,6 m $

El comando s permite hacer reemplazos de texto (substitute). por ejemplo:
:s/foo/bar/ Reemplaza el primerfoo por bar en la línea actual.
:s/foo/bar/g Reemplaza foo por bar todas las veces que aparezca en la línea.
:% s/foo/bar/gc Reemplaza todas las foo por bar en todo el documento. la c
	indica que antes de cada reemplazo pedirá confirmación.
:1/first/,/end/ s/foo/bar/g Reemplaza todas las apariciones de foo por bar
	que haya entre la primera línea con el texto first y la siguiente
	línea que encuentre con el texto end. El 1 que hay al principio del
	comando modifica el inicio de la busqueda y sirve para que empiece a
	buscar desde el principio.
:g/tree/s/foo/bar/gc En todas las líneas con la cadena tree (:g/tree/)
	substituye foo por bar pidiendo confirmación.
:s repite la anterior substitución
El carácter & equivale al anterior comando de substitución usado, por ejemplo
:%&g repite el último reemplazo pero para todo el documento globalmente.
& El comando de vi & también repite la anterior substitución, es equivalente
	a :&
:~ Repite la anterior substitución, pero en la parte de la búsqueda usa la
	última búsqueda aunque no haya sido una substitución. Ejemplo:
	:1,5s/foo/bar/
	:/tree
	:~ --> esto equivaldría a :/tree/bar/

Hasta ahora en todos los comandos de substitución hemos usado el carácter /
para delimitar los campos de búsqueda y reemplazo. Realmente podemos usar
cualquier carácter que no sea ni una letra, un espacio, una contrabarra o unas
comillas dobles. por ejemplo:
	:s:/home/user1/bin/:/usr/local/bin/:
El anterior comando cambiaría todas las apariciónes del path /home/user1/bin/
por /usr/local/bin/

COMANDOS DE VENTANAS DE VIM
===========================
Si la versión de vi que tenemos es vim (vi improved) existen multidud de 
comandos nuevos que podemos usar, una de las funcionalidades que nos da vim es
la posibilidad de tener ventanas dentro de la propia ventana de vim. para ello
usamos los siguientes comandos:

:split Abre una nueva ventana dividendo la ventana horizontalmente. si se le
	facilita un nombre de archivo abre dicho archivo, si no ambas ventanas
	contienen el archivo abierto actualmente.
:sp Versión abreviada del comando anterior.
:vsplit Identico al anterior, pero en este caso divide las ventanas en vertical
:vsp Versión abreviada del comando anterior.
:new Abre una ventana nueva vacia horizontalmente..
:vnew Abre una ventana nueva vacía verticalmente.

Todos los comandos anteriores admiten la siguiente sintaxis:
	:5split ++opt +cmd 
El comando anterior abre en una nueva ventana horizontal el fichero 'file' con
5 líneas, ejecuta el copmando cmd y activa la opción opt. Los signos de + son
necesarios para qe se interprete correctamente

Otra forma de usar las ventanas es con los comandos precedidos por Control-W
^Ws Abre una nueva ventana horizontal.
^Wn Abre una nueva ventana horizontal vacía.
^Wv Abre una nueva ventana vertical.

Vim también permite abrir en nuevas ventanas archivos referenciados mediante
tags o nombres, por ejemplo:
:stag Divide la pantalla y abre en la nueva ventana el archivo por el punto al
	que hace referencia el tag sobre el que nos encontramos. Si no existe el
	tag, el comando falla y no se abre una ventana.
^Wg] Funciona de forma similar a :stag. Otros comando similar es ^Wg^J
^Wf Abre el fichero que tiene como nombre la palabra que se encuentra bajo el
	cursor si es que lo encuentra en el path.

Para moverse entre las ventanas abiertas sin usar el ratón, nuevamente usamos
comandos precedidos por ^W en concreto:
^W flecha dirección: Mueve el cursor hacia la ventana hacia la que apunta.
^Wh Mueve hacia la ventana de la izquierda.
^Wj Mueve hacia la ventana de abajo
^Wk Mueve hacia la ventana de arriba
^Wl Mueve hacia la ventana de la derecha
^W^W Se mueve entre las ventanas en orden.
^Wp Se mueve hacia la anterior ventana usada.
^Wt Se mueve hacia la ventana de arriba a la izquierda del todo. (top)
^Wb Se mueve hacia la ventana de abajo a la derecha del todo. (bottom)

Otro aspecto mas del movimiento, es cómo reordenar las ventanas. Para ello
usamos los siguientes comandos:
^Wr Reemplaza la ventana con la de la izquierda o la de la derecha. No es
	posible realizarlo cuando la otra ventana se encuentra dividida.
^WR Igual al anterior pero en sentido contrario.
^Wx Intercambia la ventana con la siguiente. Se puede modificar con números, 
	Por ejemplo, 2^Wx Intercambiaría con la siguiente a la siguiente.
^WX Igual al anterior pero en sentido contrario.
^WH Mueve la ventana hasta la izquierda del todo usando toda la altura.
^WJ Mueve la ventana hasta la abajo del todo usando toda la anchura.
^WK Mueve la ventana hasta la arriba del todo usando toda la anchura.
^WL Mueve la ventana hasta la derecha del todo usando toda la altura.

Por defecto si no especificamos un tamaño al dividir ventanas vim divide la
ventana actual por la mitad, dejando dos ventanas del mismo tamaño. Podemos
modificar el tamaño de las ventanas o bien con el ratón pinchando en la línea
divisoria y arrastrandola o bien usando la siguiente serie de comandos:
^W- Reduce la altura de la ventana actual. También se puede modificar con un
	número para indicar la cantidad. 3^W-
^W+ Aumenta la altura de la ventana actual.
:rezize +-n Aumenta o reduce el tamaño de la ventana seleccionada del mismo modo
	que los comandos anteriores.
^W= Ajusta las ventanas intentando dar a cada una la misma altura.
^W< Reduce la anchura de la ventana seleccionada.
^W> Aumenta la anchura de la ventana seleccionada.
^W| Intenta dar a la ventana actual la máxima anchura. Si se modifica con una
	cantidad, da a la ventana actual la cantidad indicada y a la adyacente
	la máxima anchuraIntenta dar a la ventana actual la máxima anchura. Si 
	se modifica con una cantidad, da a la ventana actual la cantidad
	indicada y a las adyacentes la máxima anchura.

Para cerrar ventanas, podemos salir de ellas como si estuviesemos saliendo de
una sesión normal de vi, (:q) (:wq), etc. Esto saldrá de la ventana seleccionada
y dejará las restantes activas. Estos son otros comandos que podemos usar:
^Wq Es el equivalente a :q no nos dejará salir si hay cambios.
^Wc Similar al anterior comando, pero no cierra la última ventana.
^Wo Cierra todas las ventanas menos la actual.
:only Equivalente al comando anterior.


Por últmo, para ejecutar un comando en todas las ventanas sin tener que
repetirlo n veces, podemos precedero del comando windo:
:windo cmd
:windo set ai


USO DE PESTAÑAS EN VIM
======================
Aparte de multiples ventanas dentro de la misma pantalla de vim, podemos tener
múltiples pantallas virtuales abiertas en pestañas (tabs). Esto nos permite
tener varias "sesiones" de trabajo abiertas al mismo tiempo con múltiples
ficheros en edición.

Para abrir pestañas usamos los siguietnes comandos:
:tabnew [filename] Abre una nueva pestaña, si se especifica un nombre de
	fichero lo abre en dicha pestaña.
:tabclose Cierra la pestaña actual
:tabonly Cierra todas las anteriores pestañas. Si hay pestañas con datos sin
	salvar no se cierran.
^page_up Pasa a la siguiente pestaña.
^page_down Pasa a la pestaña anterior.

Existen también varios comandos de ventana (^W) que en vez de crear una ventana
nueva, crean una pestaña, por ejemplo:
^WT Mueve la ventana actual a una nueva pestaña.
^Wgf Busca un fichero con el mismo nombre que la palabra bajo el cursor en el
	path y si lo encuentra lo abre en una nueva pestaña.

BUFFERS
=======
Ahora que manejamos ventanas dentro de vim, lo mas probable es que empecemos a
tener múltiples archivos al mismo tiempo, algunos de los cuales pueden ser
visibles o no. Para ver los archivos que tenemos abiertos ahora mismo podemos
usar cualquiera de los siguientes comandos:
:ls
:files
:buffers

Si a cualquiera de los anteriores comandos les añadimos un ! al final nos darán
información extendida, como por ejemplo:
	u Buffer no listado, como por ejemplo un archivo de ayuda abierto, o un
	  fichero que se ha editado y cerrado anteriormente pero permanece en
	  memoria. No aparece sin !
	% El fichero que está en edición en la ventana activa.
	# El fichero que se estuvo editando anteriormente.
	a Buffer activo. Significa que está visible.
	h Buffer escondido. Está cargado en memoria pero no es visible.
	- Fichero abierto en modo solo lectura. Como con view en vez de vi.
	= Fichero sobre el que no se tienen permisos de escritura.
	+ Fichero con datos por salvar.
	x Fichero en el que ha habido errores de lectura.

Para ejecutar un comando en todas los bufferes cargados sean visibles o no
ejecutamos el comando:
:buffdo cmd
