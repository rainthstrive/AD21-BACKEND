# Despliegue de Aplicaciones

Derivado del tema de ambientes de despliegue, tenemos dos posibilidades al momento de desplegar nuestras aplicaciones a diferentes ambientes. Si decidimos trabajar en nuestra computadora (ambiente local), o si decidimos usar un servidor remoto (producción).

En cuanto decidamos queramos desplegar hacia un ambiente, debemos elegir que tipo de despliegue realizar y a cuál ambiente realizar ese despliegue.
 
Para ambientes locales, nos basta con cambiar de branches en un repositorio en Git y validar con variables de entorno las configuraciones de nuestra API. Pero en cuanto decidimos realizar un despliegue a un servidor remoto, como en ambientes productivos/vivos, es donde debemos tomar una decisión mejor pensada.

## ¿Cómo saber que servidor remoto elegir?

Antes que existieran los servidores en la nube, lo normal era que las compañías y emprendedores hostearan sus aplicaciones dentro de servidores propios. Desde la popularización de servicios como AWS, Google Cloud, y Azure, no solamente se ha vuelto más sencillo tener una aplicación en Internet, si no que incluso se ha vuelto gratis (un servidor propio requiere un **pago constante de electricidad, servicio constante de internet, capacidad de manejo de peticiones**, y más factores que lo vuelven 💲💲💲)

> Cabe notar que tenemos muchísimas opciones hoy en día, tanto el uso de
> grandes infraestructuras como Azure, Google Cloud, AWS, entre otros más sencillos como Netlify, Firebase de Google, y más. A donde subas tu API depende de tu propio análisis pros/contras.

Si quieres experimentar en tu propio servidor y ejecutar tus aplicaciones en un ambiente controlado entre testers de confianza (amigos, y colegas) una opción bastante popular es hacer un deployment hacia una computadora dedicada, como un Raspberry Pi (o incluso crear clusters de servidores por medio de muchos Pis enlazados). Esta es una excelente alternativa si lo que deseas es también tener una idea de cómo sería el mantenimiento de un servidor basado en Linux.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QdHvS0D1zAI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

___

Factores que influyen en tu decisión sobre qué servidor remoto elegir son las siguientes:
- Los requerimientos de tu software en particular
- Recursos externos (bases de datos, otros servicios/software/sitios)
- La carga de trabajo de la aplicación (más alta, promedio, en fila) 
- El rendimiento del valor de negocio (análisis costo/beneficio) 
- La expectativa de rendimiento por tus usuarios
- Cualquier contrato a nivel de servicio que puedas tener

Hacer un propio análisis de estos factores, y otros más, se encuentra más allá de sencillas respuestas. Requieren un conocimiento detallado acerca de tu ambiente y requerimientos, los cuales solamente tu equipo (o consultores adecuados) puedan obtener eficientemente.

___

En esta lectura, veremos cómo hacer un despliegue de una API REST hecha en Go hacia la plataforma App Engine de Google.

## Go (1.12+) en Google App Engine

**Google App Engine** es un servicio de alojamiento web que presta Google de forma gratuita hasta determinadas cuotas. Este servicio permite ejecutar aplicaciones sobre la infraestructura de Google.

Actualmente se divide en dos módulos:

-   Google App Standard Environment: Solo tiene posibilidad de programación con C#, Java, Python, PHP y Go. No Permite accesos a disco usando esto lenguajes, a cambio, los costes son más económicos.
-   Google App Flexible Environment : Puede implementar cualquier lenguaje de programación mediante contenedores Docker, permitiendo mayor control sobre la aplicación. Los costes son proporcionales al consumo de CPU.

En este ejemplo, usaremos el Standard Environment.

### Antes de Empezar
1. En la Google Cloud Console, dentro de la página de selección de proyectos, selecciona o crea un proyecto Google Cloud (lo puedes borrar al terminar el uso definitivo de la app).
[Ir al project selector](https://console.cloud.google.com/projectselector2/home/dashboard?_ga=2.219835486.696731797.1633170283-1025426034.1632715274)

2. Asegúrate que una forma de pago esté habilitada en tu proyecto.

3.  Habilita la Cloud Build API
[Habilitar](https://console.cloud.google.com/flows/enableapi?apiid=cloudbuild.googleapis.com&_ga=2.246516803.696731797.1633170283-1025426034.1632715274)

4. Instalar e inicializar el [Cloud SDK](https://cloud.google.com/sdk/docs/install)

### Prerequisitos Adicionales

1. Inicializa tu app de App Engine con tu proyecto y elige una región (el lugar físico donde se encuentran los servidores hacia los que mandamos las peticiones):

``` shell
gcloud app create --project=[YOUR_PROJECT_ID]
```

Cuando te permita, selecciona la región a donde quieres localizar tu aplicación. Esto no se puede cambiar una vez que se elige.

2. Instala los siguientes prerequisitos:
	- [Descarga e instala Git](https://git-scm.com/). 
	- Ejecuta el siguiente comando para instalar el [gcloud component](https://cloud.google.com/sdk/docs/managing-components) que incluye la extensión para Go versión 1.12+ (No usar versiones menores ya que su forma de despliegue es diferente).

```shell
gcloud components install app-engine-go
```

### Descarga la app Hello World

Google nos provee de una app sencilla que muestra "Hello World" como mensaje, y la usaremos para desplegarla en la nube. Primero descargaremos esta app en nuestras máquinas.

1. Clona el repositorio
```shell
git clone https://github.com/GoogleCloudPlatform/golang-samples.git
```

### Despliega Hello World en App Engine

1. Para desplegar, ejecutamos el siguiente comando desde el directorio de nuestra aplicación:
```shell
gcloud app deploy
```
2. Si ejecutamos correctamente, veremos nuestra app viva en https://PROJECT_ID.REGION_ID.r.appspot.com:
```
gcloud app browse
```

El mensaje *Hello, World!* se mostrará por nuestro nuevo web server corriendo en una instancia de App Engine.

¡Felicidades, has desplegado un servicio web hecho en Go con App Engine!

## Referencias
- https://serverfault.com/questions/384686/can-you-help-me-with-my-capacity-planning
- https://devcenter.heroku.com/categories/continuous-delivery
- https://cloud.google.com/appengine/docs/standard/go/quickstart

### Lecturas recomendadas
- Para programadores de Node.js https://www.youtube.com/watch?v=uEVmD6n8Il0