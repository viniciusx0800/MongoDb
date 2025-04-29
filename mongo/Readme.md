# Resolução das Tarefas da Apostila de MongoDB

Este repositório contém as respostas das tarefas propostas na apostila de MongoDB, realizadas durante o estudo prático da linguagem. As tarefas estão organizadas por tema, abrangendo desde operações básicas até consultas avançadas e otimização com índices.

---

## ✔️ Tarefas Concluídas

### Tarefa 01 e 02

- ✅ Tarefa 01: OK  
- ✅ Tarefa 02: OK  
  - **Solução**:  
    ```json
    { "nome": "categoria" }
    ```

---

### Tarefa 03: Operações CRUD Básicas

- Inserção de produto:
    ```json
    {
      "nome": "produto",
      "descricao": "Do produto",
      "preco": 19.90,
      "estoque": 45,
      "categoria": "Dente",
      "tags": ["higiene", "bucal", "antisséptico"]
    }
    ```

- Inserção de cliente:
    ```json
    {
      "nome": "Ana Paula Souza",
      "email": "ana.souza@email.com",
      "telefone": "(11) 91234-5678",
      "endereco": {
        "rua": "Rua das Flores",
        "numero": 123,
        "cidade": "São Paulo",
        "estado": "SP",
        "cep": "01001-000"
      }
    }
    ```

- Inserção de múltiplos clientes:
    ```javascript
    db.clientes.insertMany([
      {
        "nome": "Juliana Martins",
        "email": "juliana.martins@email.com",
        "telefone": "(31) 98765-4321",
        "endereco": {
          "rua": "Rua dos Andradas",
          "numero": 89,
          "cidade": "Belo Horizonte",
          "estado": "MG",
          "cep": "30120-010"
        }
      }
    ])
    ```

---

### Tarefa 04: Consultas Avançadas

1. Produtos com preço entre R$100 e R$500:
    ```javascript
    db.produtos.find({ preco: { $gte: 100, $lte: 500 } })
    ```

2. Busca por nome com regex:
    ```javascript
    db.produtos.find({ nome: { $regex: /Bola/i } })
    ```

3. Produtos por categoria ordenados por preço:
    ```javascript
    db.produtos.find({ categoria: "Andame" }, { nome: 1, preco: 1 })
    ```

4. Clientes por cidade:
    ```javascript
    db.clientes.find({ "endereco.cidade": "irece" })
    ```

5. Produtos com tags específicas:
    ```javascript
    db.produtos.find({ tags: { $all: ["fio"] } })
    ```

6. Top 5 produtos mais caros:
    ```javascript
    db.produtos.find({ categoria: "Andame" }, { nome: 1, preco: 1 }).limit(5)
    ```

---

### Tarefa 05: Relacionamentos e Documentos Complexos

1. Criação de pedido:
    ```javascript
    db.produtos.insertMany([
      {
        "cliente": "Ana Paula Ramalho",
        "data": "2025-04-22",
        "status": "concluído",
        "itens": [
          { "produtoNome": "Bola de Futebol", "quantidade": 2, "preco": 99.9 },
          { "produto": "Shampoo Fortalecedor", "quantidade": 1, "preco": 16.85 }
        ],
        "valorTotal": 216.65
      }
    ])
    ```

2. Avaliações dos produtos:
    ```javascript
    db.avaliacoes.insertMany([
      {
        "produto": "Bola de Futebol",
        "cliente": "Ana Paula Ramalho",
        "pedido": ObjectId("680bd549bd38784f945b804c"),
        "nota": 5,
        "comentario": "Ótima bola, excelente para jogo de campo!",
        "data": "2025-04-22"
      },
      {
        "produto": "Shampoo Fortalecedor",
        "cliente": "Ana Paula Ramalho",
        "pedido": ObjectId("680bd549bd38784f945b804c"),
        "nota": 4,
        "comentario": "Bom shampoo, deixou o cabelo mais forte.",
        "data": "2025-04-22"
      }
    ])
    ```

3. Consulta de pedidos por cliente com detalhes dos produtos.

---

### Tarefa 06: Índices e Otimização

1. Criação de índices:
    ```javascript
    db.produtos.createIndex({ nome: 1 })
    db.produtos.createIndex({ categoria: 1, preco: -1 })
    db.clientes.createIndex({ email: 1 }, { unique: true })
    db.pedidos.createIndex({ cliente: 1, data: -1 })
    ```

2. Análise de desempenho com explain():
    ```javascript
    db.produtos.find({ $text: { $search: "shampoo" } }).explain("executionStats")
    db.clientes.find({ email: "ana.ramalho@email.com" }).explain("executionStats")
    db.pedidos.find({ cliente: ObjectId("680923bcbfacf57a64b727d2") }).sort({ data: -1 }).explain("executionStats")
    db.produtos.find({ categoria: "Bola", preco: { $lte: 100 } }).explain("executionStats")
    db.produtos.find({ nome: "Bola de Futebol" }).explain("executionStats")
    db.produtos.find({ $text: { $search: "andame" } })
    ```

3. Índice de texto para busca em nome e descrição:
    ```javascript
    db.produtos.createIndex({ nome: "text", descricao: "text" })
    ```

4. Testes com diferentes tipos de consultas e tamanhos de resultados.

---

> 📘 *Este material foi desenvolvido com fins educacionais para reforçar o aprendizado prático de MongoDB, cobrindo inserções, consultas, relacionamentos e otimizações.*
