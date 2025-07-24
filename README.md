# proj-logico-bd
Projeto DIO Construindo seu Primeiro Projeto Lógico de Banco de Dados

# Passo a Passo para Construir um Projeto Lógico de Banco de Dados com SQL

Este guia detalha os passos para construir um projeto lógico de banco de dados, desde a criação do esquema até a elaboração de queries SQL mais complexas.

## 1. Criação do Esquema Lógico (Etapa de Modelagem)

Esta etapa envolve a criação do modelo conceitual e lógico do banco de dados.  Não há informações suficientes nos prints para criar um modelo específico.  Portanto, este passo é deixado em aberto para ser adaptado ao contexto do seu projeto.

**Considerações Gerais:**

*   **Identificação das Entidades:** Defina as entidades principais que serão representadas no banco de dados. Ex: Clientes, Produtos, Pedidos.
*   **Definição dos Atributos:** Para cada entidade, liste os atributos (características) relevantes. Ex: Cliente (ID, Nome, Endereço, Telefone).
*   **Estabelecimento dos Relacionamentos:** Determine como as entidades se relacionam umas com as outras (1:1, 1:N, N:M). Ex: Um Cliente pode ter vários Pedidos (1:N).
*   **Diagrama Entidade-Relacionamento (DER):** Crie um diagrama visual que represente as entidades, atributos e relacionamentos.
*   **Normalização:** Aplique as formas normais para reduzir a redundância e garantir a integridade dos dados.
*   **Documentação:** Documente todas as decisões tomadas durante o processo de modelagem.

## 2. Criação do Script SQL para Criação do Esquema do Banco de Dados

Com o esquema lógico definido, o próximo passo é traduzi-lo para um script SQL que criará as tabelas no banco de dados.

**Exemplo Genérico (Adaptar ao seu esquema lógico):**

```sql
-- Criação da Tabela Clientes
CREATE TABLE Clientes (
    ID INT PRIMARY KEY,
    Nome VARCHAR(255),
    Endereco VARCHAR(255),
    Telefone VARCHAR(20)
);

-- Criação da Tabela Produtos
CREATE TABLE Produtos (
    ID INT PRIMARY KEY,
    Nome VARCHAR(255),
    Descricao TEXT,
    Preco DECIMAL(10, 2)
);

-- Criação da Tabela Pedidos
CREATE TABLE Pedidos (
    ID INT PRIMARY KEY,
    ClienteID INT,
    DataPedido DATE,
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ID)
);

-- Criação da Tabela ItensPedido (para relacionamento N:M entre Pedidos e Produtos)
CREATE TABLE ItensPedido (
    PedidoID INT,
    ProdutoID INT,
    Quantidade INT,
    PRIMARY KEY (PedidoID, ProdutoID),
    FOREIGN KEY (PedidoID) REFERENCES Pedidos(ID),
    FOREIGN KEY (ProdutoID) REFERENCES Produtos(ID)
);
```

**Considerações:**

*   **Tipos de Dados:** Escolha os tipos de dados apropriados para cada atributo (INT, VARCHAR, DATE, DECIMAL, etc.).
*   **Chaves Primárias:** Defina as chaves primárias de cada tabela.
*   **Chaves Estrangeiras:** Estabeleça os relacionamentos entre as tabelas utilizando chaves estrangeiras.
*   **Constraints:** Adicione constraints para garantir a integridade dos dados (NOT NULL, UNIQUE, CHECK).
*   **Índices:** Crie índices para otimizar o desempenho das consultas.

## 3. Persistência de Dados (Inserção de Dados para Testes)

Após a criação do esquema, é necessário inserir alguns dados para realizar testes.

**Exemplo Genérico (Adaptar aos seus dados):**

```sql
-- Inserção de dados na tabela Clientes
INSERT INTO Clientes (ID, Nome, Endereco, Telefone) VALUES
(1, 'João da Silva', 'Rua A, 123', '11-1234-5678'),
(2, 'Maria Souza', 'Avenida B, 456', '21-9876-5432');

-- Inserção de dados na tabela Produtos
INSERT INTO Produtos (ID, Nome, Descricao, Preco) VALUES
(1, 'Notebook', 'Notebook Dell Inspiron', 3500.00),
(2, 'Mouse', 'Mouse sem fio Logitech', 50.00);

-- Inserção de dados na tabela Pedidos
INSERT INTO Pedidos (ID, ClienteID, DataPedido) VALUES
(1, 1, '2023-10-26'),
(2, 2, '2023-10-27');

-- Inserção de dados na tabela ItensPedido
INSERT INTO ItensPedido (PedidoID, ProdutoID, Quantidade) VALUES
(1, 1, 1),
(1, 2, 2),
(2, 2, 1);
```

**Considerações:**

*   **Dados de Teste:** Insira dados realistas para simular o ambiente de produção.
*   **Testes de Integridade:** Verifique se os dados inseridos respeitam as constraints definidas no esquema.

## 4. Criação de Queries SQL Mais Complexas

Agora, elabore queries SQL que utilizem as cláusulas listadas abaixo para fornecer uma perspectiva mais complexa dos dados.

