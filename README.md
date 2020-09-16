**ESCUELA SUPERIOR POLITÉCNICA**

**DEL LITORAL**

***FACULTAD DE INGENIERÍA ELÉCTRICA Y COMPUTACIÓN***

***SISTEMAS OPERATIVOS***

***NOMBRE: Veas Cervantes Tony NÚMERO DE MATRÍCULA: 201506377***

**REPORTE**

I.  **ANTECEDENTES DEL PROBLEMA**

Basándose en la revisión de la literatura realizada, se estableció una
estructura de antecedentes de investigación de forma teórica. Donde se
tiene como principal fuente de investigación lo descrito en el capítulo
9 (Main Memory) y el capítulo 10 (Virtual Memory, sección 10.2) del
texto guía "Operating System Concepts" tenth edition by Abraham
Silverschatz, Peter Baer Galvin and Greg Gagne.

En el libro conceptual se detalla el proceso que se debe seguir para
realizar la traducción de una dirección virtual generada por la CPU a
una dirección física: a) Se extrae el número de página de la dirección
lógica y se lo emplea como índice en la tabla de página; b) Se extrae el
número de frame de la tabla de página; c) Se reemplaza el número de
página con el número de frame y como el valor del offset no cambia, este
no se reemplaza con lo cual el número de frame y el desplazamiento ahora
comprenden la dirección física.

Por otra parte se describe el inconveniente que se da cuando todo un
programa es cargado en memoria física al momento de su ejecución, ya que
posiblemente al inicio no se requerirá que se tenga todo el programa
cargado en la memoria principal, donde se propone como alternativa
cargar únicamente las páginas al momento que éstas sean requeridas,
técnica conocida formalmente como paginación bajo demando o demand
paging, con lo cual las páginas que nunca sean solicitadas nunca se
cargarán en la memoria principal, pudiéndose utilizar de forma más
eficiente la memoria, ya que las páginas no usadas residirán en memoria
secundaria como por ejemplo un dispositivo HHD o dispositivo no volátil
de memoria. Debido a que durante la ejecución de un proceso algunas
páginas estarán en memoria principal y otras en memoria secundaria se
debe manejar algún tipo de soporte para realizar una distinción, una
sugerencia es manejar un esquema de bits validos-inválidos, cuando el
bit se establece en válido, la página asociada es legal y está en
memoria principal por otro lado si el bit es inválido, la página no es
válida o es válida pero actualmente esta página reside en memoria
secundaria.

Cuando un proceso trata de acceder a una página que no está en memoria
principal (fallo de página), se produce una trampa en el sistema
operativo, que es el resultado de traer una página deseada a memoria. El
procedimiento general a seguir para manejar el fallo de página es: a) Se
verifica en una tabla interna para el proceso si la referencia para el
acceso a memoria es válido o inválido. B) Si la referencia es inválida
se finaliza el proceso. Si es válido y la página no está incluida se
procede a ingresarla. c) Se busca un frame libre en la memoria
principal. d) Se programa una operación de almacenamiento secundario
para leer la página deseada en el marco recién asignado. e) Cuando se
completa la operación de lectura en el almacenamiento, se modifica la
tabla interna guardada con el proceso y la respectiva tabla de páginas
para indicar que la página está en memoria. f) Se reinicia la
instrucción que fue interrumpida por la trampa. Y ahora el proceso puede
acceder a la página como si siempre hubiera estado en memoria principal.

El planteamiento conceptual descrito anteriormente y los conocimientos
adquiridos durante el desarrollo de las clases sobre el análisis de las
traducciones de direcciones virtuales y paginación bajo demanda,
permitió el desarrollo del proyecto, el cual sin lugar a dudas ofrece a
los estudiantes una fundamentación teórica-práctica que facilita el
entendimiento del proceso llevado a cabo para poder realizar la
traducción de las direcciones virtuales generadas por la CPU y las
consideraciones que hay que tener cuando se realiza dicho proceso.

II. **LIMITACIONES**

Aquí se va a detallar cada una de las complicaciones que se presentaron
durante el proceso de desarrollo-entendimiento del proyecto

-   **Limitación 1:**

Esta limitación está relacionada con el hecho de poder encontrar una
máscara para obtener el número de página y otra para el offset. Ya que
esta vez se necesitaba solamente ocupar 8 bits para el número de página
y 8 bits para el offset, empleándose una máscara diferente a la que se
usó para la resolución de la tarea de programación de traducción de
direcciones.

