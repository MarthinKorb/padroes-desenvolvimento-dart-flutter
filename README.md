# Padrões de Desenvolvimento - Dart/Flutter ![dart](https://skills.thijs.gg/icons?i=dart&theme=light)

Neste documento, abordaremos a estrutura básica de organização das pastas e arquivos, além das principais práticas empregadas no desenvolvimento dos softwares com o framework <a href="https://flutter.dev/" > **_Flutter_**</a>.

## Estrutura do Projeto

Trabalhamos com uma estrutura de pastas que visa a organização e separação das responsabilidades do projeto.

Abaixo, segue a estrutura básica de pastas do projeto utilizando o módulo Cliente:

<p align="center" border-radius="4px">
<img src="imagens\estrutura_projeto.png" width=30%">

<a href="https://medium.com/luizalabs/descomplicando-a-clean-architecture-cf4dfc4a1ac6" target="_blank">_ver mais sobre clean architecture aqui_...</a>

## Arquitetura do projeto

Para começarmos a falar sobre a organização do projeto, podemos falar um pouco dos conceitos das camadas da **_Clean Architecture_**.

<p align="center" border-radius="4px">
<img src="imagens\clean-architecture.png" width="60%">

Na estrutura dos nossos projetos dart/flutter, utilizamos as seguintes camadas/pastas:

- <a href="#domain">Domain</a>
- <a href="#application">Application</a>
- <a href="#infra">Infra</a>
- <a href="#injecao_dependencia">Injecao_Dependencia</a>

Com estas camadas, começamos a modelar a estrutura da nossa aplicação.

<div id="domain"></div>

## <image src="imagens/pasta.png" width="25"> Domain

O **domain** é onde ficam as entidades e as interfaces dos **_repositories_** de acesso aos dados da entidade.
Também no domain podemos ter uma pasta **_errors_**, onde criamos as **exceptions** padrões para a entidade.

Essa camada, se torna uma pasta em nosso projeto. Ela geralmente tem mais algumas pastas dentro dela. São elas:

- models
- repositories
- errors

#### <image src="imagens/pasta.png" width="20"> Models

Contém as classes que representam os modelos de dados das nossas entidades.

Exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
/// Classe que representa a entidade [Cliente]
class Cliente {
    int? idCliente;
    String? nome;

    const Cliente({
        this.idCliente,
        this.nome,
    });
}
```

##

#### <image src="imagens/pasta.png" width="20"> Repositories

Contém as interfaces(abstract class) que representam repositórios de acesso aos dados das entidades. Por exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
i_cliente_repository.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
abstract class IClienteRepository {
    Future<void> salvar(Cliente cliente);
    Future<Cliente> buscaPorId(int idCliente);
}
```

Esta classe deve ser uma classe abstrata(_abstract class_). Ela funcionará como uma interface de um repository de acesso aos dados da entidade.

##

#### <image src="imagens/pasta.png" width="20"> Errors

Contém as classes de exceptions relacionadas à entidade.

Exemplo:

Arquivo:

```
cliente_exceptions.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
class ClienteExceptions extends Exception {
    ClienteExceptions({String? message}) : super(this.message);
}
```

---

<div id="application"></div>

## <image src="imagens/pasta.png" width="25"> Application

O **application** é onde ficam os serviços que contém as regras de negócio da entidade.

Essa camada, se torna uma pasta em nosso projeto. Ela geralmente tem um arquivo e uma pasta dentro dela, são eles:

- cliente_facade.dart
- impl

A classe **_ClienteFacade_** deve ser uma classe abstrata(_abstract class_). Ela funcionará como uma interface de um serviço da entidade.

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_facade.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
/// Interface de serviço da entidade [Cliente]
abstract class ClienteFacade {
    /// Método responsável por salvar os dados do cliente
    Future<void> salvar(Cliente cliente);
}
```

Dentro da pasta **_impl_** criamos a classe que implementa a classe abstrata **_ClienteFacade_**. O nome padrão para esta classe seria **_ClienteService_**.

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_service.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
/// Classe concreta de serviço da entidade cliente
class ClienteService implements ClienteFacade {
    /// Abstração de repositório de acesso aos dados da entidade [cliente]
    final IClienteRepository clienteRepository;
    /// Construtor
    const ClienteService({required this.clienteRepository});

    /// Método responsável por salvar os dados do cliente
    @override
    Future<void> salvar(Cliente cliente) async {
        // Implementação de validações do cliente
        if(cliente.nome.isEmpty) {
            throw ClienteException('O nome do cliente não pode ser vazio.');
        }
        // Chamar o repository para salvamento dos dados do cliente
        await clienteRepository.salvar(cliente);
    }
}
```

