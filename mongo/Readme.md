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

### Tarefa 7: Agregações Básicas

1.  Calcule o valor médio dos pedidos:
    ```javascript
      db.pedidos.aggregate([
      {
        $group: {
          _id: null,
          valorMedio: { $avg: "$valorTotal" }
        }
      }
    ])

  
    ```

2.  Conte o número de produtos por categoria:
    ```javascript
      db.produtos.aggregate([
      {
        $group: {
          _id: "$categoria",
          quantidade: { $sum: 1 }
        }
      }
    ])

    
    ```

3. Encontre os 3 clientes com maior valor total em pedidos:
    ```javascript
    db.pedidos.aggregate([
    {
      $group: {
        _id: "$cliente",
        totalGasto: { $sum: "$valorTotal" }
      }
    },
    { $sort: { totalGasto: -1 } },
    { $limit: 3 }
   ])
    ```

4. Calcule o total de vendas por mês: 
```javascript
db.pedidos.aggregate([
  {
    $group: {
      _id: "$data",
      totalVendas: { $sum: "$valorTotal" }
    }
  },
  { $sort: { "_id": 1 } }
])
```

5. Crie um relatório de produtos mais vendidos:
    ```javascript
    db.pedidos.aggregate([
      { $unwind: "$itens" },
      {
        $group: {
          _id: "$itens.produtoNome",
          quantidadeTotal: { $sum: "$itens.quantidade" }
        }
      },
      { $sort: { quantidadeTotal: -1 } }
    ])
  
    ```
### Tarefa 8: Agregações Avançadas

1. Crie um pipeline que mostre a distribuição de notas nas avaliações por categoria de produto:
    ```javascript
     db.avaliacoes.aggregate([
        {
          $group: {
            _id: { categorias: "$categorias", nota: "$nota" },
            total: { $sum: 1 }
          }
        },
        {
          $sort: {
            "_id.categorias": 1,
            "_id.nota": 1
          }
        }
      ])

  
    ```

2. Gere um relatório de vendas por dia da semana e hora do dia:
    ```javascript
     db.pedidos.aggregate([
        {
          $addFields: {
            dataPedido: { $dateFromString: { dateString: "$data" } }
          }
        },
        {
          $project: {
            diaSemana: { $dayOfWeek: "$dataPedido" }, 
            horaDia: { $hour: "$dataPedido" },
            valorTotal: 1
          }
        },
        {
          $group: {
            _id: { diaSemana: "$diaSemana", horaDia: "$horaDia" },
            totalVendas: { $sum: "$valorTotal" },
            qtdPedidos: { $sum: 1 }
          }
        },
        {
          $sort: { "_id.diaSemana": 1, "_id.horaDia": 1 }
        }
      ])

    
    ```

3. Calcule o tempo médio entre a criação do pedido e sua entrega:
    ```javascript
   db.pedidos.aggregate([
      { $project: {
          tempoEntregaMS: { $subtract: ["$dataEntrega", { $dateFromString: { dateString: "$data" } }] }
      }},
      { $project: {
          tempoEntregaDias: { $divide: ["$tempoEntregaMS", 1000 * 60 * 60 * 24] }
      }},
      { $group: {
          _id: null,
          tempoMedioDias: { $avg: "$tempoEntregaDias" }
      }}
    ])

    ```

4.  Identifique produtos que são frequentemente comprados juntos: 
```javascript
  db.pedidos.aggregate([
    { $project: { produtos: "$itens.produto" } },
    { $project: { produtos: 1, produtos2: "$produtos" } },
    { $unwind: "$produtos" },
    { $unwind: "$produtos2" },
    { $match: { $expr: { $lt: ["$produtos", "$produtos2"] } } },
    { $group: {
        _id: { produtoA: "$produtos", produtoB: "$produtos2" },
        quantidadeCompradosJuntos: { $sum: 1 }
      }
    },
    { $sort: { quantidadeCompradosJuntos: -1 } }
  ])

```

5. Crie um dashboard com indicadores de desempenho (vendas, ticket médio, produtos mais vistos):
    ```javascript
   db.pedidos.aggregate([
      {
        $facet: {
          indicadoresGerais: [
            { $unwind: "$itens" },
            { $group: {
                _id: "$_id",
                totalPedido: { $sum: { $multiply: ["$itens.quantidade", "$itens.preco"] } }
              }
            },
            { $group: {
                _id: null,
                totalVendas: { $sum: "$totalPedido" },
                quantidadePedidos: { $sum: 1 },
                ticketMedio: { $avg: "$totalPedido" }
              }
            },
            { $project: { _id: 0, totalVendas: 1, quantidadePedidos: 1, ticketMedio: 1 } }
          ],
          produtosMaisVendidos: [
            { $unwind: "$itens" },
            { $group: {
                _id: "$itens.produto",
                quantidadeVendida: { $sum: "$itens.quantidade" },
                totalVendido: { $sum: { $multiply: ["$itens.quantidade", "$itens.preco"] } }
              }
            },
            { $sort: { quantidadeVendida: -1 } },
            { $limit: 5 }
          ]
        }
      }
      ])

  
    ```

    

> 📘 *Este material foi desenvolvido com fins educacionais para reforçar o aprendizado prático de MongoDB, cobrindo inserções, consultas, relacionamentos e otimizações.*
