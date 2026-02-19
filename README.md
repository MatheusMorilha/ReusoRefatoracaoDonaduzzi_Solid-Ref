# ReusoRefatoracaoDonaduzzi_Solid-Ref

## Objetivo

Este projeto foi refatorado com o objetivo de aplicar de forma prática os princípios SOLID, promovendo melhor organização arquitetural, redução de acoplamento, maior coesão entre componentes e melhor capacidade de extensão do sistema.

A refatoração concentrou-se na separação adequada das responsabilidades entre as camadas da aplicação e na eliminação de dependências diretas entre componentes de alto e baixo nível.

## Arquitetura

Após a refatoração, a aplicação passou a seguir uma arquitetura em camadas bem definida:

Controller → Service → Repository → NotificationService → EmailService

Cada camada passou a exercer uma responsabilidade específica e claramente delimitada.

O Controller é responsável exclusivamente por receber requisições HTTP e retornar respostas apropriadas.

O Service concentra as regras de negócio e a orquestração das operações.

O Repository é responsável pela persistência de dados.

O NotificationService trata o envio de notificações.

O EmailService representa a abstração do mecanismo de envio de e-mails.

## Aplicação dos Princípios SOLID

### Single Responsibility Principle (SRP)

Antes da refatoração, o Controller acumulava múltiplas responsabilidades, incluindo validações, persistência, regras de negócio e envio de e-mails. Após a refatoração, cada responsabilidade foi movida para sua respectiva camada.

O Controller passou a apenas delegar chamadas ao Service. O Service concentra as regras de negócio. O Repository trata exclusivamente da persistência. O envio de notificações foi isolado em um serviço próprio.

Essa reorganização reduziu o acoplamento e aumentou a coesão entre os componentes.

### Open/Closed Principle (OCP)

Foi criada a interface `RegraUsuario`, permitindo que cada tipo de usuário possua sua própria implementação de regra de negócio.

O `GerenciadorUsuarioService` passou a depender de uma coleção de implementações da interface `RegraUsuario`, permitindo que novos tipos de usuário sejam adicionados sem necessidade de modificação da classe principal de serviço.

Dessa forma, o sistema permanece fechado para modificação e aberto para extensão.

### Liskov Substitution Principle (LSP)

Foi identificado que a hierarquia de herança poderia violar o contrato da superclasse quando métodos lançavam exceções em subclasses.

A refatoração buscou eliminar comportamentos que quebrassem o contrato implícito da superclasse, evitando lançar exceções inesperadas e, quando necessário, substituindo herança por composição.

Com isso, subclasses podem substituir a superclasse sem comprometer o funcionamento do sistema.

### Interface Segregation Principle (ISP)

O repositório original concentrava múltiplas responsabilidades. Para aplicar o princípio da segregação de interfaces, foram criadas interfaces específicas:

- UsuarioCrudRepository
- UsuarioFiltroRepository
- UsuarioRelatorioRepository

Cada interface passou a representar uma responsabilidade distinta, evitando que classes dependam de métodos que não utilizam.

### Dependency Inversion Principle (DIP)

Foi criada a interface `EmailService`, representando uma abstração para envio de e-mails.

A implementação concreta `SpringEmailService` passou a depender de `JavaMailSender`, enquanto as camadas superiores dependem apenas da abstração `EmailService`.

Essa inversão de dependência reduz o acoplamento entre regras de negócio e detalhes de infraestrutura.

## Estrutura do Projeto

O projeto foi reorganizado em pacotes coerentes com suas responsabilidades:

- controller
- service
- service.regra
- repository
- model
- dto

Essa organização reforça a separação de responsabilidades e facilita a manutenção e evolução da aplicação.

A arquitetura atual facilita a inclusão de novas regras de negócio, novos tipos de usuário e novas formas de notificação sem necessidade de alterações significativas nas classes já existentes.
