# akka-http
Com a ajuda dos modelos Actor &amp; Stream da Akka, aprenderemos como configurar a Akka para criar uma API HTTP que fornece operações CRUD básicas

## Criando um ator
Como exemplo, construiremos uma API HTTP que nos permite gerenciar os recursos do usuário. A API suportará duas operações:

- criando um novo usuário
- carregando um usuário existente

## Basicamente, estamos estendendo a classe AbstractActor e implementando seu método createReceive()

Em createReceive(), estamos mapeando tipos de mensagens de entrada para métodos que manipulam mensagens do respectivo tipo.

Os tipos de mensagem são classes simples de contêiner serializáveis com alguns campos que descrevem uma determinada operação. GetUserMessage e tem um único campo userId para identificar o usuário a ser carregado. CreateUserMessage contém um objeto User com os dados do usuário de que precisamos para criar um novo usuário.

Mais tarde, veremos como traduzir as solicitações HTTP de entrada nessas mensagens.

Por fim, delegamos todas as mensagens a uma instância de UserService, que fornece a lógica de negócios necessária para gerenciar objetos de usuário persistentes.

Além disso, observe o método props(). Embora o método props() não seja necessário para estender AbstractActor, ele será útil posteriormente ao criar o ActorSystem.

## Definindo Rotas HTTP
Tendo um ator que faz o trabalho real para nós, tudo o que nos resta fazer é fornecer uma API HTTP que delega as solicitações HTTP de entrada para nosso ator.

Akka usa o conceito de rotas para descrever uma API HTTP. Para cada operação, precisamos de uma rota.

Para criar um servidor HTTP, estendemos a classe de framework HttpApp e implementamos o método de rotas

##

Agora, há uma boa quantidade de boilerplate aqui, mas observe que seguimos o mesmo padrão de antes das operações de mapeamento, desta vez como rotas. Vamos decompô-lo um pouco.

Em getUser(), simplesmente envolvemos o ID de usuário recebido em uma mensagem do tipo GetUserMessage e encaminhamos essa mensagem para nosso userActor.

Depois que o ator processa a mensagem, o manipulador onSuccess é chamado, no qual concluímos a solicitação HTTP enviando uma resposta com um determinado status HTTP e um determinado corpo JSON. Usamos o Jackson marshaller para serializar a resposta dada pelo ator em uma string JSON.

Em postUser(), fazemos as coisas de maneira um pouco diferente, já que esperamos um corpo JSON na solicitação HTTP. Usamos o método entity() para mapear o corpo JSON de entrada em um objeto User antes de envolvê-lo em um CreateUserMessage e passá-lo para nosso ator. Novamente, usamos Jackson para mapear entre Java e JSON e vice-versa.

Como o HttpApp espera que forneçamos um único objeto Route, combinamos ambas as rotas em uma única dentro do método de rotas. Aqui, usamos a diretiva de caminho para finalmente fornecer o caminho de URL no qual nossa API deve estar disponível.

Vinculamos a rota fornecida por postUser() ao caminho / usuários. Se a solicitação de entrada não for uma solicitação POST, a Akka irá automaticamente para o branch orElse e esperará que o caminho seja / users / <id> e o método HTTP seja GET.
  
  ##
  Se o método HTTP for GET, a solicitação será encaminhada para a rota getUser (). Se o usuário não existir, a Akka retornará o status HTTP 404 (não encontrado). Se o método não for POST nem GET, a Akka retornará o status HTTP 405 (Método não permitido)
  
  
