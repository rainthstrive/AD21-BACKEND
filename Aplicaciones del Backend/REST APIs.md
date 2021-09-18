# ¿Qué es una API?
Una Application Program Interface es la forma en que una arquitectura de software se comunica con otra, a partir de peticiones y respuestas mediante métodos expuestos por la aplicación que provee los datos.
Así pues, podemos hablar de una API como una especificación formal que establece cómo un módulo de un software se comunica o interactúa con otro para cumplir una o muchas funciones.
![enter image description here](https://geekflare.com/wp-content/uploads/2018/11/apis-everywhere.png)

## REST API
Representational State Transfer es una forma sencilla de organizar interacciones entre sistemas independientes.
La forma en que funciona es mediante un cliente (i. e. un navegador de internet) haciendo una petición a un servidor (i. e. Twitter), el cual regresa los datos solicitados, todo mediante protocolos HTTP.

![enter image description here](https://miro.medium.com/max/720/1*0z-VAOlNo2Gm6hOMfFPy1g.jpeg)
### REST
REST no es un protocolo ni un estándar, sino más bien un conjunto de límites de arquitectura. Toda la información la enviamos por medio de HTTP en un formato designado, ya sea JSON, XML, texto sin formato, etc. Siendo JSON el lenguaje más popular, ya que tanto las personas como las computadoras lo pueden entender sin importar el lenguaje.
Para que una API se considere RESTful, debe cumplir los siguientes criterios:

 1. Arquitectura cliente-servidor, gestado a través de HTTP.
 2. Comunicación entre el cliente y el servidor sin estado, lo cual implica que no se almacena la información del cliente entre las solicitudes de GET, y que cada solicitud es independiente y está desconectada del resto. 
 3. Datos que pueden almacenarse en caché y las interacción entre el cliente y el servidor.
 4. Un sistema de capas que organiza jerarquías invisibles para el cliente cada uno de los servidores (los encargados de seguridad, equilibrio de carga, etc.) que participan en las peticiones.
 5. Una interfaz uniforme entre los elementos, para que la información se transfiera de forma estandarizada. Para ello deben cumplirse las siguientes condiciones:
Los recursos solicitados deben ser identificables e independientes de las representaciones enviadas al cliente.
 5.1. El cliente debe poder manipular los recursos a través de la
    representación que recibe, ya que esta contiene suficiente
    información para permitirlo.
 5.2. Los mensajes autodescriptivos que se envíen al cliente deben
    contener la información necesaria para describir cómo debe
    procesarla.
 5.3. Debe contener hipertexto o hipermedios, lo cual significa que cuando
    el cliente acceda a algún recurso, debe poder utilizar hipervínculos
    para buscar las demás acciones que se encuentren disponibles en ese
    momento.

## HTTP
Este es el protocolo que permite enviar documentos de un lado a otro en la web. Solo tiene dos funciones diferentes: servidor, y cliente.
Cada mensaje HTTP se compone de una cabecera, y un cuerpo. En REST, los datos de cabecera suelen ser más importantes que el cuerpo.

## URL
Las URL son para identificar las cosas en las que deseas operar. Decimos que cada URL identifica un recurso. Estas son exactamente las mismas URL que se asignan a las páginas web.

![enter image description here](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL/mdn-url-all.png)

## Verbos HTTP
Cada solicitud especifica un cierto verbo o método HTTP, en el encabezado de la solicitud, e indican al servidor qué hacer con los datos identificados por la URL.​ En REST, los verbos siempre van en el encabezado, jamás en la URL.
Los verbos más usados e importantes son:​
| Verbo | Descripción |
|--|--|
| GET | El método GET solicita una representación de un recurso específico. Las peticiones de este tipo solo deben usarse para recuperar datos. |
| POST | El método POST se utiliza para enviar una entidad a un recurso en específico, causando a menudo un cambio en el estado o efectos secundarios en el servidor. |
| PUT | El método PUT reemplaza todas las representaciones actuales del recurso de destino con la carga útil de la petición. |
| DELETE | El método DELETE borra un recurso en específico. |
| PATCH | El método PATCH es utilizado para aplicar modificaciones parciales a un recurso. |

### Idempotencia
Esto es aplicado a los verbos HTTP, y nos dice que, la ejecución repetida de una petición con los mismos parámetros sobre un mismo recurso tendrá el mismo efecto en el estado de nuestro recurso en el sistema si se ejecuta 1 o N veces.
| Verbo | Idempotente |
|--|--|
| GET | ✅ |
| POST | ❌ |
| PUT | ✅ |
| DELETE | ✅ |
| PATCH | ❌ |

#### ¿Por qué PATCH no es idempotente pero PUT si lo es?
Es porque lo importante está en cómo aplicas tus cambios. Si quieres cambiar la propiedad `name` de un recurso, puede que envíes algo como `{ "name": "foo" }` como el payload (esto es, el cuerpo de la petición) y eso sería idempotente ya que si ejecutas esta petición siempre te traerá el mismo resultado, el cambio de `name`.
Pero PATCH es mucho más general en cómo puedes modificar un recurso. Podríamos hacer una petición para agregar elementos a un arreglo cómo : `{"op": "add", "path": "/-", "value": "foo"}`. La primera vez que lo llamamos, transforma `[]` en `["foo"]`, y luego a `["foo", "foo"]` la segunda vez, y así sucesivamente. Esta operación no es idempotente porque al llamarla una segunda vez resultaría en un error.
Así que mientras que la mayoría de las operaciones PATCH son idempotentes, algunas no lo son.

### Para ser idempotente...
Solamente el estado actual del back-end en el server es considerado, **el código de estatus retornado por cada llamada puede variar**. Por ejemplo, la primera llamada de un DELETE debería retornar un estatus 200, mientras que nuevas llamadas retornarían un 404. Otra implicación de DELETE siendo idempotente es que los desarrolladores no deben implementar APIs REST con una funcionalidad de *"borrar la última entrada"* usando el método DELETE.

## Códigos de Respuesta
Los códigos de respuesta HTTP estandarizan una forma de informar al cliente sobre el resultado de su solicitud. Se encuentran en la cabecera de respuesta de las solicitudes, y se definen por los siguientes números.
| Número | Descripción |
|--|--|
| 2XX | Petición exitosa |
| 3XX | Redirección de recurso |
| 4XX | Error de petición |
| 5XX | Error en el servidor |


## Referencias
- https://www.redhat.com/es/topics/api/what-is-a-rest-api
- https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL
- https://code.tutsplus.com/es/tutorials/a-beginners-guide-to-http-and-rest--net-16340
- https://www.programmableweb.com/apis/directory
- https://developer.mozilla.org/en-US/docs/Glossary/Idempotent
- https://www.adictosaltrabajo.com/2015/06/29/rest-y-el-principio-de-idempotencia/
- https://stackoverflow.com/questions/41390997/why-patch-is-neither-safe-nor-idempotent