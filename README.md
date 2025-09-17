# Desafio de Projeto: Construindo seu Primeiro Projeto Lógico de Banco de Dados

Este repositório documenta a resolução do desafio de projeto do Bootcamp "Randstad - Análise de Dados" da DIO, focado na modelagem e implementação de um banco de dados para um cenário de e-commerce.

O objetivo deste projeto foi aplicar os conceitos de modelagem lógica de banco de dados a partir de um modelo conceitual refinado, com foco em um sistema de e-commerce. As etapas do desafio incluíram:

 1. Modelagem Lógica:   Refinamento do modelo conceitual para um modelo lógico, considerando as particularidades de um sistema de e-commerce. Isso incluiu a definição de entidades (tabelas), atributos (colunas) e, principalmente, os relacionamentos entre eles, como chaves primárias e estrangeiras. Foram abordados relacionamentos 1:1 (Cliente PF e PJ), 1:N (Um cliente pode ter vários pedidos) e N:N (Um pedido pode ter vários produtos).

2. Criação do Schema (DDL): Desenvolvimento do script SQL (Data Definition Language) para a criação do banco de dados ecommerce, incluindo todas as tabelas, suas colunas, tipos de dados e constraints (restrições) como chaves primárias, chaves estrangeiras, AUTO_INCREMENT, ENUM, entre outras.

3. Persistência de Dados (DML): Criação de um script para popular as tabelas com dados fictícios. Esta etapa foi crucial para simular um ambiente real e permitir a execução de consultas para validação do modelo.

4. Execução de Queries Complexas: Elaboração de uma série de consultas SQL para extrair insights do banco de dados, demonstrando o domínio das seguintes cláusulas e conceitos:

- Recuperações Simples (SELECT): Listagem básica de dados das tabelas.

- Filtros (WHERE): Consulta de dados com base em condições específicas.

- Atributos Derivados: Criação de colunas temporárias para exibir informações calculadas.

- Ordenação (ORDER BY): Organização dos resultados em ordem crescente ou decrescente.

- Agrupamentos com Filtro (HAVING): Aplicação de condições a resultados de funções de agregação (COUNT, SUM, etc.).

- Junções entre Tabelas (JOIN): Combinação de dados de múltiplas tabelas para responder a perguntas mais complexas.

Etapas do Projeto
1. Construção do Modelo Lógico (Scripts DDL)
O script a seguir foi utilizado para criar o banco de dados e as tabelas, definindo a estrutura lógica do projeto de e-commerce:



  
