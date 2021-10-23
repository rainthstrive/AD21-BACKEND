# CORS

- El Intercambio de Recursos de Origen Cruzado (CORS) es un mecanismo que utiliza cabeceras HTTP adicionales para permitir que un cliente obtenga permiso para acceder a recursos seleccionados desde un servidor, en un origen distinto (dominio) al que pertenece. Un agente/cliente crea una petición HTTP de origen cruzado cuando solicita un recurso desde un dominio distinto, un protocolo o un puerto diferente al del documento que lo generó.​
    
- Un ejemplo de solicitud de origen cruzado: el código JavaScript frontend de una aplicación web que es localizada en http://domain-a.com utiliza XMLHttpRequest  para cargar el recurso http://api.domain-b.com/data.json.​

- Por razones de seguridad, los exploradores restringen las solicitudes HTTP de origen cruzado iniciadas dentro de un script. Por ejemplo, XMLHttpRequest y la API Fetch siguen la política de mismo-origen (same  origin). Ésto significa que una aplicación que utilice esas APIs  XMLHttpRequest sólo puede hacer solicitudes HTTP a su propio dominio, a menos que se utilicen cabeceras CORS.​

![enter image description here](https://mdn.mozillademos.org/files/14295/CORS_principle.png)

## CORS en Go con el Framework Echo

La especificación del middleware CORS es totalmente usable con Echo.
El CORS le da a los servidores web control de acceso de transferencias seguras de dominios cruzados.

### Uso

```go
e.Use(middleware.CORS())
```

```go
e := echo.New()
e.Use(middleware.CORSWithConfig(middleware.CORSConfig{
  AllowOrigins: []string{"https://labstack.com", "https://labstack.net"},
  AllowHeaders: []string{echo.HeaderOrigin, echo.HeaderContentType, echo.HeaderAccept},
}))
```

### Receta de CORS

Servidor permitiendo una lista definida de orígenes

```go
package main

import (
	"net/http"

	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
)

var (
	users = []string{"Joe", "Veer", "Zion"}
)

func getUsers(c echo.Context) error {
	return c.JSON(http.StatusOK, users)
}

func main() {
	e := echo.New()
	e.Use(middleware.Logger())
	e.Use(middleware.Recover())

	// CORS default
	// Allows requests from any origin wth GET, HEAD, PUT, POST or DELETE method.
	// e.Use(middleware.CORS())

	// CORS restricted
	// Allows requests from any `https://labstack.com` or `https://labstack.net` origin
	// wth GET, PUT, POST or DELETE method.
	e.Use(middleware.CORSWithConfig(middleware.CORSConfig{
		AllowOrigins: []string{"https://labstack.com", "https://labstack.net"},
		AllowMethods: []string{http.MethodGet, http.MethodPut, http.MethodPost, http.MethodDelete},
	}))

	e.GET("/api/users", getUsers)

	e.Logger.Fatal(e.Start(":1323"))
}

```
___
Servidor permitiendo **todos** los orígenes:

```go
package main

import (
	"net/http"
	"regexp"

	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
)

var (
	users = []string{"Joe", "Veer", "Zion"}
)

func getUsers(c echo.Context) error {
	return c.JSON(http.StatusOK, users)
}

// allowOrigin takes the origin as an argument and returns true if the origin
// is allowed or false otherwise.
func allowOrigin(origin string) (bool, error) {
	// In this example we use a regular expression but we can imagine various
	// kind of custom logic. For example, an external datasource could be used
	// to maintain the list of allowed origins.
	return regexp.MatchString(`^https:\/\/labstack\.(net|com)$`, origin)
}

func main() {
	e := echo.New()
	e.Use(middleware.Logger())
	e.Use(middleware.Recover())

	// CORS restricted with a custom function to allow origins
	// and with the GET, PUT, POST or DELETE methods allowed.
	e.Use(middleware.CORSWithConfig(middleware.CORSConfig{
		AllowOriginFunc: allowOrigin,
		AllowMethods:    []string{http.MethodGet, http.MethodPut, http.MethodPost, http.MethodDelete},
	}))

	e.GET("/api/users", getUsers)

	e.Logger.Fatal(e.Start(":1323"))
}

```

## Referencias

- https://developer.mozilla.org/es/docs/Web/HTTP/CORS
- https://echo.labstack.com/middleware/cors/