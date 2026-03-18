# 💳 API de Clientes e Contatos (Estilo Nubank)

Esta é uma API REST desenvolvida com **Spring Boot** para gerenciamento de **clientes e seus contatos**, simulando uma estrutura simples de relacionamento muito utilizada em sistemas financeiros e CRMs.

---

## 🚀 Funcionalidades

A API permite:

* 👤 Cadastro de clientes
* 📞 Cadastro de contatos vinculados a um cliente
* 📋 Listagem de clientes com seus contatos
* 🔎 Consulta de contatos por cliente

---

## 🧠 Regras de Negócio

### 👤 Cadastro de Cliente

* Um cliente pode ser cadastrado com ou sem contatos
* Caso existam contatos no cadastro:

  * Eles são automaticamente vinculados ao cliente
  * Persistidos junto com o cliente

---

### 📞 Cadastro de Contato

* Todo contato deve estar vinculado a um cliente existente
* Caso o cliente não exista, a API retorna erro

---

### 📋 Listagem de Clientes

* Retorna todos os clientes
* Cada cliente inclui sua lista de contatos

---

### 🔎 Consulta de Contatos por Cliente

* Busca todos os contatos com base no `clienteId`
* Retorna erro caso o cliente não exista

---

## 📂 Estrutura do Projeto

### 📌 Service (`ClienteService`)

Responsável pelas regras de negócio e conversões:

#### Métodos principais:

* `salvarCliente(ClienteDTO dto)`

  * Cria cliente
  * Converte contatos do DTO → Entity
  * Faz o vínculo entre cliente e contatos

* `buscarClientes()`

  * Retorna todos os clientes com seus contatos (DTO)

* `listarContatosPorCliente(Long clienteId)`

  * Busca contatos de um cliente específico

* `paraDTO(Cliente entity)`

  * Converte entidade em DTO de resposta

---

### 🌐 Controllers

---

### 👤 ClienteController (`/clientes`)

#### ➕ Criar cliente

```id="b4v9x1"
POST /clientes
```

**Body:**

```json id="u8s2jk"
{
  "nome": "João Silva",
  "contatos": [
    {
      "telefone": "11999999999",
      "email": "joao@email.com"
    }
  ]
}
```

**Resposta:**

* `201 Created`

---

#### 📋 Listar clientes

```id="q1k8zp"
GET /clientes
```

**Resposta:**

```json id="r4m9tz"
[
  {
    "id": 1,
    "nome": "João Silva",
    "contatos": [
      {
        "id": 10,
        "telefone": "11999999999",
        "email": "joao@email.com",
        "clienteId": 1
      }
    ]
  }
]
```

---

#### 🔎 Listar contatos por cliente

```id="y7n3lw"
GET /clientes/{id}/contatos
```

---

### 📞 ContatoController (`/contatos`)

#### ➕ Criar contato

```id="x2p6cd"
POST /contatos
```

**Body:**

```json id="h9v2sn"
{
  "telefone": "11988888888",
  "email": "novo@email.com",
  "clienteId": 1
}
```

**Resposta:**

```json id="p6z1tr"
{
  "id": 11,
  "telefone": "11988888888",
  "email": "novo@email.com",
  "clienteId": 1
}
```

---

## 🔄 Relacionamento entre Entidades

* Um **Cliente** pode ter vários **Contatos** (1:N)
* Cada contato pertence a apenas um cliente

---

## ⚙️ Conversão de Dados (DTOs)

A aplicação utiliza DTOs para:

* Evitar exposição direta das entidades
* Controlar os dados de entrada e saída
* Facilitar manutenção e escalabilidade

DTOs utilizados:

* `ClienteDTO` → entrada
* `ClienteResponseDTO` → saída
* `ContatoDTO` → entrada
* `ContatoResponseDTO` → saída

---

## 🛠️ Tecnologias Utilizadas

* Java 21
* Spring Boot
* Spring Data JPA
* Hibernate
* Lombok

---

## ⚠️ Tratamento de Erros

* `RuntimeException` → Cliente não encontrado

> 💡 Sugestão: implementar `@ControllerAdvice` para padronizar respostas de erro

---

## 📌 Observações

* O relacionamento entre cliente e contatos é gerenciado automaticamente
* A API já retorna dados estruturados prontos para consumo (frontend/mobile)

---

## 👨‍💻 Autor

Projeto desenvolvido para estudo de:

* Relacionamentos com JPA (OneToMany)
* Uso de DTOs
* Estruturação de APIs REST com boas práticas