> **Solución:**

-   **Máscara para el número de página**

> Se procedió a escribir un número en binario de la siguiente manera:
>
> 00000000000000001111111100000000
>
> Con esto se pudo obtener una referencia de cómo debía ser nuestra
> máscara a nivel de bits, ya que con esto se consigue saber cuáles bits
> que corresponden al número de página están encendidos.
>
> Luego usando este número binario se lo convirtió en un número base 10
> obteniendo lo siguiente:

(00000000000000001111111100000000)~2~ = (65280)~10~

-   **Máscara para el offset**

> De manera similar que la explicación anterior, se procedió a escribir
> un número en binario de la siguiente manera:
>
> 00000000000000000000000011111111
>
> Con lo cual se tiene una referencia de cómo debía ser nuestra máscara
> a nivel de bits, consiguiendo así saber cuáles bits que corresponden
> al offset están encendidos.
>
> Luego usando este número binario se lo convirtió en un número base 10
> obteniendo lo siguiente:
>
> (00000000000000000000000011111111)~2~ = (255)~10~
>
> Una vez obtenidas las respectivas máscaras ya se podía proceder a
> obtener el valor del número de página y el offset. A continuación se
> detalla el proceso seguido de forma adicional.

-   **Para el page number:**

> Se usó la siguiente máscara
>
> (00000000000000001111111100000000)~2~ = (65280)~10~
>
> Como la cantidad de bits del page number es de 8, entonces usamos
> dicha máscara con los bits de interés en 1 en las posiciones
> correspondientes, para sacar únicamente los bits encendidos que
> corresponden al número de página.
>
> Después se realizó la operación AND (&) a nivel de bits entre la
> dirección virtual en decimal y la máscara. Por ejemplo:
>
> (19986)~10~ = (00000000000000000100111000010010)~2~
>
> AND (&)
>
> (65280)~10~ = (00000000000000001111111100000000)~2~
>
> (19968)~10~ = (00000000000000000100111000000000)~2~
>
> Después se realizó un desplazamiento de bits a la derecha de 8, que es
> la cantidad de bits que posee el offset y lo cuales no los
> necesitamos.
>
> (78)~10~ = (00000000000000000000000000000100)~2~
>
> Obteniendo así el número de página en decimal.

-   **Para el offset:**

> Se usó la siguiente máscara
>
> (255)~10~ = (00000000000000000000000011111111)~2~
>
> Como la cantidad de bits del offset es de 8, entonces usamos dicha
> máscara con los bits de interés en 1 en las posiciones
> correspondientes, para sacar únicamente los bits encendidos que
> corresponden al desplazamiento.
>
> Después se realizó la operación AND (&) a nivel de bits entre la
> dirección virtual en decimal y la máscara. Por ejemplo:
>
> (19986)~2~ = (00000000000000000100111000010010)~2~
>
> AND (&)
>
> (255)~10~ = (00000000000000000000000011111111)~2~
>
> (18)~10~ = (00000000000000000000000000010010)~2~
>
> Para este caso no es necesario hacer ningún desplazamiento, ya que
> directamente obtenemos los bits en la posición correspondiente al
> offset.
>
> Y así se pudo obtener los valores en decimales para el page number y
> el offset.

-   **Limitación 2:**

> En el documento se mencionaba sobre el uso de las funciones fread,
> fwrite y fseek, pero no se comprendía muy bien cómo se empleaban estas
> funciones para manipular archivos ya que antes únicamente había usado
> las funciones de read y write de C las cuales son de bajo nivel y
> específicas de Linux. Se desconocía el funcionamiento de éstas
> funciones y como era la sintaxis que se debía manejar en C, para
> lograr una correcta manipulación de los archivos.
>
> **Solución:**
>
> Se procedió a consultar cierto tipo de fuentes para poder comprender
> como utilizar las funciones fread, fwrite, fseek. Estas funciones
> forman parte de la biblioteca estándar de C.
>
> \#include \<stdio.h\>
>
> Su sintaxis se muestra a continuación:

-   fread \[1\]

> size_t fread( void \*ptr, size_t tam, size_t nmiemb, FILE \*flujo);
>
> Leer nmiemb elementos de datos, cada uno con tam bytes de largo, del
> flujo de datos apuntado por flujo, almacenándolos en el sitio apuntado
> por ptr \[1\].
>
> Devuelve el número de elementos leídos. En caso de error o si se llega
> al final del fichero, el número devuelto es menor que el esperado o
> cero \[1\].

