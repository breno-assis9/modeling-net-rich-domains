# 🌍 Modeling .NET Rich Domains

## 📜 Descrição

Este repositório serve como uma demonstração de como aplicar os princípios de **Domain-Driven Design (DDD)** para modelar **domínios ricos** em **.NET 6/8**. A ideia central do DDD é criar software que seja altamente focado no **domínio de negócios**, e no qual a **lógica de negócios** seja encapsulada diretamente dentro do modelo de domínio. Em vez de tratar os objetos de domínio como simples containers de dados (objetos anêmicos), **DDD** promove a criação de **entidades**, **agregados** e **objetos de valor** que encapsulam comportamentos e garantem que todas as regras de negócio sejam respeitadas.

Além disso, DDD inclui padrões arquiteturais e práticas para **modelagem de domínios complexos**, como **agregados**, **repositórios**, **fábricas**, **serviços de domínio**, **eventos de domínio**, e **CQRS**, entre outros.

Este repositório irá explorar como essas práticas podem ser aplicadas em **.NET 6/8**, utilizando bibliotecas como **Entity Framework Core**, **FluentValidation**, e **MediatR**.

---

## 🚀 Fundamentos do DDD

O **Domain-Driven Design** é composto por uma série de padrões e práticas que ajudam a construir sistemas que são profundamente conectados com o domínio de negócios. Vamos explorar os principais conceitos do DDD.

### 1. **Entidades** 🧑‍💻

As **entidades** são objetos do domínio que possuem uma identidade única. Elas podem ter atributos que mudam ao longo do tempo, mas sua identidade persistente ao longo de sua existência é o que as distingue.

- **Características**:
  - **Identidade única**: A identidade de uma entidade é o que a torna única, independente de seus atributos.
  - **Mudança de estado**: As entidades podem mudar de estado, mas sempre mantendo sua identidade.
  - **Encapsulação de comportamentos**: Além de dados, as entidades também contêm comportamentos específicos do domínio.

Exemplo: Uma `ContaBancaria` é uma entidade, onde o saldo pode mudar ao longo do tempo, mas a identidade da conta persiste durante toda a sua vida útil.

### 2. **Objetos de Valor (Value Objects)** 🪙

Os **objetos de valor** são definidos pelos seus atributos, sem uma identidade única. Ao contrário das entidades, dois objetos de valor com os mesmos dados são considerados idênticos.

- **Características**:
  - **Imutabilidade**: Uma vez criados, seus atributos não podem ser alterados.
  - **Definidos por seus atributos**: Sua identidade é determinada pelos valores dos seus atributos.
  - **Substituíveis**: Objetos de valor com os mesmos atributos são intercambiáveis.

Exemplo: Um `Endereco` ou `Telefone` são objetos de valor, pois sua identidade não importa; o que importa são seus dados.

### 3. **Agregados** 🏗️

Um **agregado** é um conjunto de entidades e objetos de valor que formam uma unidade de consistência. O **agregado raiz** é a entidade principal que controla o acesso às outras entidades e objetos de valor do agregado.

- **Características**:
  - **Consistência transacional**: Operações dentro de um agregado devem ser atômicas e garantir consistência.
  - **Agregado raiz**: Apenas a raiz do agregado pode ser acessada diretamente. Outras entidades dentro do agregado devem ser manipuladas indiretamente, através da raiz.
  - **Unidade de consistência**: Qualquer alteração em um agregado precisa garantir que todas as regras de consistência do domínio sejam respeitadas.

Exemplo: Um `Pedido` pode ser um agregado que contém múltiplos `ItensPedido`. O `Pedido` garante que todos os itens estejam corretos antes de finalizar a transação.

### 4. **Repositórios** 📚

Os **repositórios** são usados para abstrair o acesso e a persistência de objetos de domínio. Eles permitem que as entidades e agregados sejam armazenados e recuperados sem que o código de aplicação precise conhecer os detalhes de implementação do banco de dados.

- **Características**:
  - **Abstração de persistência**: Eles escondem a complexidade da camada de persistência e fornecem uma interface simples para o domínio.
  - **Operações CRUD**: Repositórios facilitam operações como salvar, carregar e excluir agregados.
  - **Unidade de Consistência**: Os repositórios devem garantir que, ao recuperar um agregado, ele esteja em um estado consistente.

Exemplo: Um `PedidoRepository` pode ser responsável por salvar e recuperar `Pedidos` e seus `ItensPedido` associados.

### 5. **Serviços de Domínio** ⚙️

**Serviços de domínio** encapsulam lógica de negócio que não se encaixa diretamente em entidades ou objetos de valor. Eles são usados para representar operações complexas que envolvem múltiplos objetos de domínio.

- **Características**:
  - **Lógica que não pertence a entidades**: Quando uma operação afeta múltiplas entidades ou não faz sentido ser colocada em uma única entidade, ela pode ser movida para um serviço de domínio.
  - **Abstração**: O serviço abstrai a complexidade da lógica de negócio, proporcionando uma interface simples para a aplicação.

