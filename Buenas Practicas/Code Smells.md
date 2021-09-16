# Code Smells

***El código no huele, hiede.***

Los code smells (*hediondez de código*) son decisiones pobres de implementación, que hacen difíciles de mantener a los sistemas orientados a objetos. Lo que determina qué es y no es un code smell es subjetivo, y varía entre lenguajes, desarrolladores, y metodología de desarrollo.
Según el libro *Refactoring for Software Design Smells*, "Los smells son ciertas estructuras en el código que indican una infracción en principios de diseño fundamentales y que impactan negativamente a la calidad de diseño". **Los smells no son bugs**; no son técnicamente incorrectos y no previenen el funcionamiento del programa. Más bien, son indicadores de debilidades en diseño que pueden ralentizar el desarrollo o incrementar el riesgo de bugs o fallas en el futuro. Los code smells también pueden ser indicadores de factores que contribuyen a *deuda técnica*. Incluso, según Robert C. Martin, autor de *Clean Code*, podríamos llamar a una lista de code smells como un "sistema de valor" para nuestras habilidades de desarrollo de software.

## Tipos de Code Smells

A continuación veremos una lista de smells, con su descripción, y propuestas de cómo resolverlos, de forma genérica a lenguajes de programación.

### Smells Dentro de Classes
| Tipo| Descripción |
|--|--|
| Comments|Existe una fina línea entre comentarios que "iluminan" y comentarios que "oscurecen". ¿Los comentarios que escribes son necesarios? ¿Explican el "por qué" y no el "qué"? ¿Puedes refactorizar el código para los comentarios no sean requeridos? Y por último, escribe comentarios para que sean entendidos por otras personas.|
| Métodos largos| Los métodos cortos son más sencillos de leer, más fáciles de entender, y también más rápidos para resolver. Refactoriza métodos largos en varios métodos cortos cuando sea posible. |
| Larga lista de parámetros | Mientras más parámetros tenga un método, más complejo se vuelve. Limita el número de parámetros que se necesitan en un método, o usa un objeto para combinar los parámetros |
| Código duplicado | El código duplicado es una de peores prácticas en el desarrollo. Elimina la duplicación donde sea que sucede y sea posible. También debemos estar alertas para situaciones donde puedan haber futuros casos de duplicación. *[Don't Repeat Yourself](https://www.artima.com/articles/orthogonality-and-the-dry-principle)* |
| Complejidad Condicional | Debemos tener cuidados con largos bloques de lógica condicional, particularmente en bloques que crezcan o cambien significativamente con el tiempo. Aquí se deben considerar acercamientos orientados a objetos, cómo el patrón de [Decorador](https://refactoring.guru/es/design-patterns/decorator), [Estrategia](https://refactoring.guru/es/design-patterns/strategy), o [Estado](https://refactoring.guru/es/design-patterns/state). |
| Explosión Combinatoria | Tienes muchas líneas de código que hacen *casi* lo mismo... pero tienen pequeñas variaciones en datos o comportamiento. Son una forma sutil de duplicación, y no tienen un reparo rápido sin tener que rediseñar el sistema. El primer paso para evitar o solucionarlo es usando patrones de diseño desde el principio. |
| Clases Largas | Las clases largas, como los métodos largos, son dificiles de leer, entender, y mantener. ¿La Clase contiene demasiadas responsabilidades? ¿Puede la clase ser reconstruida o dividida en pequeñas subclases? |
| Tipado de dato embebido en nombres  | Evita escribir el tipo de dato dentro nombres de métodos; ya que no solo es redundante, pero también fuerza a uno cambiar el nombre si cambia el tipo de dato. |
| Nombres inconsistentes | Elige una terminología estándar y apegate a ella a lo largo de los métodos. Por ejemplo, si tienes Open(), probablemente debas tener Close(). |
| Código Muerto | Borra todo el código que ya no sirve o no está en uso. Si usas sistemas de versión como Git, no sería problema volver a un commit pasado y validar ese código en caso de ser necesario |
| Generalidad Especulativa | Escribe código que resuelva los problemas de hoy, y preocúpate por los problemas de mañana cuando se materialicen. [Tal vez no sea necesario especular tanto.](http://xp.c2.com/YouArentGonnaNeedIt.html) |
| Soluciones Extrañas | Solo debe existir una forma de resolver el mismo problema en tu código. Si encuentras más de una, entonces tenemos una solución extraña. Elimina estas soluciones innecesarias, pero si son realmente requeridas, considera implementar el [patrón de diseño Adapter](https://refactoring.guru/es/design-patterns/adapter). |
| Campo Temporal | Ten cuidado de objetos que contengan muchos campos opcionales o innecesarios. Si pasas un objeto como un parámetro para un método, asegúrate que estás usando todas sus propiedades, y no solo tomando un único campo. |

## Solucionando Smells
Los problemas causados por los smells pueden ser descubiertos cuando el código 

## Referencias
 - https://blog.codinghorror.com/code-smells/
 - https://pragmaticways.com/31-code-smells-you-must-know/
 - https://www.sciencedirect.com/book/9780128013977/refactoring-for-software-design-smells