-   fwirte \[1\]

> Escribe nmiemb elementos de datos, cada uno con tam bytes de largo,
> del flujo de datos apuntado por flujo, almacenándolos en el sitio
> apuntado por ptr \[1\].
>
> Devuelve el número de elementos escritos. En caso de error o si se
> llega al final del fichero, el n+umero devuelto es menor que el
> esperado o cero \[1\].
>
> size_t fwrite( const void \*ptr, size_t tam, size_t nmiemb, FILE
> \*flujo);

-   fseek \[2\]

> int fseek( FILE \*flujo, long desplto, int origen);
>
> Mueve el indicador de posición del fichero, correspondiente al flujo
> de datos apuntado por flujo. Su nueva posición, medida en bytes, se
> obtiene añadiendo desplto bytes a la posición especificado por origen
> \[2\].
>
> Origen puede tomar estos valores: SEEK_SET, SEEK_CUR o SEEK_END \[2\].
>
> Así se pudo comprender como era que se debía manejar su sintaxis y
> valores de retornos en cada caso.

-   **Limitación 3**

> Algo que llevó tiempo comprender fue como se manipulaba el archivo
> BACKING_STORE.bin, ya que si se lo trataba de abrir de forma normal,
> dando doble clic en el mismo aparecerían una serie de caracteres que
> no eran entendibles. Ya que muchas veces cuando se quiere leer un
> archivo lo que previamente se hace es abrir el mismo y tratar de
> analizar su contendido y familiarizarse con el mismo, pero en este
> caso fue muy complicado tratarlo de esa forma.
>
> **Solución:**
>
> El archivo se lo manipuló únicamente usando las funciones de la
> librería estándar de C, para manipular archivos, estas funciones
> fueron, fread para leer los 256 bytes correspondientes a una
> determinada página y fseek para realizar un acceso aleatorio en el
> mismo archivo, para poder posicionar en una página cuando esta era
> demandada en un instante. Sin que sea relevante lo que se leyera para
> esta parte, lo único que se hacía era obtener los bytes los cuales más
> adelante se los interpretaba.

-   **Limitación 4:**

> Una vez que se obtenían los 256 bytes del BACKING STORE, estos había
> que almacenarlos en la memoria principal, pero como en C no se puede
> asignar directamente un arreglo en otro, fue una de las cosas más
> complicadas de tratar de resolver debido a que no se conocía de alguna
> función que pueda facilitar el realizar esta asignación.
>
> **Solución:**
>
> Para tratar de resolver este problema lo que se hiso fue crear un
> arreglo auxiliar de 256 bytes, el cual iba a servir para almacenar los
> 256 bytes, cada vez que se leía una página del backing store, una vez
> que se tenía la página, se procedía a usar un for el cual se ejecutaba
> 256 veces, donde además se tenía una variable llamada referencia que
> lleva el control del número del frame de la memoria principal que
> estaba libre y donde se debía asignar dichos 256 bytes.
>
> El bucle for es algo similar a lo que se mostrará a continuación:
>
> char auxiliar\[256\] = {0};
>
> int k = 0;
>
> for (i = 256 \* referencia ; i \< 256 \* (referencia + 1); i++){
>
> memoria_principal\[i\] = auxiliar \[k\];
>
> k++;
>
> }
>
> El valor de "i" inicia de forma dinámica en una determinada posición
> de acuerdo a la variable referencia (que lleva el control del frame
> libre donde se puede almacenar los 256 bytes) y así usando una
> variable k procedíamos a copiar cada uno de los valores en la memoria
> principal.

-   **Limitación 5**

> Una vez que ya se tenía la información correspondiente a la traducción
> de la dirección virtual a física, se debía de crear una cadena con el
> formato:
>
> Virtual Address: \<VPN\> Physical Address: \<number\> Value: \<value\>
>
> Lo cual en C no se brinda muchas facilidades para realizar una
> operación de concatenación.
>
> **Solución:**
>
> Lo cual se recurrió la función snprintf() para almacenar en un buffer
> dicha información y poder ser luego escrita en el archivo de salida.
>
> snprintf(linea_escribir, sizeof(linea_escribir), \"Virtual address: %d
> Physical address: %d Value: %d\\n\",
> valor_direccion_virtual,direccion_fisica,value)