---

<div id="infra"></div>

### <image src="imagens/pasta.png" width="25"> Infra

A infra é onde ficam os repositórios concretos de acesso aos dados da entidade e também os componentes que compõe as telas da aplicação(pages e widgets).

Essa camada, se torna uma pasta em nosso projeto. Ela geralmente tem duas pastas dentro dela, são elas:

- repositories
- ui

#### <image src="imagens/pasta.png" width="20"> Repositories

Contém as implementações concretas dos repositories que ficam lá no domain.

Criamos uma classe **_ClienteRepositorySqfLite_** que implementará a interface _`IClienteRepository`_ que está no <a href="#domain">**_domain_**.</a>

Exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_repository_sqflite.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
/// Repositório concreto de acesso aos dados da entidade [Cliente] no banco SqfLite
class ClienteRepositorySqfLite implements IClienteRepository {
    @override
    Future<void> salvar(Cliente cliente) {
        // Implementação da lógica de salvamento dos dados
    }

    @override
    Future<Cliente> buscaPorId(int idCliente) {
        // Implementação da lógica de busca do cliente
        // no banco SqfLite
    }
}
```

##

#### <image src="imagens/pasta.png" width="20"> Ui

Contém as classes relacionadas a interface gráfica da aplicação. Nesta pasta, ficarão nossas pages, widgets, gerenciadores de estado(providers), mixins e tudo mais que possa estar relacionado à _view_.

##

##### <image src="imagens/pasta.png" width="20"> Pages

Esta pasta conterá as nossas páginas do aplicativo relacionadas a entidade cliente.

Exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_form_page.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

A classe _ClienteFormPage_ representa a página que contém o formulário de cliente, porém a mesma não tem toda a lógica de montagem do formulário. Isso se dá, por que isolamos essa lógica para um outro widget(_ClienteFormWidget_) que será passado no body da ClienteFormPage.

```
/// Classe que representa a página de formulário de [Cliente]
class ClienteFormPage extends StatelessWidget {
  /// Objeto cliente que pode vir em escopo de alteração
  final Cliente? cliente;

  /// Construtor
  const ClienteFormPage({this.cliente, Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Formulário de cliente')),
      body: SingleChildScrollView(
        child: ClienteFormWidget(cliente: cliente)
      ),
    );
  }
}
```

##

##### <image src="imagens/pasta.png" width="20"> Widgets

Esta pasta contém componentes menores(widgets) que podem compor uma página(page).

Abaixo, temos o **_ClienteFormWidget_** que isola a parte de formulário de inseção/edição do cliente, tornando-o reutilizável caso seja necessário. Também, ajuda na organização da página em que é chamado.

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_form_widget.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
// Classe que representa o formulário de [Cliente]
class ClienteFormWidget extends StatelessWidget {
    /// Objeto cliente que pode vir em escopo de alteração
  final Cliente cliente;

  const ClienteFormWidget({this.cliente, super.key});

  @override
  Widget build(BuildContext context) {
    return Form(
      child: Column(
        children: [
          TextFormField(
            decoration: const InputDecoration(
              label: Text('Nome'),
            ),
          ),
        ],
      ),
    );
  }
}
```

---

<div id="injecao_dependencia"></div>

### <image src="imagens/pasta.png" width="25"> Injecao_Dependencia

Esta pasta contém um arquivo que basicamente possui o registro das instâncias das classes(repositories, services e etc) serem contruídas no sistema de <a href="https://www.devmedia.com.br/padrao-de-injecao-de-dependencia/18506" target="_blank">**_injeção de dependências_**</a>.

Criamos um arquivo com o nome da entidade do módulo + \_binds.dart.

Exemplo:

```
cliente_binds.dart
```

Este arquivo deve ser importado no arquivo principal do registro de instâncias no sistema de injeção de dependências(**_main_module.dart_**).

Este módulo é muito importante, pois ele gerencia as instâncias concretas das classes(respositories, services e etc) que teremos acesso em todo o sistema, nos ajudando a não ter um alto acomplamento entre as classes, visto que sempre injetamos como dependência de uma classe uma interface.

