# Probando REST APIs 

El *Testing* de las REST APIs es una técnica open-source de pruebas, cuyo propósito es asegurarnos que, mediante varias peticiones HTTP,  nuestra API funcione correctamente.
Las pruebas de las REST APIs son hechas con los métodos disponibles por el programador, como GET, POST, PUT, y DELETE.
 
## Cómo probar las REST APIs

Para realizar pruebas requerimos de software para interactuar con estas APIs, y algo de documentación propia para anticipar peticiones y resultados, en otras palabras:

- Herramienta de pruebas / Framework para utilizar la API
- Escribir nuestro propio código de prueba para nuestra API

Los casos de pruebas pueden ser probados con algunas de las siguientes herramientas:

- Cliente REST
- Comandos Curl

## Retos de las pruebas de APIs

Algunos problemas interesantes para testers de REST son:

 1. Asegurarse que las pruebas contengan variedad en los parámetros de las llamadas de las APIs de tal forma que verifique la funcionalidad, y exponga las fallas. Esto incluye explorar las condiciones de rango y asignando parámetros comunes.
 2. Crear combinaciones de parámetros interesantes para las llamadas con dos o más parámetros.
 3. Identificar el contenido bajo el cual están hechas las llamadas a la API. Esto puede incluir configurar condiciones de ambientes externos (dispositivos periféricos, archivos, etcétera) así como datos internos que afecten a la API.
 4. Secuenciar llamadas en el orden que las funciones serán ejecutadas. Por ejemplo, hacer un login antes de acceder a datos protegidos del usuario. 

En esta lectura, veremos como usar Postman para probar nuestras REST APIs.

## Postman

Inicialmente comenzó como un plugin de Google Chrome, con el objetivo de probar servicios de APIs. Hoy en día se describe como una plataforma API para construir y utilizar APIs. Simplifica cada paso del ciclo de vida de APIs y facilita la colaboración para poder crear mejores APIS más rápido.

### El API Client

El API Client de Postman es la herramienta fundacional de este software. Permite explorar, debuggear, y probar nuestras APIs mientras definimos peticiones por HTTP, REST, SOAP, GraphQL, y WebSockets.

