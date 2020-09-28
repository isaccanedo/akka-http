# akka-http
Com a ajuda dos modelos Actor &amp; Stream da Akka, aprenderemos como configurar a Akka para criar uma API HTTP que fornece operações CRUD básicas

## Criando um ator
Como exemplo, construiremos uma API HTTP que nos permite gerenciar os recursos do usuário. A API suportará duas operações:

- criando um novo usuário
- carregando um usuário existente

## Basicamente, estamos estendendo a classe AbstractActor e implementando seu método createReceive ().

Em createReceive (), estamos mapeando tipos de mensagens de entrada para métodos que manipulam mensagens do respectivo tipo.

Os tipos de mensagem são classes simples de contêiner serializáveis com alguns campos que descrevem uma determinada operação. GetUserMessage e tem um único campo userId para identificar o usuário a ser carregado. CreateUserMessage contém um objeto User com os dados do usuário de que precisamos para criar um novo usuário.

Mais tarde, veremos como traduzir as solicitações HTTP de entrada nessas mensagens.

Por fim, delegamos todas as mensagens a uma instância de UserService, que fornece a lógica de negócios necessária para gerenciar objetos de usuário persistentes.

Além disso, observe o método props (). Embora o método props () não seja necessário para estender AbstractActor, ele será útil posteriormente ao criar o ActorSystem.