Exemplo: Um `ServicoDePagamento` pode ser um serviço de domínio que gerencia o processo de pagamento, interagindo com várias entidades como `Pedido`, `Cliente`, e serviços externos como gateways de pagamento.

### 6. **Fábricas** 🏭

As **fábricas** são responsáveis pela criação de objetos complexos. Elas abstraem a complexidade de construir agregados e entidades, centralizando a lógica de criação e garantindo que todas as invariantes do domínio sejam respeitadas no momento da criação.

- **Características**:
  - **Abstração de criação**: A fábrica isola a lógica de criação complexa de objetos de domínio.
  - **Controle de consistência**: Elas garantem que, durante a criação, todas as regras e invariantes do domínio sejam aplicadas.

Exemplo: Uma `FábricaDePedido` pode ser responsável por criar um novo `Pedido`, garantindo que todos os itens e valores estejam corretos, e criando o pedido de forma consistente.

### 7. **Eventos de Domínio** 📡

Os **eventos de domínio** representam algo que aconteceu no domínio, e são usados para comunicar alterações ou ações que podem ser importantes para outros contextos ou partes do sistema. Eles são uma maneira de capturar e disseminar mudanças no estado de um sistema.

- **Características**:
  - **Captura de alterações**: Eventos de domínio capturam mudanças significativas que aconteceram no domínio.
  - **Comunicação assíncrona**: Eles podem ser usados para acionar processos ou reações em outros sistemas ou partes do sistema de forma assíncrona.

Exemplo: Um `PagamentoRealizadoEvent` pode ser disparado quando um pagamento é concluído, informando que o pagamento foi processado com sucesso.

### 8. **CQRS (Command Query Responsibility Segregation)** 🔄

O padrão **CQRS** separa a leitura e a escrita de dados em sistemas. Em vez de usar o mesmo modelo para operações de leitura e escrita, o CQRS sugere o uso de modelos separados para comandos (escrita) e consultas (leitura).

- **Características**:
  - **Separação de responsabilidades**: Leitura e escrita são tratadas por modelos e interfaces diferentes.
  - **Escalabilidade**: Com a separação de leitura e escrita, pode-se otimizar cada um deles de maneira independente.
  - **Desempenho**: Operações de leitura podem ser otimizadas para consultas rápidas, enquanto as operações de escrita podem ser mais focadas em garantir consistência.

Exemplo: O modelo de leitura pode ser simplificado para apenas retornar dados necessários para exibição, enquanto o modelo de escrita pode ser mais complexo, lidando com a lógica de negócios e validações.

### 9. **Anticorrupção Layer (ACL)** 💡

A **Camada de Anticorrupção (ACL)** é um padrão que visa proteger o modelo de domínio de influências externas indesejadas, como modelos de dados ou lógicas de sistemas legados.

- **Características**:
  - **Isolamento**: A camada de anticorrupção isola o modelo de domínio das mudanças externas.
  - **Adaptação**: Quando a comunicação com sistemas externos é necessária, ela deve ser feita através de uma camada adaptadora que mapeia entre os modelos externos e o modelo de domínio, evitando a contaminação do domínio.

Exemplo: Se um sistema legado utiliza uma estrutura de dados diferente, a camada de anticorrupção pode criar adaptadores que transformam esses dados em objetos do domínio que fazem sentido para a aplicação.

---

## 🛠️ Tecnologias Utilizadas

Este projeto utiliza várias tecnologias do **.NET 6/8** para implementar as práticas de **Domain-Driven Design (DDD)**:

- **.NET 6/8**: A plataforma base para construir a aplicação, utilizando **C#**.
- **Entity Framework Core**: Para persistência de dados, mapeando entidades do domínio para tabelas de banco de dados.
- **FluentValidation**: Para validar entidades e objetos de valor, garantindo a consistência do domínio.
- **MediatR**: Para implementação do padrão **CQRS**, separando os comandos e consultas.
- **XUnit/NUnit**: Frameworks para testes unitários.
- **AutoMapper**: Para mapear objetos entre camadas (por exemplo, entre DTOs e entidades do domínio).

---

## 🧑‍💻 Requisitos

Para rodar o projeto, você precisa:

1. Instalar o **.NET 6/8 SDK**.
2. Instalar um editor como **Visual Studio** ou **Visual Studio Code**.

---

## 📝 Licença

Este projeto está licenciado sob a **MIT License** - veja o arquivo [LICENSE](./LICENSE).

---

### Conclusão

Agora, você tem uma visão detalhada dos principais componentes do **Domain-Driven Design (DDD)** e como implementá-los em um sistema **.NET 6/8** com **domínios ricos**. Aplicando essas práticas, o sistema será mais robusto, fácil de manter e expandir conforme novas regras de negócios surgem, mantendo a integridade do modelo de domínio.
