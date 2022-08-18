Expresiones regulares 

barra invertida \ podemos escapar meta-chars, por ejemplo /.

\d, lo que significa cualquier dígito.

La clase \D, por su vez, es un non-digit, o sea, \D es un atajo para [^\d].

. es un meta-char que significa cualquier char.

\s significa whitespace y es un atajo para [ \t\r\n\f].

\w significa word char y es un atajo para [A-Za-z0-9_].

El \W es la non-word char, o sea, todo lo que no es un word char. \W es un atajo para [^\w].

[] significa clase de caracteres.

^ significa busca cualquier carecter o digito pero tiene que estar dentro de las clases de caracteres [], lo que se escriba a su derecha es lo que no quiero que tome para la busqueda.

^ es un ancla que indica que nada viene antes de una string.

$ es un ancla que indica que nada viene despues una string.

word boundary \b significa que no tome nada de la clase \w que son [A-Za-z0-9_].

non-word-boundary: \B  significa que tome cualquiera de la clase \w que son [A-Za-z0-9_].

non capturing group A través de ?: decimos que no queremos ver ese grupo en la respuesta.

() Declara grupo.Un grupo es retornado al momento ejecutar, lo que nos ayuda a seleccionar una parte del match.

| para definir dentro de un grupo que queremos U otro usamos el caracter.

\1 BackReference seleciona el primer grupo encontrado.

QUANTIFIER

los quantifiers son codiciosos por patrón y que podemos utilizar un ? después del quantifier, dejándolo perezoso.

? significa que puede aparecer 0 o 1 vez lo que esta a su izquierda.

' +  significa que puede aparecer 1 o mas vez lo que esta a su izquierda '.

' * significa que puede aparecer 0 o mas vez lo que esta a su izquierda '.

{n} significa se repite n cantidad de veces.

{n,}  significa se repite al menos n cantidad de veces o mas.

{n,m} significa se repite minimo n y maximo m cantidad de veces.

+?  aparece la menos cantidad de veces posibles.


UNA MANERA FÁCIL DE MEJORAR LA LEGIBILIDAD SERÍA USAR ALGUNAS VARIABLES AUXILIARES, QUE DEJAN CLARO LO QUE ESTAMOS DEFINIENDO, POR EJEMPLO EN JAVASCRIPT PODEMOS CREAR 4 VARIABLES:

var DIA  = "[0123]?\d"; 

var _DE_ = "\s+de\s+";

var MES  = "[A-Za-z][a-zç]{1,8}";

var AÑO  = "[12]\d{3}"; 


REPARA QUE CADA VARIABLE REPRESENTA UNA PARTE DE LA REGEX. DESPUÉS DE ESO ES SOLAMENTE CONCATENAR LAS DOS VARIABLES PARA TENER LA EXPRESIÓN FINAL:

var stringRegex = DIA + _DE_ +  MES + _DE_ + AÑO;

PASAMOS ESA STRING PARA LA REGEX ENGINE DE JAVASCRIPT:

var objetoRegex  = new RegExp(stringRegex, 'g');


DESAFIO #1

Tu tarea es crear una regex que verifica el correo de los usuarios y extraer su nombre.

El correo debe de tener un @ y terminar com alura.com o caelum.com. El nombre de usuario (todo antes del @) contiene apenas letras en minúsculas, puede tener un numero al final y tiene de 5 a 15 caracteres.

Por ejemplo:

super.mario@caelum.com extrae super.mario

donkey.kong@alura.com extrae donkey.kong

bowser1@alura.com extrae bowser1

SOLUCION:  ([a-z.]{4,14}[a-z\d])@(?:caelum.com|alura.com)

DESAFIO #2

ahora para validar cualquier correo. Siguen algunos tips:

Puedes utilizar algo de tu regex anterior

Usa los anclas ^ y $

Analiza parte por parte

Primero enfoca en la parte antes del @

Después en el domínio, después del @

Puedes repetir un grupo

Por ejemplo, (([a-z]+)\.)+ significa varios caracteres en minúsculas seguido por un punto, una o más veces.

Sigue algunos correos que debes de encontrar con la regex:

donkey.kong@kart.com.br

bowser1@games.info 

super-mario@nintendo.JP

TEAM.donkey-kong@MARIO.kart1.nintendo.com

Y algunos que no:

toad@kart...com

wario@kart@nintendo.com

yoshi@nintendo

daisy@nintendo.b

..@email.com

SOLUCION:  ^([\w-]\.?)+@([\w-]+\.)+([A-Za-z]{2,4})+$

Vamos a ver parte por parte:

La regex contiene anclas en el inicio ^ y al final $ para garantizar todo el match;

Antes del @ tenemos ^([\w-]\.?)+

Definimos una clase con word chars y guión, seguido por un punto opcional:[\w-]\.?

Después del @ tenemos:

([\w-]+\.)+ que es muy parecido con lo que viene antes del @, pero el . es obligatorio

([A-Za-z]{2,4})+$ que está al final, selecciona el dominio del correo, como mx, com, br. Lo mínimo de letras en esta parte deben de ser 2 y máximo 4.

Hay varios ejemplos complejos disponibles en la internet, pero recuerda que la autenticidad de un correo electrónico solo puede ser verificada enviando un mensaje para el usuario.

DESAFIO #3

Necesitamos ayudar a una empresa de entregas a realizar correctamente la entrega de sus paquetes. Para eso, nos han disponibilizado un archivo con diversas líneas, en las cuales son necesarias las informaciones: Nombre, Calle, Número y Código Postal.

Las demás informaciones también necesitan ser separadas para futuros servicios de la empresa, pero no necesitamos capturarlas en este momento.

Diego Bustamante|14/05/1977|Calle Paseo de La Reforma|50|06600|Ciudad de México
Miguel Herrera|14/06/1993|Calle Temístocles|120|11560|Ciudad de México
Giovanni Chavez|01/01/1995|Calle Lago Mayor|01|11550|Ciudad de México

SOLUCION: ^([\w\s]+)\|(?:\d\d\/\d\d\/\d\d\d\d)\|([\w\s]+)\|(\d{1,4})\|(\d{5}-\d{3})\|(?:[\w\s]{10,})$

El tip para la creación de una regex es siempre ir paso a paso. Por eso, vamos a analizar en siguiente patrón, acuérdate de agregar los grupos e añadir el (|) al final.

Nombre: es necesario entonces creamos un grupo así ([\w\s]+)\|

Fecha de Nacimiento: no es necesario, por eso lo dejaremos como non-capturing group (?:\d\d\/\d\d\/\d\d\d\d)\|

Calle: es necesario, entonces vamos a crear otro grupo ([\w\s]+)\|

Número: es necesario (\d{1,4})\|

Código Postal: también es necesario, podemos crear un grupo de la siguiente manera: (\d{5}-\d{3})\|

Ciudad: no es necesario, entonces agregamos ?: para que su grupo sea non-capturing group (?:[\w\s]{10,})

¡Una regex gigante! Pero como lo hemos visto, si la rompemos en partes menores, es mucho más fácil de analizarla y organizarla.


Existen varias herramientas sofisticadas en la web que puedes utilizar para escribir y analizar tu regex. Esas herramientas son más complejas que nuestro `”probador” y te regalan más informaciones sobre las regex creadas. Siguen algunos links:

https://regexr.com/

https://regex101.com/