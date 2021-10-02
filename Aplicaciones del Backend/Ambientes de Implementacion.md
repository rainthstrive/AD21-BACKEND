# Ambientes de Implementación

Empezando por lo que es el despliegue, o *deployment*, significa empujar nuestros cambios de código desde un ambiente hacia otro. Por ejemplo, cuando configuras un sitio web siempre tendrás a tu sitio en vivo y funcionando, lo cual es llamado *live environment*, o ambiente de producción.

Si quieres la capacidad de hacer cambios sin que afecten a tu sitio vivo, entonces puedes agregar más ambientes. Estos ambientes se llaman ambientes de desarrollo, o de despliegue. 

Un ambiente refiere a hardware y software donde se ejecuta una aplicación. Dependiendo del proceso de desarrollo, se definirá la cantidad de ambientes por los cuales se irá propagando una aplicación desde desarrollo hasta producción.​

Cada ambiente tiene su propia base de datos y su copia de los binarios de la aplicación, para que de esta forma no haya interferencias en los ambientes y entre los diferentes desarrolladores en la construcción del software.

## ¿Por qué usar ambientes?

Implementar un ambiente sirve para facilitar el trabajo realizado por los desarrolladores, realizar un control de versiones, realizar pruebas, y actualizar la documentación de un proyecto.​
Este tipo de metodología **permite que el desarrollo de software se realice de forma ágil, acortando tiempos**.​

## Tipos de ambientes locales

### Desarrollo

- Este ambiente/entorno es propiamente referido a la creación del software. Este entorno puede ser manejado a nivel local en el ordenador del programador, en un servidor local, o un servidor remoto limitado. Asimismo, puede ser procesado en herramientas desarrolladas para este tipo de tareas.​
- En este espacio de trabajo el desarrollador codifica, realiza las pruebas iniciales y comprueba la ejecución del código.​
	- Ejemplo: Branch tipo *feature* donde se codifica una sola característica del software.​

### Integración

-   Es habitual que trabaje un equipo en el desarrollo de un proyecto, lo que hace que se realicen tareas de forma individual. El entorno de integración facilita la unión de los diferentes desarrollos y permite comprobar que los diferentes trabajos no interfieran entre sí. ​
	-   Ejemplo: *Merge* de *branches* tipo *feature* hacia un solo *branch* de desarrollo.​

### Pruebas/Testing/QA

-   Este tipo de entorno usualmente está ubicado en la nube. La ventaja de contar con un servidor de pruebas es que permite a otros miembros del equipo y clientes a interactuar con el software para realizar tests de funcionalidades del sistema.​
-   También se pueden realizar *testing automatizados*, su principal objetivo es detectar el mayor número de fallos posible, al no necesitar una persona que realice un testeo manual permite ahorrar tiempo. Además, la ejecución de pruebas automatizadas permite ejecutar *tests* durante las 24 horas del día los 7 días de la semana.​
	-   Nota: En proyectos donde esté designada una persona para QA, éste notificará los defectos en el software, para que el desarrollador los corrija al momento (idealmente).​
    
### Pre Producción

-   También llamado entorno de *staging*, permite trabajar en un entorno con una configuración exactamente igual a la que existe en el entorno de producción. La finalidad de usar este tipo de servidor es simular el entorno de producción para validar su usabilidad en un entorno real. Del mismo modo permite el poder realizar pruebas de actualizaciones y minimizar las causas que puedan generar caídas del sistema.​
	-   Nota: En este entorno también se designa un QA, y un equipo de líderes técnicos desarrollan el plan de despliegue a este ambiente para prevenir impactos a despliegues previos.​
	
### Producción/Live
-   Es el entorno donde finalmente se ejecuta el software, el cual es utilizado por el usuario final. Este servidor, a diferencia del servidor de pre-producción o anteriores, debería tener una mayor infraestructura y mayor capacidad de manejo de tráfico o de conexiones recurrentes.​
-   Si el entorno de producción está bien configurado, se han realizado pruebas automatizadas, y también por parte del usuario, no debería existir ninguna incidencia en la ejecución del software final.​
	-   Nota: En caso de haber *bugs* que impidan al usuario final seguir el flujo del software, estos bugs se reparan a la brevedad directo a producción cómo un *hotfix*. Esto implica un desbalance entre ambientes, y debe evitarse a toda costa.​

## Referencias​
- https://umbraco.com/knowledge-base/deployment/
- https://wiki.genexus.com/commwiki/servlet/wiki?29683,Manejo+de+ambientes+%28Desarrollo%2C+Testeo%2C+PreProduccion%2C+Producci%C3%B3n%29​
- https://www.ekon.es/entornos-desarrollo-software/
### Lecturas recomendadas
- https://danielkummer.github.io/git-flow-cheatsheet/
- https://codingsight.com/git-branching-naming-convention-best-practices/​​