# 📘 DDL e DML --- Guia Educacional Completo

Este material explica de forma simples e objetiva o que são comandos
**DDL** e **DML**, ilustrando com exemplos reais do banco
**Lanchonete_do_Raul**.

------------------------------------------------------------------------

# 🧱 1. O que é DDL?

**DDL (Data Definition Language)** é o conjunto de comandos usados para
**definir a estrutura** do banco de dados.

Com ele, criamos, alteramos e removemos tabelas, bancos e restrições.

### 📌 Comandos DDL mais comuns:

-   `CREATE`
-   `ALTER`
-   `DROP`
-   `TRUNCATE`

------------------------------------------------------------------------

## 🏗️ Exemplos de DDL do projeto *Lanchonete_do_Raul*

### ✔️ Criando o banco:

``` sql
CREATE DATABASE Lanchonete_do_Raul;
```

### ✔️ Criando uma tabela:

``` sql
CREATE TABLE Categoria (
    categoria_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT
);
```

### ✔️ Criando tabela com chave estrangeira:

``` sql
CREATE TABLE Produto (
    produto_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(150) NOT NULL,
    preco DECIMAL(10,2) NOT NULL,
    categoria_id INT NOT NULL,
    FOREIGN KEY (categoria_id) REFERENCES Categoria(categoria_id)
);
```

### ✔️ Alterando uma tabela:

``` sql
ALTER TABLE Cliente ADD data_cadastro DATE NOT NULL;
```

------------------------------------------------------------------------

# 📦 2. O que é DML?

**DML (Data Manipulation Language)** é o conjunto de comandos usados
para **manipular dados** dentro das tabelas.

Com ele, inserimos, atualizamos, excluímos e consultamos informações.

### 📌 Comandos DML mais comuns:

-   `INSERT`
-   `UPDATE`
-   `DELETE`
-   `SELECT`

------------------------------------------------------------------------

## 🍔 Exemplos de DML do projeto *Lanchonete_do_Raul*

### ✔️ Inserindo dados:

``` sql
INSERT INTO Categoria (nome, descricao)
VALUES ('Lanches', 'Sanduíches e hambúrgueres artesanais');
```

``` sql
INSERT INTO Produto (nome, preco, categoria_id)
VALUES ('Hambúrguer Clássico', 22.90, 1);
```

------------------------------------------------------------------------

### ✔️ Atualizando dados:

``` sql
UPDATE Produto
SET preco = 24.90
WHERE produto_id = 1;
```

------------------------------------------------------------------------

### ✔️ Excluindo dados:

``` sql
DELETE FROM Ingrediente
WHERE ingrediente_id = 3;
```

------------------------------------------------------------------------

### ✔️ Consultando dados:

``` sql
SELECT 
    Produto.nome AS Produto,
    Produto.preco,
    Categoria.nome AS Categoria
FROM Produto
JOIN Categoria 
    ON Produto.categoria_id = Categoria.categoria_id;
```

``` sql
SELECT 
    Pedido.pedido_id,
    Pedido.data_hora,
    Pedido.status_pagamento
FROM Pedido
WHERE cliente_id = 2;
```

------------------------------------------------------------------------

# 🎯 Resumo Final


  | Categoria      |  Significa        |  Atua sobre        |  Exemplos       |
  |----------------| ------------------| -------------------| ----------------|
  |**DDL**         | Data Definition   | Estrutura          | `CREATE`,       |
  |                | Language          |                    |`ALTER`, `DROP`  | 
  |**DML**         | Data Manipulation | Dados              |`INSERT`,        |
  |                | Language          |                    |`UPDATE`,        |
  |                |                   |                    |`DELETE`,        |
  |                |                   |                    |`SELECT`         |
  ------------------------------------------------------------------------

------------------------------------------------------------------------