-   **Limitación 6**

> Esta limitación se trata en cómo se debía de hacer para mostrar el
> valor final que estaba almacenado en el frame, en el archivo de
> salida, ya que como no se podía ver muy bien el contenido del archivo
> bin, se presentó una dificultad para saber exactamente como mostrarlo.
>
> **Solución:**
>
> Lo que se trató de hacer es tomar dicho "byte" hacer un printf por
> pantalla, primero mostrándolo como %s, luego cómo %c dándonos
> resultados inconsistentes, hasta que luego se empleó %d y el resultado
> coincidía de acuerdo a lo que se mostraba en el archivo.

III. **COMPILACIÓN, USO Y PARÁMETROS DE ENTRADA**

Para crear el programa ejecutable:

make clean

make

**USO**

-   **Uso**

./pagingdemand \<addresses_file\> \<output_file\>

**ACLARACIÓN**

Cuando se realiza la lectura de cada una de la líneas del archivo que
contiene las direcciones virtuales, se valida que dicha línea sea un
número válido, es decir se verifica que dicho número no sea menor que 0,
mayor a 65535, que no sea un carácter-palabra-combinación alfanumérica o
cualquier cosa que no se considere un número válido. Para este caso el
programa está desarrollado para no realizar la traducción de valores
inconsistentes, es decir se pasa por alto estos valores y se traducen
únicamente valores considerados como válidos.

IV. **SALIDA DE PANTALLA DEL PROGRAMA Y PRUEBAS REALIZADAS** 🖥

![](media/image1.png){width="5.905555555555556in" height="1.36875in"}

-   Validaciones

```{=html}
<!-- -->
```
-   Argumentos ingresados

Se valida que se ingrese la cantidad de argumentos correctos, mostrando
un mensaje informativo y un ejemplo de cómo debería de ejecutar el
programa en caso de que se ingresen argumentos de forma incorrecta, como
se muestra a continuación:

**Test 1**

![](media/image2.png){width="5.905555555555556in"
height="0.7263888888888889in"}

![](media/image3.png){width="5.905555555555556in"
height="0.7763888888888889in"}

-   Lectura de archivos

Se comprueba que los archivos usados en el programa se puedan abrir, si
no se pueden abrir se finaliza el programa y se muestra un mensaje:

**Test 2**

![](media/image4.png){width="5.905555555555556in"
height="0.5861111111111111in"}

**Test 3**

![](media/image5.png){width="5.905555555555556in" height="0.44375in"}

-   Valores que contiene el archivo de direcciones virtuales

Se valida que las líneas que contenga el archivo que contiene las
direcciones virtuales sean válidos, es decir que comprueba si son menor
a cero, mayor a 65535, combinación de caracteres alfanuméricos, etc., si
el archivo llegara a contener este tipo de valores, al momento de
realizar la traducción estos valores no se los considera, y se sigue
leyendo la siguiente línea.

**Test 4**

A continuación se muestra un archivo de prueba que contiene valores
inválidos:

![](media/image6.png){width="5.532022090988627in"
height="2.062787620297463in"}

Procedemos a ejecutar el programa:

![](media/image7.png){width="5.905555555555556in"
height="0.3784722222222222in"}

Y luego procedemos a revisar el archivo de salida data.txt y se puede
ver que estos valores no fueron considerados en la traducción y el
archivo está vacío.

![](media/image8.png){width="3.375in" height="2.839785651793526in"}

**Test 5**

Aquí se realiza otro ejemplo pero en cambio usando valores válidos e
inválidos:

![](media/image9.png){width="4.364583333333333in"
height="3.149008092738408in"}

Ejecutamos el programa

![](media/image10.png){width="5.905555555555556in"
height="0.3645833333333333in"}

Revisamos el archivo de salida, y se puede verificar que sólo se han
traducido los valores que se consideran como válido y los demás valores
no fueron tomados en cuenta.

![](media/image11.png){width="5.448676727909011in"
height="2.1878051181102363in"}

Además procedemos a verificar estos valores obtenidos con el archivo
correct.txt y se puede ver que la traducción es correcta:

![](media/image12.png){width="5.6570395888014in"
height="1.9169346019247595in"}

**Test 6**

Se realizó otra prueba usando los siguientes valores:

![](media/image13.png){width="4.698572834645669in"
height="4.656899606299213in"}

