# Exercício E1.12

[Link do repositório escolhido](https://github.com/rubenlagus/TelegramBots)

## *Dependências do projeto*

* `lombok`: Lombok é uma biblioteca do Java que ajuda a reduzir a verbosidade do código-fonte, automatizando a geração de métodos padrão, como getters e setters, construtores, etc.
* `junit-jupiter-api`: JUnit é um framework de testes para Java. O JUnit Jupiter API é a API de teste usada no JUnit 5, que suporta novos recursos e extensões para testes.
* `mockito-core`: Mockito é uma biblioteca de teste para Java que facilita a criação de objetos mock (simulados) para testes unitários.
* `mockito-junit-jupiter`: Integração do Mockito com o JUnit 5 para facilitar o uso de mocks em testes unitários.
* `jackson-annotations`: Jackson é uma biblioteca para processamento JSON em Java. As Jackson Annotations são usadas para configurar o mapeamento entre objetos Java e JSON.
* `guava`: Guava é uma biblioteca da Google que fornece várias classes e utilitários úteis, como coleções imutáveis, utilitários de concorrência, entre outros.
* `jackson-module-jaxb-annotations`: Um módulo do Jackson que adiciona suporte para anotações JAXB (Java Architecture for XML Binding) ao processamento JSON.
* `jackson-jaxrs-json-provider`: Um provedor JSON para JAX-RS (Java API for RESTful Web Services) baseado na biblioteca Jackson.
* `jackson-databind`: Biblioteca Jackson usada para mapear objetos Java para JSON e vice-versa.
* `slf4j-api`: SLF4J (Simple Logging Facade for Java) é uma fachada de registro de eventos para Java. A API fornece uma abstração sobre vários frameworks de logging.
* `commons-lang3`: Apache Commons Lang é uma biblioteca que fornece funcionalidades adicionais para o pacote java.lang, incluindo manipulação de strings, reflexão, validação, entre outros.
* `jakarta.annotation-api`: Jakarta Annotations (anteriormente parte do Java EE, agora Jakarta EE) é uma API para processamento de anotações em tempo de compilação.


## *Puglins configurados*

* `maven-site-plugin`: O Maven Site Plugin é usado para gerar documentação do projeto e relatórios.
* `maven-javadoc-plugin`: O Maven Javadoc Plugin é usado para gerar documentação a partir do código-fonte Java, incluindo Javadoc.
* `nexus-staging-maven-plugin`: O Nexus Staging Maven Plugin é usado para facilitar a implantação (deploy) de artefatos no Nexus Repository.
* `maven-enforcer-plugin`: O Maven Enforcer Plugin é usado para impor regras e políticas durante o processo de construção do Maven, garantindo que o projeto atenda a determinados critérios antes da compilação e implantação.

## *Configurações específicas do projeto*

* `JitPack`: 6.8.0
* `maven central`: 6.8.0


## *Partes importantes do código fonte*


* Esse é um repositório para criar bots no aplicativo Telegram
* Como é um repositorio extenso, é difícil definir qual a parte mais importante de cada diretório, porém vou resaltar partes importantes
* O principal diretorio é o ./telegrambots/src/
* Esse diretório temos a parte do código em si em `main/java/org/telegram/telegrambots` e a parte de testes unitários em `test`

## *Test*

* Existem 6 códigos de teste: 

```java
BotApiMethodHelperFactory
ServerlessWebhookTest
TelegramFileDownloaderTest
TelegramLongPollingBotTest
TestDefaultBotSession
TestRestApi
```
### *BotApiMethodHelperFactory*

* A classe BotApiMethodHelperFactory fornece métodos para criar diferentes instâncias de objetos BotApiMethod, que são usados para interagir com a API do Telegram.
* Cada método nesta fábrica retorna uma instância específica de BotApiMethod configurada com parâmetros específicos. Por exemplo, há métodos para enviar mensagens, responder a consultas inline, editar mensagens, interagir com membros do chat, entre outros. Esses métodos podem ser utilizados nos testes para simular interações do bot com a API do Telegram e verificar se o comportamento do bot está de acordo com o esperado em diferentes situações.

### *ServerlessWebhookTest*

* Essa bateria de testes, chamada ServerlessWebhookTest, visa validar o comportamento do método updateReceived da classe ServerlessWebhook em resposta a diversos tipos de atualizações provenientes do webhook do Telegram. A classe ServerlessWebhook faz parte de um sistema de webhook serverless destinado a bots do Telegram.

### *TelegramFileDownloaderTest*

* Este conjunto de testes, chamado `TelegramFileDownloaderTest`, avalia a funcionalidade do componente `TelegramFileDownloader` no contexto de um bot do Telegram. Esse componente é responsável pelo download assíncrono de arquivos do Telegram, como imagens e documentos, utilizando um cliente HTTP.
* Na configuração inicial, o ambiente de teste utiliza o framework Mockito para simular o comportamento do cliente HTTP e estabelecer um cenário onde uma resposta HTTP com status 200 (sucesso) é retornada, contendo o conteúdo fictício "Some File Content". O `TelegramFileDownloader` é instanciado com o cliente HTTP simulado.
* Os testes individuais abrangem cenários de sucesso, onde um arquivo é baixado sincronamente (`testFileDownload`) ou assincronamente (`testAsyncDownload`). Além disso, há um teste para lidar com exceções (`testDownloadException`), onde uma resposta HTTP com status 500 (erro interno do servidor) é simulada, resultando em uma exceção `TelegramApiException`. Esses testes garantem que o componente de download de arquivos do Telegram se comporte conforme o esperado em diferentes situações.

### *TelegramLongPollingBotTest*

* O teste `TelegramLongPollingBotTest` examina o comportamento da classe `TelegramLongPollingBot`, que é uma implementação da biblioteca de bots do Telegram. O primeiro teste, `testOnUpdateReceived`, verifica se o método `onUpdateReceived` é corretamente chamado para cada atualização recebida. O bot é simulado usando o framework Mockito, e duas atualizações fictícias são enviadas para garantir o comportamento adequado.
* O segundo teste, `testExecutorShutdown`, avalia se o executor associado ao bot é adequadamente encerrado durante o ciclo de vida do bot. Aqui, um bot de teste chamado `TestBot` é criado, e sua sessão é iniciada e posteriormente encerrada. O teste verifica se o método `onClosing` do bot é chamado durante o encerramento da sessão e se o executor associado a ele é devidamente encerrado e terminado.
* A classe `TestBot` é uma subclasse de `TelegramLongPollingBot` e, além de fornecer uma implementação mínima do método `onUpdateReceived` para evitar comportamento padrão, ela expõe o método `getExecutor` para possibilitar a verificação do estado do executor nos testes. Este conjunto de testes visa garantir o correto funcionamento do ciclo de vida e manipulação de atualizações da classe `TelegramLongPollingBot`.

### *TestDefaultBotSession*

* O teste `TestDefaultBotSession` avalia o comportamento da classe `DefaultBotSession`, que gerencia a sessão de um bot Telegram durante operações de long polling. Os testes abordam aspectos como iniciar, parar e manipular atualizações recebidas pelo bot durante a sessão.
* O primeiro conjunto de testes verifica se a sessão é corretamente inicializada e se pode ser iniciada e parada conforme esperado. Garante-se que tentativas incorretas de iniciar ou parar a sessão resultem em exceções apropriadas.
* O segundo conjunto de testes avalia a capacidade da sessão de processar atualizações de forma assíncrona. Utilizando o framework Mockito, o teste simula um bot que recebe atualizações. As atualizações são então fornecidas à sessão em lotes, e verifica-se se o bot as recebe corretamente.
* Finalmente, o teste inclui casos em que são utilizados objetos `BackOff` personalizados para atraso entre tentativas de obtenção de atualizações. Isso destaca a flexibilidade da `DefaultBotSession` para lidar com diferentes estratégias de retentativas em cenários de long polling.

### *TestRestApi*

* Este código Java é um conjunto de testes para uma implementação de uma API REST para bots Telegram usando a biblioteca Jersey. Os testes verificam se os métodos da API respondem corretamente para diferentes tipos de requisições, como enviar mensagens, responder a consultas de callback, editar mensagens, entre outros. A implementação utiliza a classe `JerseyTest` para configurar e realizar os testes, com a classe `FakeWebhook` sendo usada para simular a interação com o webhook do bot. Cada método de teste corresponde a um tipo específico de operação no bot Telegram, como enviar mensagens, editar mensagens, etc. O objetivo é garantir que a implementação da API esteja correta e responda adequadamente aos diferentes tipos de solicitações do Telegram.
