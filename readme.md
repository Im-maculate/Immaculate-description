# Project Immaculate

## Descrição do Projeto

O projeto "Immaculate" visa criar uma aplicação que permita verificar a identidade de uma pessoa (exemplo: CPF) com Zero-Knowledge Proof (ZKP). O sistema deve ser leve, escalável e seguir os padrões W3C para identificadores descentralizados (DIDs).

## O que são UTXO e ZKP?
#### UTXO (Unspent Transaction Output)

 - **Definição:** UTXO é um conceito usado em criptomoedas que representa a quantidade de moeda que não foi gasta. Em vez de manter um saldo, cada transação produz saídas que podem ser usadas em transações futuras.
 - **Funcionamento**: Quando uma transação é realizada, o sistema registra as saídas não gastas (UTXOs). Para gastar esses UTXOs, é criada uma nova transação que consome essas saídas como entradas.

#### ZKP (Zero-Knowledge Proof)

 - **Definição:** ZKP é um método criptográfico que permite que uma parte (o provador) prove à outra parte (o verificador) que uma afirmação é verdadeira, sem revelar informações além da veracidade da afirmação.
 - **Funcionamento:** O provador gera uma prova que é verificada sem que o verificador tenha acesso a informações sensíveis. Isso é útil para manter a privacidade em transações e dados.

#### Funcionamento na Hathor

A Hathor Network usa um modelo baseado em UTXO e pode implementar ZKP para garantir a privacidade nas transações. O sistema é projetado para ser leve e escalável, utilizando uma arquitetura de blocos e grafos.

#### Como o UTXO e ZKP funcionam na Hathor:

 - **UTXO:** Em Hathor, cada transação gera saídas que podem ser usadas como entradas em futuras transações. Isso facilita a criação de um registro imutável das transações.
 - **ZKP:** ZKPs podem ser usados para validar que uma transação é válida sem revelar o valor total ou detalhes da transação, aumentando a privacidade.

## Padrão W3C e sua Utilização

O padrão W3C (World Wide Web Consortium) refere-se a uma série de recomendações e padrões que garantem a interoperabilidade na web. Para o contexto de Identificadores Descentralizados (DIDs), o W3C fornece diretrizes sobre como criar e gerenciar identidades digitais.

#### Melhor forma de utilizar o padrão W3C

 - **Implementação de DIDs:** Use os padrões W3C para criar DIDs, que permitem que usuários e dispositivos se identifiquem de maneira segura
 - **Interoperabilidade:** Assegurar que o sistema siga as diretrizes W3C para que possa interagir com outros sistemas que também adotam esses padrões.

## Determinando o padrão W3C para um projeto de segunda camada para DID na Hathor

 - **Identificação do Escopo:** Definir como o DID será utilizado (por exemplo, autenticação, verificação de identidade)
 - **Modelo de Dados:** Definir como os dados serão estruturados de acordo com os padrões W3C.
 - **Interação com UTXO e ZKP:** Integrar a estrutura do DID com UTXOs e utilizar ZKPs para validar transações sem revelar dados sensíveis.
