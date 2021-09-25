# Seguridad

La seguridad en las aplicaciones es el proceso de hacer a estas más confiables por medio de encontrar, y mejorar sus propios métodos de seguridad. Mucho de esto sucede durante la fase de desarrollo, pero incluye herramientas y métodos de protección aún y cuando hayan sido desplegadas en ambientes productivos.

Mientras más rápido y pronto se encuentren estas fallas, más segura se volverá la empresa/grupo de uno. Porque todos cometemos errores, el reto se vuelve encontrar esos errores a tiempo.​

Por ejemplo, un error de código común es permitir inputs no verificados. Este error se puede convertir en un ataque de inyección SQL, y luego en un filtrado de datos si es que un hacker los encuentra.

## Hashing

El punto de una buena autenticación es el proveer a usuarios con un set de credenciales, tales como username y password, y verificar que esas credenciales sean correctas donde sea que accedan a la aplicación. Entonces necesitamos una forma de guardar estas credenciales en nuestra base de datos para una futura comparación.​

Guardar contraseñas en el servidor para autenticarse es una tarea complicada, pero podemos explorar uno de los mecanismos que hace el guardado de contraseñas más seguro y fácil: hashing.​

![enter image description here](https://images.ctfassets.net/23aumh6u8s0i/ES2U6Gx7w0yVF9Asidr26/75531c3695f09272142b543a94acc0de/hash-flow)

Según su definición, el hashing se refiere a "*cortar algo en pequeñas piezas*" para hacerlo ver como un "*desastre confuso*". Esto aplica muy cerca en el lado de la computación.​

En criptografía, una función *hash* es un algoritmo matemático que mapea los datos de cualquier tamaño a un bit string de tamaño fijo. Nos podemos referir a la función de input como el mensaje, o simplemente como **input**. La función que forma el string de salida es conocida como el **hash** o como el **message digest**.​

Las funciones hash tienen las siguientes propiedades:​

-   Es fácil y práctico calcular el hash, pero "es difícil o imposible volver a generar la entrada original si solo se conoce el valor del hash".​
-   Es difícil crear una entrada inicial que coincida con una salida deseada específica.​

Por lo tanto, el hash es un mecanismo unidireccional. Los datos hasheados no pueden ser simplemente "deshasheados"​.

Los servidores web guardan estos datos hasheados y los enlazan con sus usuarios, y cada vez que ingresamos nuestro par de usuario-contraseña, este último dato es comparado hash contra hash en base de datos para comprobar la autenticidad de los datos.

### Algoritmos aprobados para funciones Hash

La NIST (Institución Nacional de Estándares de Tecnología) lista dos estándares de procesamiento: la [FIPS 180-4](https://csrc.nist.gov/publications/detail/fips/180/4/final), y [FIPS 202](https://csrc.nist.gov/publications/detail/fips/202/final):

La FIPS 180-4 especifica siete algoritmos de hash:
 
 -   **_SHA-1_**  (Secure Hash Algorithm-1), y la
-   SHA-2 familia de algortimos hash  **_SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224,_** and **_SHA-512/256_**.

La FIPS 202 incluye:

-   Cuatro algoritmos hash con tamaño fijo:  **_SHA3-224, SHA3-256, SHA3-384,_**  y  **_SHA3-512_**; y
-   Dos funciones muy similares, con “salida-extendible" (XOFs [Extendible Output Functions]):  **_SHAKE128_**  y **_SHAKE256_**.

## Salt & Pepper

Ambos Salt y Pepper se refieren a datos que han sido generados y anexados a algún otro datos (en muchos casos contraseñas) antes que su resultado combinado se pase a una función hash que nos da en salida un dato digerido (digested) el cual es **casi** imposible de revertir.

En Criptografía, la expresión se refiere a agregar un dato aleatorio hacia el input de una función hash para garantizar una salida única, el hash, incluso cuando los inputs sean iguales. 

### Salt

Un salt es un dato extra y único que es usualmente generado de manera aleatoria y añadido a nuestra contraseña (o dato a proteger, en general), el resultado hash bajo un mismo texto no tendrá el mismo resultado.

```shell
hash(salt . "hello") = 58756879c05c68dfac9866712fad6a93f8146f337a78t  
hash(salt . "hello") = c0e81794384491161f1777c232bc6bd9ec38f616560b1
```

Esto ayuda produciendo hashes más seguros producidos por un salt, que nos protege contra ataques de tablas de hash, y disminuye otros ataques como los de fuerza bruta.​

Aún así, el robo de contraseñas no es imposible, el re-uso de salts o uso de salts muy cortos pueden permitir que se usen ataques de rainbow table si esas tablas contienen todo posible salt que haya sido anexado a cada contraseña.

#### Consejos de buenos Salts
La seguridad de este esquema no depende de ocultar, dividir u oscurecer los salts. En pocas palabras, *no te metas con el salt*. El salt no necesita estar encriptada, por ejemplo. Los salts están en su lugar para evitar que alguien descifre contraseñas en general y se pueden almacenar en texto plano, sin cifrar, en la base de datos. Sin embargo, **no pongas las sales a disposición del público**. Por esta razón, los nombres de usuario son malos candidatos para usar como salts.​

**Los hashing salts son como topes en la calle de un atacante que quiere acceder a tus datos. No importa si están visibles y sin cifrar, lo que importa es que estén en su lugar.**

### Pepper

El pepper funciona en una manera similar al salt a que son datos anexados al dato a proteger antes de ser hasheado. Pero, la diferencia principal es que mientras el salt es guardado con el valor hasheado, el valor pepper es escondido del valor hasheado.
​
El beneficio aquí es que el pepper no llega a ser conocido por el potencial atacante, ya que existen típicamente separados de la información de usuarios o guardada en el código fuente de la aplicación. Los salts, por otra parte, pueden estar guardados en la misma base de datos donde se encuentran sus usuarios.

## JSON Web Tokens

*JSON Web Token* (JWT) es un estándar abierto que define una forma compacta y autónoma de transmitir información de forma segura como un objeto JSON entre las partes de una aplicación. Esta información se puede verificar y confiar porque está firmada digitalmente. Los JWT se pueden firmar usando un *secret* (con el algoritmo HMAC) o un par de claves pública / privada usando RSA o ECDSA.​

Aunque los JWT se pueden encriptar para proporcionar seguridad entre ambas partes, nos centraremos en los tokens firmados. 

### Tokens Firmados (Signed)

Los tokens firmados pueden verificar la integridad de los claims contenidos en él, mientras que los tokens encriptados ocultan esos claims a otras partes. Cuando los tokens se firman utilizando pares de claves públicas / privadas, la firma también certifica que solo la parte que posee la clave privada es la que la firmó.​

### Casos de uso del JWT

-   **Autorización**: este es el escenario más común para usar JWT. Una vez que el usuario haya iniciado sesión, cada solicitud posterior incluirá el JWT, lo que le permitirá acceder a rutas, servicios y recursos que están permitidos con ese token. El inicio de sesión único (Single Sign On) es una función que utiliza ampliamente JWT en la actualidad, debido a su pequeña sobrecarga y su capacidad para usarse fácilmente en diferentes dominios.​
    
-   **Intercambio de información**: los JWT son una buena forma de transmitir información de forma segura entre sistemas. Debido a que los JWT se pueden firmar, por ejemplo, utilizando pares de claves públicas / privadas, puede estar seguro de que los remitentes son quienes dicen ser. Además, como la firma se calcula utilizando el header y el payload (el cuerpo del JWT), también puede verificar que el contenido no haya sido manipulado.​

### Estructura de un JWT

En su forma compacta, los JWT consisten de tres partes, separadas por puntos, estos son:

 - **Header**: La cabecera tipicamente consiste de dos partes, que son el tipo de token, que es JWT, y el algoritmo de firma usado, como HMAC SHA256, o RSA.
	 
	 Por ejemplo:
	  ```json
	  { 
		  "alg": "HS256", 
		  "typ": "JWT" 
	  }
	  ```
 - **Payload**: La segunda parte del token es el payload, el cual contiene los claims. Los claims son sentencias acerca de una entidad (tipicamente, un usuario) y datos adicionales. Existen tres tipos de claims: *registered*, *public*, y *private*.
	 - **Registered** claims: Son un set de claims predefinidos que no son mandatorios pero si recomendados, que proveen un set de claims utiles e interoperables.
		 - Algunos de estos son: iss (issuuer: quien realizó el token), exp (tiempo de expiración), sub (subject), aud (audiencia), entre [otros](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1). 

	 - **Public** claims: Estos pueden ser definidos a voluntad por aquellos usando JWTs. Pero para evadir colisiones con nombres reservador (como los registered) estos deben estar definidos en el [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml), o ser definidos como una URI que contenga un espacio de nombres resistentes a colisiones.

	- **Private** claims: Estos son claims personalizados que comparten información entre sistemas que se comprometen en usarlos, **y no son ni registered o public**.

Un ejemplo de un payload sería:

```json
{ 
	"sub": "1234567890", 
	"name": "John Doe", 
	"admin": true 
}
```
___
### Nota
Hay que aclarar que mientras que los tokens son formados para protegernos de peticiones maliciosas, estos son legibles por cualquiera. **No ingreses información secreta en el payload o headers de un JWT a menos que sean encriptados.**
___

 - **Signature**: Para crear la parte de la firma, tienes que tomar la parte encodificada del header, del payload, un *secret*, el algoritmo especificado en el header, y firmarlo
Por ejemplo, si queremos usar el algoritmo HMAC SHA256, la firma sería creada de la siguiente forma:

```shell
HMACSHA256( 
	base64UrlEncode(header) + "." + 
	base64UrlEncode(payload), 
	secret)
```

La firma es usada para verificar que el mensaje no haya sido cambiado a lo largo del camino, y en el caso de los tokens firmados con una llave privada, también se puede verificar que el remitente del JWT es quien dice ser que es.

### Uniéndolo todo

La salida del token son tres Base64-URL strings, separados por puntos, que pueden ser pasados en ambientes HTML y HTTP.

El siguiente muestra un JWT que ha sido creado con el header y payload mostrados previamente, y firmados con un *secret*.

![enter image description here](https://cdn.auth0.com/content/jwt/encoded-jwt3.png)

![enter image description here](https://research.securitum.com/wp-content/uploads/sites/2/2019/10/jwt_ng1_en.png)

### Debuggeando tokens

Si quieres decodificar un JWT, podemos usar el [jwt.io Debugger](https://jwt.io/#debugger-io)

![enter image description here](https://cdn.auth0.com/blog/legacy-app-auth/legacy-app-auth-5.png)

### Entonces, ¿cómo funcionan los JWT en la seguridad?

Cada vez que un usuario hace login con sus credenciales, un JSON Web Token le es otorgado. Ya que los tokens también son credenciales, debemos tener cuidado de cómo los manejamos. En general, no debemos mantener tokens vivos más tiempo del requerido.

Cuando sea que un usuario quiera acceder a una ruta protegida o recurso, este debe enviar el JWT, tipicamente en la cabecera de **Authorization**, usando el schema **Bearer**.

El header se vería de la siguiente forma:

```shell
Authorization: Bearer <token>
```

Esto puede ser, en ciertos casos, un mecanismo de autorización sin estado (stateless). Las rutas protegidas del servidor verificarán por un JWT válido en la cabecera enviada, y si está presente, le permitirá al usuario acceder a los recursos protegidos. 

En el siguiente diagrama podemos notar cómo conviven las peticiones de tokens y hacia rutas protegidas:

![enter image description here](https://cdn2.auth0.com/docs/media/articles/api-auth/client-credentials-grant.png)

1. La aplicación o cliente hace una petición de autorización al Authorization Server. 
2. Cuando la autorización es concedida, el server le dará un token de acceso a la aplicación.
3. La aplicación usará el token de acceso para interactuar con el recurso (como una API).

## Referencias
-   https://www.csoonline.com/article/3315700/what-is-application-security-a-process-and-tools-forsecuring-software.html
-   https://auth0.com/blog/hashing-passwords-one-way-road-to-security/
-  https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/​
- https://jwt.io/introduction
-  https://dev.to/sureshdsk/how-jwt-json-web-token-authentication-works-2635
- https://medium.com/@berto168/salt-pepper-spice-up-your-hash-b48328caa2af
- https://www.iana.org/assignments/jwt/jwt.xhtml
- https://csrc.nist.gov/projects/hash-functions