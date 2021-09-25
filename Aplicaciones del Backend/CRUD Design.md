# Diseño CRUD de APIs

El buen diseño de una API es esencia para una excelente experiencia de desarrollador. Una forma sencilla de acercarse a un buen diseño de API es siguiendo un cierto paradigma de diseño y las mejores prácticas que vienen con el. CRUD es uno de esos diseños comunes que funcionan muy bien para muchas APIs.

En esta lectura veremos lo que significa CRUD. Luego, hablaremos de sus puntos más importantes -- crear URLs adecuados y elegir los verbos correctos. Finalmente, tomaremos un vistazo a una pequeña API de ejemplo para mostrar como se verían todas las recomendaciones juntas.

## Hablemos de CRUD

CRUD es un acrónimo que viene del mundo de las bases de datos. Cada letra se refiere a un tipo de acción que un usuario puede realizar en un set de datos: *Create*, *Read*, *Update*, y *Delete*. En bases de datos relacionales, estas cuatro actividades corresponden a los comandos en SQL de *INSERT*, *SELECT,* *UPDATE*, y *DELETE*.

Estos conceptos son aplicables al protocolo HTTP. Cada URL apunta a un recurso, o sea, un set de datos. Y podemos accederlos con los verbos *GET*, *PUT*, *POST*, y *DELETE*. Spoiler: Estos verbos no corresponden exactamente al diseño CRUD.

> Nota: Se piensa que un diseño CRUD es necesario para hacer que una API
> sea RESTful. Este es un malentendido común. Estos términos describen
> diferentes aspectos del diseño de una API. Aún así, CRUD y REST
> funcionan bien en conjunto.

## Diseño de URLs y Buenas Prácticas

Las URLs describen recursos, mientras que los verbos HTTP describen las acciones que se realizarán sobre ellos. Esto nos lleva a la primera regla de diseño de CRUD URL: las URLs solo deben contener pronombres, nunca verbos, a menos que estés deliberadamente añadiendo algo a tu API que no se ajusta al paradigma CRUD.

Los pronombres en URLs son los mismos que describen tu modelo de dominio. Cuando modelas tu dominio, pon atención a lo siguiente:

- No hagas los pronombres innecesariamente largos y complicados (usa `users`, no `system-users`)
- Usa terminología común (`people`, en lugar de `neoloneses`)

Aparte de las reglas de pronombres, existen otras buenas prácticas que la industria ha establecido y que incluso compañías han puesto en sus guías de estilo de API:

- Usa minúsculas (`/items`, no `/Items`) para evadir ambigüedad en la sensibilidad de caracteres en la URL.
- Para recursos individuales, incluye identificadores de recurso en la ruta, no en el query (`/contacts/22` en lugar de `/contacts?id=22`).
- Rutas que terminen con el nombre de un recurso (y típicamente sin diagonal al final) son usadas para listar multiples items (`/files`) o para crear items sin especificar un identificador.
- Las rutas pueden indicar una jerarquía de subrecursos (`/contacts/22/addresses`), pero los diseñadores de APIs deben evitar estructuras complejas que requieran más de dos niveles de anidación.
- Pronombres que sean compuestos de multiples palabras deben usar un guión como separador (`/legal-documents`). Hay una razón muy mundana para esto: cuando las URLs son hipervínculos, estas se muestran subrayadas, lo cual haría de pronombres con separadores guiones bajos invisibles.
- Las URLs ***nunca*** deben temer implementaciones bajo ellas mismas (`/resources`, no `/api.php/resources`).
- La parte del query en la URL es para buscar y filtrar, y comunmente es usada con un endpoint de lista de recursos (`/contacts?first_name=Anna&limit=20`).

Ahora veamos temas un poco más controversiales:

## Versionamiento

Muchas APIs añaden prefijos a sus rutas (`/v1.0/users/{ID}`). Mientras que esto es una manera indudablemente sencilla para que los clientes sepan la versión específica de la API, el diseño de URLs puristas podrían argumentar que esto no es parte de un recurso, y por lo tanto no pertenece en la URL. Prefieren configurar el versionamiento como parte de la cuenta, enviando la información como una cabecera HTTP o construyendo la API con una manera de retrocompatibilidad, y así no requerir versionamiento jamás. Como el versionamiento es todo un tema en sí que depende del ciclo de vida de tu proyecto, no se recomienda una forma sobre la otra.

## Tipos de Recursos

Ocasionalmente, los desarrolladores de APIs añaden un sufijo a sus rutas para indicar el formato de serialización de un recurso (`/contacts/{ID}.json`). Las APIs más nuevas que no requieren de otras serializaciones, como XML para razones de legacy, van directamente al vagón del JSON. Por lo tanto, si no esperas mantener un soporte de otra tecnología que no sea JSON, el `.json` se vuelve redundante en la URL. E incluso si lo haces, existe una alternativa: la cabecera `Accept` en HTTP.
Pero, si ofreces formatos como CSV en tu caso de uso, los desarrolladores apreciarán ser capaces de reemplazar `.json` por `.csv`, en lugar de moverle a las cabeceras HTTP.

## Acciones Non-CRUD

Las cuatro palabras del CRUD no necesariamente cubren todo lo que podemos hacer a nuestros recursos. Por ejemplo, puede que quieras **suspender** o **bloquear** a un usuario, lo cual **es diferente a eliminarlo**.