Exemplo:

```
class ClienteService implements ClienteFacade {
  /// Injetando uma interface na classe [ClienteService]
  /// Quando essa classe for instanciada no sistema de injeção de dependências,
  ///
  final IClienteRepository clienteRepository;

  const ClienteService({
    required this.clienteRepository,
  });
}
```

---

##

## Nomenclaturas e padrões de escrita de código

Temos alguns padrões de nomenclaturas de classes, métodos, variáveis e etc. Seguir esses padrões facilita o trabalho em equipe, uma vez que o time conhece e aplica esses padrões.
Os padrões ajudam a organizar o código, deixando-o **_limpo_** e de fácil leitura para todos.

### Nomes de classes

Temos algumas variações de classes no sistema.

- entidades(_models_)
- interfaces de services de entidades(_facades_)
- classes de implementação de services de entidades(_services_)
- interfaces de repositories de acesso aos dados da entidade
- classes de implementação de repositórios de acesso aos dados da entidade

#### Nomes de entidades

Geralmente nomeamos as classes que representam as entidades com o nome da tabela no banco de dados.
No caso da tabela **Cliente**, o nome da nossa classe de modelo seria **Cliente**.
Criamos um arquivo `cliente.dart` na pasta _src/cliente/domain/models_.

```
cliente.dart
```

Seguindo a criação da classe, criamos a classe da seguinte maneira:

```
class Cliente {
  final int idCliente;
  final String nome;

  const Cliente({
    required this.idCliente,
    required this.nome,
  });
}
```

Por termos algumas abstrações no nosso projeto, as vezes o modelo de criação da classe da entidade pode variar. Sempre é possível se basear por outras entidades já criadas.

##

#### Interfaces

##

##### Interface de Repositories

Para as interfaces de classes de repositories, utilizamos no nome o prefixo "i". Utilizamos esse padrão para que fique fácil o entendimento de que se trata de uma interface de um repository.
Para criação do arquivo de interface de repositório para a entidade cliente, utilizaremos o seguinte padrão:

```
i_cliente_repository.dart
```

O arquivo de interface de repository será criado em `src/cliente/domain/repositories/i_cliente_repository.dart`

Seguindo a criação da classe, por ser uma interface, criamos a classe da seguinte maneira:

```
abstract class IClienteRepository {
  Future<void> salvar(Cliente cliente);
}
```

Esta classe conterá apenas as assinaturas dos métodos que deverão ser implementados pelas classes que implementarem esta interface.

##

##### Interface de Services

Para as interfaces de classes de services, utilizamos no nome _**"facade"**_. Utilizamos esse padrão para que fique fácil o entendimento de que se trata de uma interface de uma classe de serviço.
Para criação do arquivo de interface de service para a entidade cliente, utilizaremos o seguinte padrão:

```
cliente_facade.dart
```

O arquivo de interface de repository será criado em `src/cliente/application/cliente_facade.dart`

Seguindo a criação da classe, por ser uma interface, criamos a classe da seguinte maneira:

```
abstract class ClienteFacade {
  Future<void> salvar(Cliente cliente);
}
```

---

##

### Implementações de interfaces de _Repositories_ e _Facades_

##

#### Implementação de **_Facades_**

Precisamos criar uma nova classe com o nome da entidade + "Service" na pasta `src/cliente/persistence/impl/cliente_service.dart` e usar o _implements_ para o _Facade_ que queremos implementar.

Exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_service.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
class ClienteService implements ClienteFacade {
  @override
  Future<void> salvar(Cliente cliente) {
    /// Regras de negócio de salvamento do cliente...
    /// Chamada do repository que fará a persistência dos dados...
  }
}
```

Será necessário fazer o **_@override_** dos métodos contidos na interface _facade_.

#### Implementação de **_Repositories_**

Precisamos criar uma nova classe com o nome da entidade + "Repository" + "Nome do plugin/fonte de dados" e usar o implements para o IClienteRepository que queremos implementar.

Exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_repository_sqflite.dart
```

<image src="imagens/arquivo.png" width="18"> Classe:

```
class ClienteRepositorySqfLite implements IClienteRepository {
    @override
    Future<void> salvar(Cliente cliente) {
      /// Conexão com banco de dados do SqfLite
      /// Execução da persistência dos dados no banco de dados
    }
}
```

---
