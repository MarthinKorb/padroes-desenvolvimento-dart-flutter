# Padrões de Desenvolvimento Rech - Dart/Flutter

Neste documento, abordaremos as principais metodologias praticadas no desenvolvimento dos softwares
desenvolvidos em Flutter na Rech Informática.

## Estrutura do Projeto

Trabalhamos com uma estrutura de pastas que visa a organização e separação das responsabilidades
do projeto.

Abaixo, segue a estrutura básica de pastas do projeto utilizando o módulo Cliente:

<!-- ```
- src
-- cliente
--- application
------ impl
--------- cliente_service.dart
------ cliente_facade.dart
--- domain
------ models
--------- cliente.dart
------ repositories
--------- i_cliente_repository.dart
--- infra
------ repositories
--------- cliente_repository_sqflite.dart
------ ui
--------- pages
------------ clientes_listagem_page.dart
------------ cliente_form_page.dart
--------- widgets
------------ cliente_form.dart
------------ cliente_tile.dart
------------ cliente_listagem.dart
------ relacionados
--------- cliente_contato
------------ application
--------------- impl
------------------ cliente_contato_service.dart
--------------- cliente_contato_facade.dart
------------ domain
--------------- models
------------------ cliente_contato.dart
--------------- repositories
------------------ i_cliente_contato_repository.dart
------------ infra
--------------- repositories
------------------ cliente_contato_repository_sqflite.dart
--------------- ui
------------------ pages
--------------------- cliente_contato_form_page.dart
------------------ widgets
--------------------- cliente_contato_form_widget.dart
``` -->

<p align="center" border-radius="4px">
<img src="imagens\estrutura_projeto.png" width=30%">

Para entendermos melhor a estrutura apresentada acima, temos que conhecer um pouco mais de **Arquitetura Limpa(_Clean Architecture_)**.
[_Ver mais..._](https://medium.com/luizalabs/descomplicando-a-clean-architecture-cf4dfc4a1ac6)

## Arquitetura do projeto

Para começarmos a falar sobre a organização do projeto, podemos falar um pouco dos conceitos das camadas da **_Clean Architecture_**.

<p align="center" border-radius="4px">
<img src="imagens\clean-architecture.png" width=50%">

Na estrutura dos nossos projetos, utilizamos as seguintes camadas/pastas:

- Application
- Domain
- Infra

Com estas camadas, começamos a modelar a estrutura da nossa aplicação.

### Domain

O domain é onde ficam as entidades e as interfaces dos **_repositories_** de acesso aos dados da entidade.

Essa camada, se torna uma pasta em nosso projeto. Ela geralmente tem mais duas pastas dentro dela. São elas:

- models
- repositories

#### Models

A pasta models contém a classe que representa a entidade. Por exemplo:
