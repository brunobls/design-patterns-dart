# Mediator Method
O Mediator é um padrão de projeto comportamental que permite que você reduza as dependências caóticas entre objetos. O padrão restringe comunicações diretas entre objetos e os força a colaborar apenas através do objeto mediador.

## Problema
Digamos que você tem uma caixa de diálogo para criar e editar perfis de clientes. Ela consiste em vários controles de formulário tais como campos de texto, caixas de seleção, botões, etc.

Alguns dos elementos do formulário podem interagir com outros. Por exemplo, selecionando a caixa de “Eu tenho um cão” pode revelar uma caixa de texto escondida para inserir o nome do cão. Outro exemplo é o botão enviar que tem que validar todos os campos antes de salvar os dados.

Ao ter essa lógica implementada diretamente dentro do código dos elementos de formulários você torna as classes dos elementos muito difíceis de se reutilizar em outros formulários da aplicação. Por exemplo, você não será capaz de usar aquela classe de caixa de seleção dentro de outro formulário porque ela está acoplado com o campo de texto do nome do cão. Você pode ter ou todas as classes envolvidas na renderização do formulário de perfil, ou nenhuma.

## Solução
O padrão Mediator sugere que você deveria cessar toda comunicação direta entre componentes que você quer tornar independentes um do outro. Ao invés disso, esses componentes devem colaborar indiretamente, chamando um objeto mediador especial que redireciona as chamadas para os componentes apropriados. Como resultado, os componentes dependem apenas de uma única classe mediadora ao invés de serem acoplados a dúzias de outros colegas.

No nosso exemplo com o formulário de edição de perfil, a classe diálogo em si pode agir como mediadora. O mais provável é que a classe diálogo já esteja ciente de todos seus sub-elementos, então você não precisa introduzir novas dependências nessa classe.

A mudança mais significativa acontece com os próprios elementos do formulário. Vamos considerar o botão de enviar. Antes, cada vez que um usuário clicava no botão, ele teria que validar os valores de todos os elementos de formulário. Agora seu único trabalho é notificar a caixa de diálogo sobre o clique. Ao receber essa notificação, a própria caixa de diálogo realiza as validações ou passa a tarefa para os elementos individuais. Portanto, ao invés de estar amarrado a uma dúzia de elementos de formulário, o botão está dependente apenas da classe diálogo.

Você pode ir além e fazer a dependência ainda mais frouxa extraindo a interface comum de todos os tipos de caixas de diálogo. A interface deve declarar o método de notificação que todos os elementos do formulário podem usar para notificar a caixa de diálogo sobre eventos acontecendo a aqueles elementos. Portanto, nosso botão enviar deve agora ser capaz de trabalhar com qualquer caixa de diálogo que implemente aquela interface.

Dessa forma, o padrão Mediator permite que você encapsule uma complexa rede de relações entre vários objetos em apenas um objeto mediador. Quanto menos dependências uma classe tenha, mais fácil essa classe se torna para se modificar, estender, ou reutilizar.

## Aplicabilidade
- Utilize o padrão Mediator quando é difícil mudar algumas das classes porque elas estão firmemente acopladas a várias outras classes. O padrão lhe permite extrair todas as relações entre classes para uma classe separada, isolando quaisquer mudanças para um componente específico do resto dos componentes.
- Utilize o padrão quando você não pode reutilizar um componente em um programa diferente porque ele é muito dependente de outros componentes. Após você aplicar o Mediator, componentes individuais se tornam alheios aos outros componentes. Eles ainda podem se comunicar entre si, mas de forma indireta, através do objeto mediador. Para reutilizar um componente em uma aplicação diferente, você precisa fornecer a ele uma nova classe mediadora.
- Utilize o Mediator quando você se encontrar criando um monte de subclasses para componentes apenas para reutilizar algum comportamento básico em vários contextos. Como todas as relações entre componentes estão contidas dentro do mediador, é fácil definir novas maneiras para esses componentes colaborarem introduzindo novas classes mediadoras, sem ter que mudar os próprios componentes.

## Prós e contras
 - Prós
    - Princípio de responsabilidade única. Você pode extrair as comunicações entre vários componentes para um único lugar, tornando as de mais fácil entendimento e manutenção.
    - Princípio aberto/fechado. Você pode introduzir novos mediadores sem ter que mudar os próprios componentes.
    - Você pode reduzir o acoplamento entre os vários componentes de um programa.
    - Você pode reutilizar componentes individuais mais facilmente.
 - Contra
    - Com o tempo um mediador pode evoluir para um Objeto Deus.

## Relação com outros padrões
O Chain of Responsibility, Command, Mediator e Observer abrangem várias maneiras de se conectar remetentes e destinatários de pedidos:
 - O Chain of Responsibility passa um pedido sequencialmente ao longo de um corrente dinâmica de potenciais destinatários até que um deles atua no pedido.
 - O Command estabelece conexões unidirecionais entre remetentes e destinatários.
 - O Mediator elimina as conexões diretas entre remetentes e destinatários, forçando-os a se comunicar indiretamente através de um objeto mediador.
 - O Observer permite que destinatários inscrevam-se ou cancelem sua inscrição dinamicamente para receber pedidos.

O Facade e o Mediator têm trabalhos parecidos: eles tentam organizar uma colaboração entre classes firmemente acopladas.
 - O Facade define uma interface simplificada para um subsistema de objetos, mas ele não introduz qualquer nova funcionalidade. O próprio subsistema não está ciente da fachada. Objetos dentro do subsistema podem se comunicar diretamente.
 - O Mediator centraliza a comunicação entre componentes do sistema. Os componentes só sabem do objeto mediador e não se comunicam diretamente.

O Mediator centraliza a comunicação entre componentes do sistema. Os componentes só sabem do objeto mediador e não se comunicam diretamente.
 - O objetivo primário do Mediator é eliminar dependências múltiplas entre um conjunto de componentes do sistema. Ao invés disso, esses componentes se tornam dependentes de um único objeto mediador. O objetivo do Observer é estabelecer comunicações de uma via dinâmicas entre objetos, onde alguns deles agem como subordinados de outros.
 - Existe uma implementação popular do padrão Mediator que depende do Observer. O objeto mediador faz o papel de um publicador, e os componentes agem como assinantes que inscrevem-se ou removem a inscrição aos eventos do mediador. Quando o Mediator é implementado dessa forma, ele pode parecer muito similar ao Observer.
 - Quando você está confuso, lembre-se que você pode implementar o padrão Mediator de outras maneiras. Por exemplo, você pode ligar permanentemente todos os componentes ao mesmo objeto mediador. Essa implementação não se parece com o Observer mas ainda irá ser uma instância do padrão Mediator.
 - Agora imagine um programa onde todos os componentes se tornaram publicadores permitindo conexões dinâmicas entre si. Não haverá um objeto mediador centralizado, somente um conjunto distribuído de observadores.