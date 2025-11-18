# Banco de Dados: **Lanchonete do Raul**

Este README documenta a estrutura completa do banco de dados
**Lanchonete_do_Raul**, incluindo tabelas, relacionamentos, chaves
primárias e estrangeiras.

------------------------------------------------------------------------

## 🗂️ **1. Criação do Banco de Dados**

``` sql
CREATE DATABASE Lanchonete_do_Raul;
USE Lanchonete_do_Raul;
```

------------------------------------------------------------------------

## 🍔 **2. Estrutura de Cardápio e Produtos**

### **Categoria_Produto**

``` sql
CREATE TABLE Categoria_Produto (
    id_categoria INT PRIMARY KEY AUTO_INCREMENT,
    nome_categoria VARCHAR(100) NOT NULL
);
```

### **Produto**

``` sql
CREATE TABLE Produto (
    id_produto INT PRIMARY KEY AUTO_INCREMENT,
    nome_produto VARCHAR(150) NOT NULL,
    descricao TEXT,
    preco_venda DECIMAL(10, 2) NOT NULL,
    id_categoria INT,
    FOREIGN KEY (id_categoria) REFERENCES Categoria_Produto(id_categoria)
);
```

### **Ingrediente**

``` sql
CREATE TABLE Ingrediente (
    id_ingrediente INT PRIMARY KEY AUTO_INCREMENT,
    nome_ingrediente VARCHAR(100) NOT NULL
);
```

### **Produto_Ingrediente** (Tabela N:N)

``` sql
CREATE TABLE Produto_Ingrediente (
    id_produto INT,
    id_ingrediente INT,
    quantidade_necessaria DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (id_produto, id_ingrediente),
    FOREIGN KEY (id_produto) REFERENCES Produto(id_produto),
    FOREIGN KEY (id_ingrediente) REFERENCES Ingrediente(id_ingrediente)
);
```

------------------------------------------------------------------------

## 🧑‍🤝‍🧑 **3. Atendimento e Clientes**

### **Cliente**

``` sql
CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY AUTO_INCREMENT,
    nome_cliente VARCHAR(150) NOT NULL,
    telefone VARCHAR(15),
    email VARCHAR(100)
);
```

### **Endereco**

``` sql
CREATE TABLE Endereco (
    id_endereco INT PRIMARY KEY AUTO_INCREMENT,
    id_cliente INT NOT NULL,
    rua VARCHAR(150) NOT NULL,
    numero VARCHAR(10),
    bairro VARCHAR(100) NOT NULL,
    cidade VARCHAR(100) NOT NULL,
    cep VARCHAR(10),
    complemento VARCHAR(100),
    ponto_referencia VARCHAR(150),
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
);
```

### **Atendente**

``` sql
CREATE TABLE Atendente (
    id_atendente INT PRIMARY KEY AUTO_INCREMENT,
    nome_atendente VARCHAR(150) NOT NULL,
    cargo VARCHAR(50)
);
```

------------------------------------------------------------------------

## 📦 **4. Controle de Estoque e Compras**

### **Fornecedor**

``` sql
CREATE TABLE Fornecedor (
    id_fornecedor INT PRIMARY KEY AUTO_INCREMENT,
    nome_fornecedor VARCHAR(150) NOT NULL,
    telefone VARCHAR(15)
);
```

### **Estoque**

``` sql
CREATE TABLE Estoque (
    id_ingrediente INT PRIMARY KEY,
    quantidade_estoque DECIMAL(10, 2) NOT NULL,
    unidade_medida VARCHAR(20) NOT NULL,
    estoque_minimo DECIMAL(10, 2),
    data_ultima_compra DATE,
    FOREIGN KEY (id_ingrediente) REFERENCES Ingrediente(id_ingrediente)
);
```

### **Compra**

``` sql
CREATE TABLE Compra (
    id_compra INT PRIMARY KEY AUTO_INCREMENT,
    id_fornecedor INT NOT NULL,
    data_compra DATE NOT NULL,
    valor_total_compra DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_fornecedor) REFERENCES Fornecedor(id_fornecedor)
);
```

### **Item_Compra**

``` sql
CREATE TABLE Item_Compra (
    id_item_compra INT PRIMARY KEY AUTO_INCREMENT,
    id_compra INT NOT NULL,
    id_ingrediente INT NOT NULL,
    quantidade_comprada DECIMAL(10, 2) NOT NULL,
    preco_unitario_compra DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_compra) REFERENCES Compra(id_compra),
    FOREIGN KEY (id_ingrediente) REFERENCES Ingrediente(id_ingrediente)
);
```

------------------------------------------------------------------------

## 🧾 **5. Pedidos e Transações**

### **Pedido**

``` sql
CREATE TABLE Pedido (
    id_pedido INT PRIMARY KEY AUTO_INCREMENT,
    id_cliente INT,
    id_atendente INT,
    data_hora_pedido DATETIME NOT NULL,
    tipo_atendimento ENUM('Presencial', 'Delivery') NOT NULL,
    valor_total DECIMAL(10, 2) NOT NULL,
    status_pedido ENUM('Recebido', 'Em Preparação', 'Pronto', 'Entregue', 'Cancelado') NOT NULL,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente),
    FOREIGN KEY (id_atendente) REFERENCES Atendente(id_atendente)
);
```

### **Item_Pedido**

``` sql
CREATE TABLE Item_Pedido (
    id_item_pedido INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    id_produto INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario_aplicado DECIMAL(10, 2) NOT NULL,
    observacoes TEXT,
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_produto) REFERENCES Produto(id_produto)
);
```

### **Metodo_Pagamento**

``` sql
CREATE TABLE Metodo_Pagamento (
    id_metodo INT PRIMARY KEY AUTO_INCREMENT,
    nome_metodo VARCHAR(50) NOT NULL
);
```

### **Transacao**

``` sql
CREATE TABLE Transacao (
    id_transacao INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    id_metodo INT NOT NULL,
    valor_pago DECIMAL(10, 2) NOT NULL,
    data_hora_transacao DATETIME NOT NULL,
    status_pagamento ENUM('Aprovado', 'Pendente', 'Cancelado') NOT NULL,
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_metodo) REFERENCES Metodo_Pagamento(id_metodo)
);
```

------------------------------------------------------------------------

## 🚚 **6. Logística de Entrega**

### **Entregador**

``` sql
CREATE TABLE Entregador (
    id_entregador INT PRIMARY KEY AUTO_INCREMENT,
    nome_entregador VARCHAR(150) NOT NULL,
    veiculo VARCHAR(50)
);
```

### **Entrega**

``` sql
CREATE TABLE Entrega (
    id_entrega INT PRIMARY KEY AUTO_INCREMENT,
    id_pedido INT NOT NULL,
    id_entregador INT,
    id_endereco INT NOT NULL,
    data_hora_saida DATETIME,
    data_hora_entrega DATETIME,
    taxa_entrega DECIMAL(10, 2),
    status_entrega ENUM('Aguardando Retirada', 'Em Rota', 'Entregue', 'Problema') NOT NULL,
    FOREIGN KEY (id_pedido) REFERENCES Pedido(id_pedido),
    FOREIGN KEY (id_entregador) REFERENCES Entregador(id_entregador),
    FOREIGN KEY (id_endereco) REFERENCES Endereco(id_endereco)
);
```

------------------------------------------------------------------------

## 🏁 **Fim do Script**

Este documento pode ser utilizado diretamente como README no GitHub.