Se procedió a ejecutar el programa:

![](media/image14.png){width="5.905555555555556in"
height="0.37222222222222223in"}

Y luego se revisó el archivo de salida data.txt y se lo comparó con el
archivo correct.txt donde se pudo comprobar que las traducciones son
correctas.

![](media/image15.png){width="5.905555555555556in"
height="1.8194444444444444in"}

**Test 7**

Se probó el programa usando las 1000 direcciones virtuales que contiene
el archivo addresses.txt

![](media/image16.png){width="5.905555555555556in" height="2.85in"}

![](media/image17.png){width="5.905555555555556in"
height="2.8618055555555557in"}

A continuación se adjunta el archivo de salida ya que es muy grande:

**Test 8**

Se empleó un archive de texto el cual contiene 65536 direcciones virtual
que van del 0 a 65535.

![](media/image19.png){width="4.594390857392826in"
height="6.6050885826771655in"}

Se procede a ejecutar el programa

![](media/image20.png){width="5.905555555555556in"
height="0.34444444444444444in"}

Y la salida es la siguiente:

![](media/image21.png){width="5.479931102362205in"
height="6.5946708223972in"}

![](media/image22.png){width="5.490349956255468in"
height="6.657179571303587in"}

A continuación se adjunta el archivo de salida ya que es muy grande:

CONCLUSIONES

Al terminar el desarrollo del presente proyecto, se puedo poner en
práctica los conocimientos adquiridos durante las clases de la materia,
consolidando todo lo aprendido relacionado a la traducción de las
direcciones generadas por la CPU a direcciones físicas y poder obtener
el valor que se encuentra almacenado en un determinado frame, haciendo
la simulación del flujo que se lleva a cabo en sistemas reales.

Se pudo simular como el sistema operativo maneja el proceso de fallas de
páginas, al producirse una trampa, donde de manera general se extrae del
almacenamiento secundario la página que se necesita, se la carga en
memoria principal para luego proceder a actualizar la tabla de página y
así poder reiniciar la instrucción que produjo el fallo de página.

RECOMENDACIONES

En caso de que se tenga un backing store de un mayor tamaño, se puede
implementar un algoritmo de reemplazo de páginas al momento de que en la
memoria principal ya no existan frames disponibles para cargar una nueva
página.

La incorporación de alguna estructura que simule un TLB, se consigue una
mejora en el proceso de traducción ya que así se podría reducir el
tiempo de acceso a la memoria principal.

FUENTES:

\[1\]\"Ubuntu Manpage: fread, fwrite - entrada/salida binaria de flujos
de datos\", Manpages.ubuntu.com. \[Online\]. Available:
http://manpages.ubuntu.com/manpages/bionic/es/man3/fread.3.html.
\[Accessed: 14- Sep- 2020\].

\[2\]\"Ubuntu Manpage: fgetpos, fseek, fsetpos, ftell, rewind -
reposicionarse en un flujo\", *Manpages.ubuntu.com*. \[Online\].
Available:
http://manpages.ubuntu.com/manpages/bionic/es/man3/fseek.3.html.
\[Accessed: 14- Sep- 2020\].

VI. **RECOMENDACIONES**

En caso de que se tenga un backing store de un mayor tamaño, se puede
implementar un algoritmo de reemplazo de páginas al momento de que en la
memoria principal ya no existan frames disponibles para cargar una nueva
página.

La incorporación de alguna estructura que simule un TLB, se consigue una
mejora en el proceso de traducción ya que así se podría reducir el
tiempo de acceso a la memoria principal.

VII. **FUENTES:**

\[1\]\"Ubuntu Manpage: fread, fwrite - entrada/salida binaria de flujos
de datos\", Manpages.ubuntu.com. \[Online\]. Available:
http://manpages.ubuntu.com/manpages/bionic/es/man3/fread.3.html.
\[Accessed: 14- Sep- 2020\].

\[2\]\"Ubuntu Manpage: fgetpos, fseek, fsetpos, ftell, rewind -
reposicionarse en un flujo\", *Manpages.ubuntu.com*. \[Online\].
Available:
http://manpages.ubuntu.com/manpages/bionic/es/man3/fseek.3.html.
\[Accessed: 14- Sep- 2020\].

\[3\] A. Silberschatz, P. Galvin and G. Gagne, Operating system
concepts, 10th ed. Hoboken (NJ): Wiley, 2018.