![enter image description here](https://assets.postman.com/postman-docs/postman-app-default-v8.jpg)

## Enviando nuestra primera petición con Postman

En Postman puedes realizar llamadas a una API y examinar las respuestas sin necesidad de usar una terminal o escribir código. Cuando creas una petición y haces click en ***Send***, la respuesta aparecerá dentro de la interfaz de usuario de Postman.

![enter image description here](https://assets.postman.com/postman-docs/anatomy-of-a-request-v8.jpg)

### Enviando una petición

Para enviar nuestra primera *API request*, debemos primero abrir Postman. Después, hacer click en el botón *más*  **+** para abrir una nueva pestaña.

1. Ingresa **`postman-echo.com/get`** en el campo de URL.
2. Haz click en Send. Verás una respuesta en formato JSON del servidor en el panel de abajo.

![enter image description here](https://assets.postman.com/postman-docs/first-request-sent-v8.jpg)

## Manejando Peticiones

Puedes enviar peticiones en Postman para conectarte a APIs con las que estés trabajando. Tus peticiones puede traer, añadir, borrar, y actualizar datos. Las peticiones pueden enviar parámetros, detalles de autorización, y también cualquier cuerpo de petición que se requiera,

> Por ejemplo, si estás construyendo una aplicación cliente para una
> tienda, talvez quieras enviar una petición para traer la lista de
> productos disponibles, luego otra petición para crear la nueva orden
> (incluyendo los detalles de los productos seleccionados), y después
> una petición diferente para loggear a un usuario en su cuenta.

### Agregando Detalles a las Peticiones

Si estás construyendo una API, la URL típicamente tendrá de base la locación más la ruta. Por ejemplo, en la petición **`https://api-test-fun.glitch.me/info`**, **`https://api-test-fun.glitch.me`** es la URL base, e **`/info`** es la ruta del ***endpoint***. 

### Seleccionando Métodos de Petición

Por default, Postman selecciona el método **`GET`** para cada nueva request. Los **`GET`** son usualmente usados para retornar datos desde la API. Puedes cambiarlo según tus endpoints, donde las opciones más comunes son:

- **POST** -- Añade nuevos datos
- **PUT** -- Reemplaza datos existentes
- **PATCH** -- Actualiza algunos campos de datos
- **DELETE** -- Borra datos existentes

![enter image description here](https://assets.postman.com/postman-docs/request-methods.jpg)

### Enviando Parámetros

Puedes enviar la ruta y los *query parameters* (parámetros de query) con tus peticiones usando el campo URL y la pestaña **Params**.

- Los ***query parameters*** son anexados al final de la petición URL, seguido de un `?` y listados en pares de llave y valor, separados por `&` usando la siguiente sintaxis: `?id=1&type=new`
- Los ***path parameters*** (parámetros de ruta) forman parte de la URL de la petición, y son referenciados usando placeholders precedidos de `:` como en el siguiente ejemplo: `/customer/:id`

Para enviar un *query parameter*, puedes añadirlo directamente a la URL, o abrir la pestaña **Params** e ingresar su nombre y valor. *Puedes ingresar los query parameters ya sea en la URL, o en los campos de la interfaz, y estos serán actualizados de todos modos.*

![enter image description here](https://assets.postman.com/postman-docs/request-params-v8.jpg)

> Los parámetros no se encodificarán a la URL automáticamente. Haz click
> derecho en el texto, y elige **EncodeURIComponent** para manualmente
> encodear un valor de parámetro.
>  ![enter image description here](https://assets.postman.com/postman-docs/encode-param.jpg)

Para enviar un *path parameter*, ingresa el nombre del parametro en el campo de la URL, después de dos puntos, por ejemplo `:id`. Cuando hayas ingresado el *path parameter*, Postman lo llenará según la pestaña de **Params**, donde también lo puedes editar.

![enter image description here](https://assets.postman.com/postman-docs/path-param-v8.jpg)

## Enviando Cuerpo de Datos

Necesitarás enviar un cuerpo de datos con tus peticiones cuando sea que añades o actualizas datos estructurados. Por ejemplo, si estás enviando una petición que añade un nuevo usuario a tu base de datos, tal vez debas incluir los detalles de este por medio de *JSON*.
Típicamente necesitarás usar un cuerpo de datos con peticiones `PUT`, `POST`, y `PATCH`.

La pestaña Body en Postman nos permite especificar los datos que necesitamos enviar con una petición. Puedes enviar diferentes tipos de cuerpos de datos según lo que acepte tu API.

> Al enviar un cuerpo de datos, hay que asegurarse de tener los headers
> correctos seleccionados para indicar el tipo de contenido que tu API
> vaya a tener que procesar.
> - Para tipos de cuerpo *form-data* y *urlencoded*, Postman automáticamente les agregará la cabecera `Content-Type` correcta.
> - Si quieres usar el modo raw (crudo) de tu cuerpo de datos, Postman le dará un header según el tipo de texto que uses (e. g. texto, o
> json).
> - Si manualmente seleccionas un `Content-Type`, ese valor tomará precedencia a lo que Postman elija.
> - Postman no selecciona ninguna cabecera para tipos de cuerpos *binary*.

Para propósitos prácticos, veremos como enviar peticiones a modo *raw*

### Raw Data

Puedes enviar un cuerpo crudo de datos en forma de texto, usando la pestaña **raw**, y eligiendo de la lista drop-down que formato indicarás a tus datos (Text, JavaScript, JSON, HTML, o XML) y Postman establecerá el resalte de sintaxis así como anexará los headers relevantes a tu petición.

![enter image description here](https://assets.postman.com/postman-docs/body-v8.jpg)

> Puedes seleccionar el texto del editor y presionar CMD/CTRL + B para
> embellecer el XML o JSON.

## Autenticando Peticiones

Algunas APIs requieren detalles de autenticación que puedas enviar en Postman. La **autenticación** involucra verificar la identidad del cliente que envía la petición, y la **autorización** involucra verificar que el cliente tiene el permiso de llevar a cabo la operación del endpoint. Podemos abrir la pestaña de **Authorization** para configurar los detalles de acceso.

![enter image description here](https://assets.postman.com/postman-docs/auth-v8.jpg)

Postman automaticamente incluirá detalles de autenticación en la parte relevante de la petición, por ejemplo, en los **Headers**.

En esta lectura, veremos la autorización por medio de tokens Bearer

### Bearer Token

Los tokens Bearer permiten a las peticiones autenticarse usando una llave de acceso, como un *JSON Web Token (JWT)* . **El token es una cadena de texto**,  incluida en la cabecera de la petición. En la pestaña de **Authorization**, selecciona Bearer Token de la lista drop-down. En el campo **Token** es donde ingresas la llave de tu API, o, para mayor seguridad, la puedes guardar en una variable y referenciarla con el nombre de esa variable.

![enter image description here](https://assets.postman.com/postman-docs/auth-bearer-variable.jpg)

Postman anexará el valor del token después del texto "Bearer" en el formato de autorización como el siguiente:

```shell
Bearer <Your API key>
```

## Usando Variables

Las variables, como en lenguajes de programación, nos permiten guardar y reusar valores. Gracias a esto podemos manejar nuestras peticiones de manera más eficiente, y si necesitamos actualizar un valor, solo tenemos que actualizar la variable.

### Quick Start

Para probar una variable, se siguen los siguientes pasos:

- Click en el **Environment quick look** (un botón en forma de ojo) en la parte superior derecha de Postman, haz click en Edit, y luego selecciona Globals.
- Añade una variable llamada `my_variable` y dale un valor inicial de `Hello` -- haz click en **Save** y cierra el modal de ambiente.
- Abre una nueva petición e ingresa `https://postman-echo.com/get?var={{my_variable}}` en la URL. Pon el mouse encima del nombre de la variable, y podrás ver su valor.
- Envía (**Send**) la petición. Dentro de la respuesta, verás que Postman envió el valor de la variable a la API. Intenta cambiar el valor en **Environment quick look** y envía la petición de nuevo.

Sigue los mismos pasos para variables propias y prueba en tus propias APIs REST.

## Referencias
- https://learning.postman.com/docs/getting-started/sending-the-first-request/
- https://learning.postman.com/docs/getting-started/introduction/
- https://learning.postman.com/docs/sending-requests/requests/
- https://www.guru99.com/testing-rest-api-manually.html
- https://learning.postman.com/docs/sending-requests/requests/#sending-parameters
- https://learning.postman.com/docs/sending-requests/requests/#authenticating-requests
- https://learning.postman.com/docs/sending-requests/variables/