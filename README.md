# Padrões de Desenvolvimento Rech - Dart/Flutter ![dart](https://skills.thijs.gg/icons?i=dart&theme=light)

Neste documento, abordaremos a estrutura básica de organização das pastas e arquivos, além das principais práticas empregadas no desenvolvimento dos softwares com o framework <a href="https://flutter.dev/" > **_Flutter_**</a> na Rech Informática.

## Estrutura do Projeto

Trabalhamos com uma estrutura de pastas que visa a organização e separação das responsabilidades do projeto.

Abaixo, segue a estrutura básica de pastas do projeto utilizando o módulo Cliente:

<p align="center" border-radius="4px">
<img src="imagens\estrutura_projeto.png" width=30%">

[_ver mais sobre clean architecture aqui_...](https://medium.com/luizalabs/descomplicando-a-clean-architecture-cf4dfc4a1ac6)

## Arquitetura do projeto

Para começarmos a falar sobre a organização do projeto, podemos falar um pouco dos conceitos das camadas da **_Clean Architecture_**.

<p align="center" border-radius="4px">
<img src="imagens\clean-architecture.png" width="60%">

Na estrutura dos nossos projetos dart/flutter, utilizamos as seguintes camadas/pastas:

- Domain
- Application
- Infra

Com estas camadas, começamos a modelar a estrutura da nossa aplicação.

## <image src="imagens/pasta.png" width="25"> Domain

<div id="domain"></div>

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

Classe:

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

Classe:

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

Classe:

```
class ClienteExceptions extends Exception {
    ClienteExceptions({String? message}) : super(this.message);
}
```

---

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

Classe:

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

Classe:

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

Classe:

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

#### <image src="imagens/pasta.png" width="20"> Ui

Contém as classes relacionadas a interface gráfica da aplicação. Nesta pasta, ficarão nossas pages, widgets, gerenciadores de estado(providers), mixins e tudo mais que possa estar relacionado à _view_.

##### <image src="imagens/pasta.png" width="20"> Pages

Esta pasta conterá as nossas páginas do aplicativo relacionadas a entidade cliente.

Exemplo:

<image src="imagens/arquivo.png" width="18"> Arquivo:

```
cliente_form_page.dart
```

Classe:

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

Classe:

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
