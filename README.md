# 🎬 API de Filmes e Locais de Gravação

Esta API REST desenvolvida com **Spring Boot** consome dados públicos de filmes e seus locais de gravação, permitindo busca e autocomplete de títulos.

Os dados são obtidos de uma API externa (dataset público da cidade de San Francisco).

---

## 🚀 Funcionalidades

A API oferece:

* 🎥 Listagem de todos os filmes
* 🔍 Busca de filmes por título
* ⚡ Autocomplete de títulos (sugestões em tempo real)

---

## 🌐 Fonte dos Dados

Os dados são consumidos via **WebClient** de uma API pública:

```
https://data.sfgov.org/resource/yitu-d5am.json
```

Contém informações como:

* Título do filme
* Ano de lançamento
* Local de gravação
* Atores principais

---

## 🧠 Regras de Negócio

### 🎥 Listar Filmes

* Retorna todos os filmes disponíveis na API externa
* Caso seja informado um parâmetro `title`, realiza filtro

---

### 🔍 Buscar por Título

* Busca parcial (contains)

Exemplo:

```
?title=batman
```

---

### ⚡ Autocomplete

* Retorna sugestões de títulos com base no prefixo informado
* Limite de **10 resultados**
* Ordenados alfabeticamente
* Remove duplicados

---

## 📂 Estrutura do Projeto

### 📌 Model (`MovieLocation`)

Representa os dados consumidos da API externa:

* `title` → título do filme
* `release_year` → ano de lançamento
* `locations` → local de gravação
* `actor_1`, `actor_2`, `actor_3` → atores principais

---

### 📌 Service (`MovieLocationService`)

Responsável pela lógica de consumo e processamento:

#### Métodos:

* `getAllMovies()`

  * Consome a API externa usando `WebClient`
  * Retorna todos os filmes

* `filterByTitle(String query)`

  * Filtra filmes pelo título

* `autocomplete(String prefix)`

  * Retorna sugestões de títulos

---

### 🌐 Controller (`MovieLocationController`)

Responsável pelos endpoints:

---

## 🔗 Endpoints

### 🎥 Listar todos os filmes

```id="e1v7mz"
GET /movies
```

---

### 🔍 Buscar filmes por título

```id="b9n2kp"
GET /movies?title=batman
```

---

### ⚡ Autocomplete de títulos

```id="x8r4qd"
GET /movies/autocomplete?q=bat
```

**Resposta:**

```json id="k3p9wn"
[
  "Batman Begins",
  "Batman Returns"
]
```

---

## ⚙️ Tecnologias Utilizadas

* Java 21
* Spring Boot
* Spring WebFlux (`WebClient`)
* Lombok
* API pública (San Francisco Open Data)

---

## ⚠️ Observações

* Os dados **não são persistidos**, sendo consumidos em tempo real
* O método `.block()` é utilizado, tornando a chamada síncrona
* Dependência de disponibilidade da API externa

---


## 📌 Exemplo de Uso

1. Buscar todos os filmes
2. Filtrar por título específico
3. Usar autocomplete para sugerir títulos em um frontend

---

## 👨‍💻 Autor

Projeto desenvolvido para estudo de:

* Consumo de APIs externas
* Programação reativa com WebClient
* Manipulação de streams em Java
* Construção de APIs REST eficientes
