# [A nova geração](https://youtu.be/GpXAq6JvWL0?si=poTauiu2urwyTRgV)

# Respostas das tarefas da Apostila de mongo

### tarefas 

- 01 ok
- 02 ok 
  - solução : {"nome": "categoria"}


### tarefa 3: Operações CRUD Básicas

- Solução usada para resolver a tarefa:
  -  {
    "nome": "produto",
    "descricao": " Do produto",
   "preco": 19.90,
   " estoque": 45,
   " categoria": "Dente",
   " tags": ["higiene", "bucal", "antisséptico"]
  }
 ** tarefa 3 :usada no mongo direto 
  {
    nome: "Ana Paula Souza",
    email: "ana.souza@email.com",
    telefone: "(11) 91234-5678",
    endereço: ["rua": "Rua das Flores", "numero": 123, "cidade": "São Paulo", "estado":"SP","cep":"01001-000"]
  }
   

usada no terminal do mongo
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
  },
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

 ])