### Cláusulas e Exemplos

1.  **Recuperações Simples com SELECT Statement:**

    *   **Objetivo:** Recuperar todos os dados de uma tabela.
    *   **Exemplo:**

    ```sql
    -- Seleciona todos os dados da tabela Clientes
    SELECT * FROM Clientes;
    ```

2.  **Filtros com WHERE Statement:**

    *   **Objetivo:** Filtrar os dados com base em uma condição.
    *   **Exemplo:**

    ```sql
    -- Seleciona os clientes com ID maior que 1
    SELECT * FROM Clientes WHERE ID > 1;
    ```

    *   **Pergunta:** Quais são os clientes com ID maior que 1?

3.  **Crie expressões para gerar atributos derivados:**

    *   **Objetivo:** Criar novos atributos a partir dos existentes.
    *   **Exemplo:**

    ```sql
    -- Calcula o valor total de cada item do pedido
    SELECT
        PedidoID,
        ProdutoID,
        Quantidade,
        Preco * Quantidade AS ValorTotal
    FROM
        ItensPedido
    JOIN
        Produtos ON ItensPedido.ProdutoID = Produtos.ID;
    ```

    *   **Pergunta:** Qual o valor total de cada item do pedido?

4.  **Defina ordenações dos dados com ORDER BY:**

    *   **Objetivo:** Ordenar os resultados de uma consulta.
    *   **Exemplo:**

    ```sql
    -- Seleciona todos os clientes, ordenados por nome
    SELECT * FROM Clientes ORDER BY Nome;

    -- Seleciona todos os produtos, ordenados por preço de forma decrescente
    SELECT * FROM Produtos ORDER BY Preco DESC;
    ```

    *   **Pergunta:** Como listar os clientes ordenados por nome?

5.  **Condições de filtros aos grupos – HAVING Statement:**

    *   **Objetivo:** Filtrar grupos de dados após a agregação (usado com `GROUP BY`).
    *   **Exemplo:**

    ```sql
    -- Encontra os clientes que fizeram mais de um pedido
    SELECT ClienteID, COUNT(*) AS NumeroPedidos
    FROM Pedidos
    GROUP BY ClienteID
    HAVING COUNT(*) > 1;
    ```

    *   **Pergunta:** Quais clientes fizeram mais de um pedido?

6.  **Crie junções entre tabelas para fornecer uma perspectiva mais complexa dos dados:**

    *   **Objetivo:** Combinar dados de múltiplas tabelas.
    *   **Exemplo:**

    ```sql
    -- Seleciona o nome do cliente e a data de seus pedidos
    SELECT
        c.Nome,
        p.DataPedido
    FROM
        Clientes c
    JOIN
        Pedidos p ON c.ID = p.ClienteID;

    -- Seleciona o nome do cliente e os produtos que ele comprou
    SELECT
        c.Nome AS NomeCliente,
        p.Nome AS NomeProduto
    FROM
        Clientes c
    JOIN
        Pedidos pe ON c.ID = pe.ClienteID
    JOIN
        ItensPedido ip ON pe.ID = ip.PedidoID
    JOIN
        Produtos p ON ip.ProdutoID = p.ID;
    ```

    *   **Pergunta:** Quais os nomes dos clientes e a data de seus pedidos? Quais os nomes dos clientes e os produtos que compraram?

### Queries Mais Complexas (Exemplos Adicionais)

*   **Consulta com Subquery:**

    ```sql
    -- Encontra os produtos que custam mais do que a média dos preços
    SELECT * FROM Produtos WHERE Preco > (SELECT AVG(Preco) FROM Produtos);
    ```

*   **Consulta com GROUP BY e HAVING:**

    ```sql
    -- Encontra os clientes que gastaram mais de R$ 1000 em pedidos
    SELECT c.Nome, SUM(p.Preco * ip.Quantidade) AS TotalGasto
    FROM Clientes c
    JOIN Pedidos pe ON c.ID = pe.ClienteID
    JOIN ItensPedido ip ON pe.ID = ip.PedidoID
    JOIN Produtos p ON ip.ProdutoID = p.ID
    GROUP BY c.Nome
    HAVING SUM(p.Preco * ip.Quantidade) > 1000;
    ```

**Considerações:**

*   **Adapte as Queries:**  Modifique as queries para se adequarem ao seu esquema lógico e aos seus dados de teste.
*   **Use Alias:** Utilize aliases para tornar as queries mais legíveis.
*   **Otimize as Queries:** Analise o plano de execução das queries para identificar possíveis gargalos e otimizações.
*   **Documente as Queries:** Explique o objetivo de cada query e o significado dos resultados.

## Observações Adicionais

*   Não há um mínimo de queries a serem realizadas, mas os tópicos acima devem ser abordados.
*   Elabore perguntas que podem ser respondidas pelas consultas.
*   As cláusulas podem estar presentes em mais de uma query.

Este guia fornece uma estrutura básica para construir um projeto lógico de banco de dados.  Aproveite os exemplos e adapte-os às suas necessidades específicas.
