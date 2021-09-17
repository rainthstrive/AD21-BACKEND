# Code Smells

***El código no huele, hiede.***

Los code smells (*hediondez de código*) son decisiones pobres de implementación, que hacen difíciles de mantener a los sistemas orientados a objetos. Lo que determina qué es y no es un code smell es subjetivo, y varía entre lenguajes, desarrolladores, y metodología de desarrollo.
Según el libro *Refactoring for Software Design Smells*, "Los smells son ciertas estructuras en el código que indican una infracción en principios de diseño fundamentales y que impactan negativamente a la calidad de diseño". **Los smells no son bugs**; no son técnicamente incorrectos y no previenen el funcionamiento del programa. Más bien, son indicadores de debilidades en diseño que pueden ralentizar el desarrollo o incrementar el riesgo de bugs o fallas en el futuro. Los code smells también pueden ser indicadores de factores que contribuyen a *deuda técnica*. Incluso, según Robert C. Martin, autor de *Clean Code*, podríamos llamar a una lista de code smells como un "sistema de valor" para nuestras habilidades de desarrollo de software.

## Tipos de Code Smells

A continuación veremos una lista de smells, con su descripción, y propuestas de cómo resolverlos, de forma genérica a lenguajes de programación.

### Smells Dentro de Clases
| Tipo| Descripción |
|--|--|
| Comments| Existe una fina línea entre comentarios que "iluminan" y comentarios que "oscurecen". ¿Los comentarios que escribes son necesarios? ¿Explican el "por qué" y no el "qué"? ¿Puedes refactorizar el código para los comentarios no sean requeridos? Y por último, escribe comentarios para que sean entendidos por otras personas.|
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

### Smells Entre Clases
| Tipo| Descripción |
|--|--|
| Clases Alternativas con Diferentes Interfaces | Si dos clases son similares por dentro, pero diferentes por fuera, entonces tal vez pueden ser modificadas para compartir una interface en común |
| Obsesión por los Primitivos | No uses un montón de variables con tipos de dato primitivos si tu programa es complejo. Es muchísimo mejor utilizar una clase para datos completos |
| Clases de Datos | Evita las clases que pasivamente contienen datos. Las clases deberían contener tanto *datos* como *métodos* para operar esos mismos datos. |
| Grupos de datos | Si ves que varios datos siempre están enviándose en grupos, entonces sería bueno agruparlos en sus propias clases. |
| Legado rechazado | Si heredas de una clase, pero nunca usas alguna de sus funcionalidades heredadas, ¿vale la pena usar esa herencia? |
| Intimidad Inapropiada | Hay que estar alertas de clases que pasan mucho tiempo juntas, o clases que se comunican de formas inapropiadas. Las clases deben saber tan poco sea posible de la existencia de las otras. |
| Exposición indecente | Debemos tener cuidado de clases que expongan sus atributos internos. Hay que refactorizar agresivamente para minimizar su apariencia pública. Hay que tener bien justificados cada elemento que hacemos público. Si no, hay que esconderlo. |
| Envidia de Características | Métodos que hacen uso extensivo de otra clase puede que pertenezcan a otra clase. Considera mover ese método a la clase de la cual tenga tanta "envidia". |
| Clase Floja | Las clases deben poder jalar su peso. Toda clase adicional incrementa la complejidad de un proyecto. Si tienes una clase que no está haciendo lo suficiente para justificarse, ¿puede esta clase ser colapsada o combinada dentro de otra clase? |
| Cadenas de Mensajes | Esto ocurre cuando un cliente hace una petición a un objeto, entonces ese objeto hace otra petición, y así sucesivamente. Estas cadenas significan que el cliente es dependiente de la navegación a lo largo de la estructura de las clases. Cualquier cambio en estas relaciones requieren modificar el cliente. Un tratamiento a este smell es por medio de [Ocultar Delegados](https://refactoring.guru/es/hide-delegate). |
| Middle Man | Si una clase está delegando todo su trabajo, ¿por qué existe? Corta el intermediario. Ten cuidado con clases que funcionan meramente como wrappers sobre otras clases o funcionalidades de un framework. |
| Cambio Divergente | Cuando haces demasiados cambios a una sola clase, puede que te encuentres haciendo cambios a métodos no relacionados a las funcionalidades de la clase. Entonces considera aislar las partes que cambiaron hacia otra clase. |
| Shotgun Surgery | Si un cambio en una clase crea una cascada de cambios a más clases relacionadas, considera refactorizar de tal modo que los cambios sean limitados a una única clase. |
| Jerarquías de herencia paralela | Cuando creas una subclase para una clase, puede que encuentres necesario crear una subclase para otra clase. Todo estaba bien mientras la jerarquía se mantenía pequeña. Pero con nuevas clases siendo agregadas, hacer cambios se hacer más y más difícil. Podemos solucionarlo en dos pasos. Primero, haz que las instancias de una jerarquía se refieran a las instancias de otra jerarquía. Después, remueve la jerarquía en la clase referida, usando el Método Move, y el Campo Move. |
| Librería de Clase Incompleta | Necesitamos un método que no existe en una librería externa, pero no estamos dispuestos o en el poder de cambiar la librería para incluir el método. Una solución independiente del vendor de la librería es  crear una nueva clase de pseudo-librería, y usar esa nueva pseudo-librería en adelante. |
| Soluciones Expandidas | Si toma 5 clases para hacer algo útil, puede que hayas hecho una solución expandida. Para solucionarlo, considera simplificar y consolidar tu diseño. |

## Referencias
 - https://blog.codinghorror.com/code-smells/
 - https://www.industriallogic.com/blog/smells-to-refactorings-cheatsheet/
 - https://refactoring.guru/es/smells/parallel-inheritance-hierarchies
 - https://pragmaticways.com/31-code-smells-you-must-know/
 - https://www.sciencedirect.com/book/9780128013977/refactoring-for-software-design-smells
 - https://dzone.com/articles/code-smell-series-parallel-inheritance-hierchies
 - http://xp.c2.com/YouArentGonnaNeedIt.html
