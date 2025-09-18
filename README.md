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


```ruby
-- CRIAÇÃO DE BANCO DE DADOS PARA ECOMMERCE
CREATE DATABASE ecommerce;
USE ecommerce;

-- FORNECEDOR
CREATE TABLE Fornecedor (
    idFornecedor INT NOT NULL AUTO_INCREMENT,
    razao_social VARCHAR(100),
    cnpj VARCHAR(45),
    telefone VARCHAR(45),
    PRIMARY KEY (idFornecedor)
);

-- PRODUTO
CREATE TABLE Produto (
    idProduto INT NOT NULL AUTO_INCREMENT,
    pnome VARCHAR(100),
    categoria VARCHAR(50),
    descricao VARCHAR(255),
    valor DECIMAL(10,2),
    dimensoes VARCHAR(50),
    avaliacao FLOAT,
    PRIMARY KEY (idProduto)
);

-- TABELA JUNCTION: Fornecedor disponibiliza Produto
CREATE TABLE DisponibilizandoProduto (
    fornecedor_idFornecedor INT NOT NULL,
    produto_idProduto INT NOT NULL,
    PRIMARY KEY (fornecedor_idFornecedor, produto_idProduto),
    CONSTRAINT fk_disp_fornecedor FOREIGN KEY (fornecedor_idFornecedor)
        REFERENCES Fornecedor(idFornecedor) ON DELETE RESTRICT ON UPDATE CASCADE,
    CONSTRAINT fk_disp_produto FOREIGN KEY (produto_idProduto)
        REFERENCES Produto(idProduto)
);

-- ESTOQUE (local físico/depósito)
CREATE TABLE Estoque (
    idEstoque INT NOT NULL AUTO_INCREMENT,
    localizacao VARCHAR(100),
    quantidade INT DEFAULT 0,
    PRIMARY KEY (idEstoque)
);

-- RELAÇÃO ENTRE ESTOQUE E PRODUTO (estoque por produto)
CREATE TABLE Estoque_Produto (
    estoque_idEstoque INT NOT NULL,
    produto_idProduto INT NOT NULL,
    quantidade INT DEFAULT 0,
    endereco VARCHAR(100),
    PRIMARY KEY (estoque_idEstoque, produto_idProduto),
    CONSTRAINT fk_ep_estoque FOREIGN KEY (estoque_idEstoque)
        REFERENCES Estoque(idEstoque) ON DELETE RESTRICT ON UPDATE CASCADE,
    CONSTRAINT fk_ep_produto FOREIGN KEY (produto_idProduto)
        REFERENCES Produto(idProduto) ON DELETE RESTRICT ON UPDATE CASCADE
);

-- TERCEIRO VENDEDOR (vendedor externo)
CREATE TABLE TerceiroVendedor (
    idTerceiroVendedor INT NOT NULL AUTO_INCREMENT,
    razao_social VARCHAR(100),
    nome_fantasia VARCHAR(100),
    endereco VARCHAR(200),
    cnpj VARCHAR(45),
    cpf CHAR(11),
    PRIMARY KEY (idTerceiroVendedor)
);

-- PRODUTOS POR VENDEDOR (Terceiro)
CREATE TABLE ProdutosPorVendedor (
    idProdutoporVendedor INT NOT NULL AUTO_INCREMENT,
    produto_idProduto INT NOT NULL,
    terceiroVendedor_idTerceiroVendedor INT,
    quantidade INT DEFAULT 0,
    PRIMARY KEY (idProdutoporVendedor),
    CONSTRAINT fk_ppv_produto FOREIGN KEY (produto_idProduto)
        REFERENCES Produto(idProduto) ON DELETE RESTRICT ON UPDATE CASCADE,
    CONSTRAINT fk_ppv_terceiro FOREIGN KEY (terceiroVendedor_idTerceiroVendedor)
        REFERENCES TerceiroVendedor(idTerceiroVendedor) ON DELETE SET NULL ON UPDATE CASCADE
);

-- CLIENTE (entidade base)
CREATE TABLE Cliente (
    idCliente INT NOT NULL AUTO_INCREMENT,
    nome_meio VARCHAR(10), -- Alterado para VARCHAR para acomodar 'Ltda.'
    pnome VARCHAR(50),
    sobrenome VARCHAR(50),
    cpf CHAR(11),
    endereco VARCHAR(255),
    email VARCHAR(100),
    telefone VARCHAR(45),
    PRIMARY KEY (idCliente)
);

-- CLIENTE PJ (Pessoa Jurídica) -- ligação 1:1 com Cliente
CREATE TABLE ClientePJ (
    idClientePJ INT NOT NULL AUTO_INCREMENT,
    razao_social VARCHAR(100),
    cliente_idCliente INT NOT NULL,
    cnpj VARCHAR(45),
    inscricao_estadual VARCHAR(50),
    PRIMARY KEY (idClientePJ),
    CONSTRAINT fk_clientepj_cliente FOREIGN KEY (cliente_idCliente)
        REFERENCES Cliente(idCliente) ON DELETE CASCADE ON UPDATE CASCADE
);

-- CLIENTE PF (Pessoa Física) -- ligação 1:1 com Cliente
CREATE TABLE ClientePF (
    idClientePF INT NOT NULL AUTO_INCREMENT,
    nome VARCHAR(100),
    sobrenome VARCHAR(100),
    cliente_idCliente INT NOT NULL,
    data_nascimento DATE,
    PRIMARY KEY (idClientePF),
    CONSTRAINT fk_clientepf_cliente FOREIGN KEY (cliente_idCliente)
        REFERENCES Cliente(idCliente) ON DELETE CASCADE ON UPDATE CASCADE
);

-- ENTREGA
CREATE TABLE Entrega (
    idEntrega INT NOT NULL AUTO_INCREMENT,
    status VARCHAR(45),
    data_envio DATE,
    codigo_rastreio VARCHAR(100),
    data_entrega_prevista DATE,
    data_entrega_realizada DATE,
    PRIMARY KEY (idEntrega)
);

-- PEDIDO
CREATE TABLE Pedido (
    idPedido INT NOT NULL AUTO_INCREMENT,
    status ENUM('PENDENTE','PROCESSANDO','CONFIRMADO','CANCELADO','CONCLUIDO') DEFAULT 'PENDENTE',
    descricao VARCHAR(255),
    cliente_idCliente INT NOT NULL,
    frete DECIMAL(10,2),
    entrega_idEntrega INT,
    PRIMARY KEY (idPedido),
    CONSTRAINT fk_pedido_cliente FOREIGN KEY (cliente_idCliente)
        REFERENCES Cliente(idCliente) ON DELETE RESTRICT ON UPDATE CASCADE,
    CONSTRAINT fk_pedido_entrega FOREIGN KEY (entrega_idEntrega)
        REFERENCES Entrega(idEntrega) ON DELETE SET NULL ON UPDATE CASCADE
);

-- RELAÇÃO ENTRE PRODUTO E PEDIDO (itens do pedido)
CREATE TABLE RelacaoProdutoPedido (
    produto_idProduto INT NOT NULL,
    pedido_idPedido INT NOT NULL,
    quantidade INT DEFAULT 1,
    produto_status ENUM('SOLICITADO','RESERVADO','ENVIADO','ENTREGUE','DEVOLVIDO') DEFAULT 'SOLICITADO',
    PRIMARY KEY (produto_idProduto, pedido_idPedido),
    CONSTRAINT fk_rpp_produto FOREIGN KEY (produto_idProduto)
        REFERENCES Produto(idProduto) ON DELETE RESTRICT ON UPDATE CASCADE,
    CONSTRAINT fk_rpp_pedido FOREIGN KEY (pedido_idPedido)
        REFERENCES Pedido(idPedido) ON DELETE CASCADE ON UPDATE CASCADE
);

-- FORMA DE PAGAMENTO (vinculada ao cliente)
CREATE TABLE FormaDePagamento (
    idFormaDePagamento INT NOT NULL AUTO_INCREMENT,
    tipo ENUM('CARTAO','BOLETO','PIX','DEPOSITO','DOIS_CARTOES') NOT NULL,
    detalhe VARCHAR(255),
    cliente_idCliente INT NOT NULL,
    PRIMARY KEY (idFormaDePagamento),
    CONSTRAINT fk_fp_cliente FOREIGN KEY (cliente_idCliente)
        REFERENCES Cliente(idCliente) ON DELETE CASCADE ON UPDATE CASCADE
);
```
  
