# Lei de Demeter X Tell, don't Ask - by @EDUMATT3 

Ao ler os conteúdos, muitas vezes acabo confundindo os conceitos.
Talvez eu não tenha muito claro as diferenças das duas diretivas.
Ao meu entender, eles se **sobrepõem**.

As diferenças que consigo ver são:

Lei de Demeter: Te orienta para definir o **acoplamento entre as unidades** do escopo do seu código.
Tell, don't Ask: Te direciona a como **definir as responsabilidades** dos seus objetos e a **não terceirizar comportamentos**.

Mas a minha percepção é que para cumprir essas boas práticas **o código produzido acaba sendo o mesmo**.

**O que pensam?**

## Comment 1 by @rafaelpontezup

Excelente questionamento, @EDUMATT3 ! 👏🏻👏🏻

De fato ambas as técnicas tendem a levar pro mesmo resultado de código em diversos cenários, mas na minha opinião o _Tell, Don't Ask_ parece ser mais "completinho". Ele te fornece o resultado da _Law of Demeter_ e ainda te permite desenhar interações entre objetos através de troca de mensagens (comandos), o que naturalmente te leva a um bom nível de encapsulamento!

A verdade é que ambos são baseados em um principio ainda maior conhecido como **[Information Hide](https://www.careerride.com/oops-information-hiding.aspx)**. Que basicamente fala sobre encapsulamento e abstrações para esconder detalhes de implementação e o que não importa para quem consome nossa API (ou seja, nosso código).

Muitos autores consideram _Law of Demeter_ muito rigida e restritiva, o que pode levar a um código mais dificil pois basicamente você tenta encapsular TUDO, o que dificulta a navegação dentro de um objeto. Não à toa alguns autores preferem não chama-la de Lei (law) mas sim de "sugestão" ou "recomendação". 

No mais, eu separei alguns links que navegam nessas diferenças entre as técnicas e podem te ajudar a assimilar melhor cada uma delas:

- https://blog.testdouble.com/posts/2022-06-15-law-of-demeter/
- https://www.oneworldcoders.com/apprentice-journals/law-of-demeter
- https://hackernoon.com/object-oriented-tricks-2-law-of-demeter-4ecc9becad85
- https://www.infoworld.com/article/3136224/demystifying-the-law-of-demeter-principle.html
- http://blog.jasonharrelson.com/blog/2014/03/04/ruby-oop-events-and-the-tell-dont-ask-principle/
- https://thoughtbot.com/blog/tell-dont-ask
- http://www.mockobjects.com/2006/10/tell-dont-ask-and-mock-objects.html
- https://www.slideshare.net/nelsonsar/melhorando-seu-cdigo-com-law-of-demeter-e-tell-dont-ask
- https://www.linkedin.com/pulse/beautiful-law-demeter-ioan-tinca/
- https://www.oreilly.com/library/view/implementing-domain-driven-design/9780133039900/ch10lev2sec23.html#ch10lev2sec23 (esse aqui precisa ter conta na O'Reilly 😰)

Tem muita informação boa nos links acima, se tiver com tempo e interesse vale a pena consumi-los e aumentar seu repertório de heuristicas de código 😎


