# Probando REST APIs 

El *Testing* de las REST APIs es una técnica open-source de pruebas automáticas, cuyo propósito es asegurarnos que, mediante varias peticiones HTTP,  nuestra API funcione correctamente.
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

![Imagen de Postman.com](https://assets.postman.com/postman-docs/postman-app-default-v8.jpg)

## Enviando nuestra primera petición con Postman

En Postman puedes realizar llamadas a una API y examinar las respuestas sin necesidad de usar una terminal o escribir código. Cuando creas una petición y haces click en ***Send***, la respuesta aparecerá dentro de la interfaz de usuario de Postman.

![Imagen de Postman.com](https://assets.postman.com/postman-docs/anatomy-of-a-request-v8.jpg)

### Enviando una petición

Para enviar nuestra primera *API request*, debemos primero abrir Postman. Después, hacer click en el botón *más*  **+** para abrir una nueva pestaña.

1. Ingresa **`postman-echo.com/get`** en el campo de URL.
2. Haz click en Send. Verás una respuesta en formato JSON del servidor en el panel de abajo.

![Imagen de Postman.com](https://assets.postman.com/postman-docs/first-request-sent-v8.jpg)


## TODO:
Manejando Peticiones
Autorizando Peticiones


## Referencias
- https://learning.postman.com/docs/getting-started/sending-the-first-request/
- https://learning.postman.com/docs/getting-started/introduction/
- https://www.guru99.com/testing-rest-api-manually.html