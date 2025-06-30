# Como os Garbage Collectors cuidam da sua memória para você

Garbage collectors (GC) são mecanismos que muitas linguagens de programação utilizam para liberar automaticamente a memória que não será mais utilizada dentro de um programa. Dessa forma, tais mecanismos têm por objetivo determinar quando uma parte da memória pode ser desalocada. Determinar se uma parte da memória não é mais necessária para o programa é um problema [indecidível](https://pt.wikipedia.org/wiki/Problema_indecid%C3%ADvel).

C é uma linguagem que não possui um GC. Quando escrevemos um código em C, precisamos deliberadamente alocar memória para criar objetos utilizando funções como [`malloc`](https://en.cppreference.com/w/c/memory/malloc) e [`calloc`](https://en.cppreference.com/w/c/memory/calloc). Além disso, é necessário desalocar esses espaços de memória utilizando a função [`free`](https://en.cppreference.com/w/c/memory/free) pois se não o fizermos é provável que a utilização de memória pelo programa aumente consideravelmente, podendo causar problemas inesperados. Assim, a utilização de um GC contribui para a diminuição desse tipo de problema e melhora a experiência do desenvolvedor, já que este não precisa mais se preocupar tanto com o gerenciamento da memória.

Existem vários tipos de GCs, com cada um variando a forma que identifica a memória que pode ser liberada. Uma das formas mais comuns e básicas de fazer essa verificação é por contagem de referências. A ideia aqui é criar um contador para cada objeto criado no programa. Esse contador guarda a quantidade de referências a esse objeto em um dado momento da execução do programa. A partir desses contadores, o GC liberará a memória de um objeto se seu contador chegar em algum momento a zero. Isso pode ser feito porque se nenhuma parte do programa referencia o objeto, ele não pode mais ser acessado. Um exemplo desse método pode ser visto a seguir:

```javaScript
let variable = {
  attribute: 10 // Objeto considerado
};
// Valor do contador do objeto considerado = 1

let anotherVariable = variable;
// Valor do contador do objeto considerado = 2

variable = 5;
// Valor do contador do objeto considerado = 1

anotherVariable = var;
// Valor do contador do objeto considerado = 0 -> memória pode ser liberada
```

Esse tipo de GC possui uma limitação bem conhecida de não lidar bem com referências circulares. Referências circulares acontecem quando dois objetos referenciam um ao outro. Quando dois objetos que referenciam um ao outro saem do escopo que foram criados, os espaços de memória dos mesmos podem ser liberados. Porém esse tipo de GC não consegue fazer essa identificação. Exemplo:

```javaScript
function exampleOfCircularReference() {
  const firstObject = {};
  // Valor do contador do objeto referenciado por firstObject = 1
  const secondObject = {};
  // Valor do contador do objeto referenciado por secondObject = 1
  secondObject.attribute = firstObject;
  // Valor do contador do objeto referenciado por firstObject = 2
  firstObject.attribute = secondObject;
  // Valor do contador do objeto referenciado por secondObject = 2
}

exampleOfCircularReference();
// As variáveis firstObject e secondObject saem do escopo que foram criados e são 'excluídas'. 
// Com isso, os objetos referenciados por essas variáveis poderiam ser liberados. 
// Mas isso não acontece pois cada objeto ainda é referenciado uma vez pelo outro objeto.
```

Outro tipo de GC é o *Mark-and-sweep*. Esse tipo de algoritmo usa a definição de inalcançável (*unreachable*) para determinar quais objetos não são mais necessários para a execução do programa e assim liberar a memória alocada para os mesmos. Tal algoritmo assume que um conjunto de objetos raízes (*roots*) existe e que a partir deles é possível acessar qualquer outro objeto do programa. Com isso, a partir dos *roots* o GC determina todos os objetos que são alcançáveis (*reachable*) e considera todos os outros objetos inalcançáveis, liberando a memória alocada para estes.

![Mark and sweep image](https://raw.githubusercontent.com/pdrzan/articles/refs/heads/master/02_garbage_collector/images/mark-and-sweep.png)

Esse tipo de GC não possui limitações relacionadas a referências circulares, como o último tipo de GC apresentado. No último exemplo, esse tipo de GC identificaria que depois da chamada de função nenhum dos objetos seria alcançável a partir dos *roots* (que seria o objeto [Global](https://developer.mozilla.org/en-US/docs/Glossary/Global_object) no caso de JavaScript) e com isso a memória associada a eles poderia ser liberada.

Um ponto a ser considerado é que os GCs consomem memória e processamento como qualquer outro algoritmo e por isso podem prejudicar a performance de um programa caso não sejam otimizados. Existem diversas formas de otimizar um GC, com essas técnicas tendo como objetivo diminuir o tempo de processamento e recursos utilizados. 

Uma dessas formas é dividir a memória em 'gerações' e fazer a 'coleta de lixo' em cada geração (espaço de memória) de forma distinta. A ideia de dividir a memória em gerações é fazer a 'coleta de lixo' mais frequentemente em espaços de memória recentemente alocados e menos frequentemente em espaços de memória 'antigos'. Com isso, os objetos 'antigos' e amplamente utilizados no programa não precisam ser constantemente considerados pelo GC, aumentando assim sua eficiência. Essa técnica funciona porque a maioria dos objetos criados em um programa tem um tempo de vida curto.

![Generational garbage collector](https://raw.githubusercontent.com/pdrzan/articles/refs/heads/master/02_garbage_collector/images/generational-garbage-collector.png)

Outra forma de otimização é executar o GC paralelamente com o programa, com a utilização de múltiplas threads. Dessa forma, o programa não precisa 'parar' para que o GC seja executado e com isso aumenta a performance do programa.

<hr>

Achou algum erro no artigo? Mande um email para pedeozanelato19@gmail.com!

## Referências
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Memory_management
- https://noncodersuccess.medium.com/understanding-garbage-collection-in-programming-languages-7c83e0a709b9
- https://www.techtarget.com/searchstorage/definition/garbage-collection
- https://www.dio.me/articles/uma-abordagem-profunda-sobre-garbage-collection-conceitos-algoritmos-e-desafios
- https://www.telerik.com/blogs/fundamentals-garbage-collection
- https://www.cs.cornell.edu/courses/cs3110/2012sp/lectures/lec26-gc/lec26.html
- https://wiki.osdev.org/Garbage_collection