Es posible mapear estas operaciones de update, por ejemplo, actualizar un usuario con un atributo de estatus de *suspended*. En esos casos, tiene sentido el poner la acción en el URL como `/users/{ID}/actions/suspend`. Es una buena idea el denotarlas claramente, por ejemplo, dándoles el prefijo `/actions`, aunque el uso del verbo ya esté rompiendo el concepto puro de CRUD para una mejor experiencia de desarrollo.

## Los Verbos HTTP

Usar el verbo correcto es uno de los puntos más importantes de buen diseño de APIs, y las bases de estos son sencillos 😎

| Verbo | Descripción |
|--|--|
| GET | Es para leer y retornar datos, es la R de CRUD. Este verbo es idempotente (como visto en la lectura de APIs REST), y también es *seguro* porque no altera nada en el servidor. Al diseñar una API, **nunca de los jamases** se usa GET para algo que modifica un recurso en el servidor.  |
| DELETE | Es la D en CRUD, y elimina uno o más recursos del servidor. |
| PUT | Afecta al recurso identificado por la URL. Por ejemplo, un PUT en `/users/1` puede crear un `user` con el ID 1, o modifica el existente `user` con ese ID. Ya que la mayoría de las APIs generan identificadores en el servidor y no permiten crear recursos de esta manera, PUT se encuentra confinado a operaciones de actualización. Pero, otra vez, eso no es inherente del verbo. También, PUT es idempotente. |
| POST | Envía datos al servidor y deja a cargo a este para hacer algo con esos datos. Es el verbo más flexible. En el contexto de CRUD, es típicamente usado para crear recursos donde la URL del recurso final no es una priori conocida. Por ejemplo, un POST en `/users` crea un nuevo usuario con un nuevo ID, el cual entonces puede ser retornado desde, por ejemplo `/users/1`. POST **no** es idempotente, y es aplicable en toda acción non-CRUD. |

## Ejemplo de una API CRUD

Este escenario será un pequeño sistema CRM (Customer Relationship Mangement) para una compañía, con los siguientes requerimientos:

- El CRM tiene contactos, y cada contacto tiene notas y emails junto a ellos.
- Los consumidores de la API pueden crear, leer, actualizar, y borrar contactos, así como notas.
- Acceso a los emails por la API es de solo-lectura.

Basado en esto, tenemos los siguientes recursos:

- Todos los contactos: `/contacts`
- Contacto individual: `/contacts/{cID}`
- Notas para un contacto: `/contacts/{cID}/notes`
- Nota individual: `/contacts/{cID}/notes/{nID}`
- Emails para un contacto: `/contacts/{cID}/emails`
- Email individual: `/emails/{eID}`
 
 Aclaremos unas cosas. ¿Por qué usamos `/emails/{eID}` y no `contacts/{cID}/emails/{eID}` para un correo individual, aunque la lista de emails existe en `/contacts/{cID}/emails`, y seguimos este patrón para las notas?

En pocas palabras, esto se concentra en el tipo de identificadores que uses. En este diseño, el razonamiento pudo ser que los `Email IDs` son globalmente únicos, pero los `Note IDs` no lo son. Un buen diseño de APIs mantiene las URLs lo más cortas posibles.

Debemos definir las operaciones, las cuales son una combinación de una URL y un verbo expuesto por la API. Las implementaciones de frameworks las llaman *routes*. Así que, ¿qué operaciones, o rutas, necesitamos?

Para los contactos, podemos hacer todo en acciones CRUD, que serían las siguientes:

- **GET** `/contacts`
- **POST** `/contacts`
- **GET** `/contacts/{cID}`
- **PUT** `/contacts/{cID}`
- **DELETE** `/contacts/{cID}`

Estas son las cinco acciones que verás la mayoría del tiempo: GET y POST en la URL de lista de recurso, y GET, PUT y DELETE en las URLs de recurso individuales. Nota que las llaves curveadas indican un placeholder. Pocos frameworks de APIs presentan esta estructura por default.

El mismo acercamiento aplica para notas, excepto que tendremos URLs más largas con (a veces) dos placeholders:

 - **GET** `/contacts/{cID}/notes`
- **POST** `/contacts/{cID}/notes`
- **GET** `/contacts/{cID}/notes/{nID}`
- **PUT** `/contacts/{cID}/notes/{nID}`
- **DELETE** `/contacts/{cID}/notes/{nID}`

Finalmente, para los correos, tenemos pocas operaciones ya que son de solo-lectura:

- **GET** `/contacts/{cID}/emails`
- **GET** `/emails/{eID}`


Bajo estas especificaciones, podemos empezar a desarrollar una API con nuestro lenguaje y framework de preferencia.
Algunas recomendaciones son:
- [Go con Echo](https://echo.labstack.com/)
- [Node.js con Express](https://expressjs.com/es/)
- [Python con Django](https://www.django-rest-framework.org/)

Los atributos de estas clases son libres, mientras contengan las relaciones entre ellas.

Para simplificar, se puede optar por usar una pseudo base de datos usando JSON, o usar SQLite. En lo posible, usar ORMs.

## Referencias
https://blog.stoplight.io/crud-api-design
### Lecturas recomendadas
- https://www.rose-hulman.edu/class/csse/csse490WebServicesDev/201620/Slides/RESTAPI.pdf
- https://github.com/whitehouse/api-standards
- https://opensource.zalando.com/restful-api-guidelines/index.html#introduction
- https://geemus.gitbooks.io/http-api-design/content/en/