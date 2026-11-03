# 🚨 API produtos não relacionados

## 📌 Sobre o projeto

Este projeto consiste em uma API REST desenvolvida utilizando **Node.js**, **Express** e **MongoDB**. Seu objetivo é realizar o gerenciamento de produtos de uma loja, permitindo cadastrar, consultar, atualizar e excluir informações armazenadas no banco de dados.

A API utiliza métodos HTTP para realizar as operações de CRUD (Create, Read, Update e Delete).

## 🚀 Tecnologias utilizadas

- **Node.js:** ambiente de execução JavaScript.
- **Express:** framework utilizado para criar o servidor e gerenciar as rotas.
- **MongoDB:** banco de dados não relacional utilizado para armazenar os produtos.
- **MongoDB Driver:** biblioteca responsável pela comunicação entre a aplicação e o banco de dados.

## ⚙️ Funcionamento do código

### 1. Importação das bibliotecas

```javascript
const express = require('express');
const {MongoClient,ObjectId} = require('mongodb');
```

Nessa parte, são importadas as bibliotecas necessárias para o funcionamento da aplicação:

- `express`: permite criar o servidor e configurar as rotas.
- `MongoClient`: estabelece a conexão com o banco de dados MongoDB.
- `ObjectId`: permite trabalhar com os identificadores únicos dos documentos.

### 2. Criação do servidor

```javascript
const app = express();
app.use(express.json());
```

É criada a aplicação utilizando o Express.

O `express.json()` permite que o servidor receba e interprete informações enviadas no formato JSON.

### 3. Conexão com o banco de dados

```javascript
const client = new MongoClient(process.env.MONGO_URI);
const produtos = client.db('loja').collection('produtos');
```

Nessa etapa, é configurada a conexão com o MongoDB por meio da variável de ambiente `MONGO_URI`.

O código seleciona o banco de dados chamado **loja** e a coleção **produtos**, onde as informações serão armazenadas.

### 4. Métodos HTTP (CRUD)

A API possui cinco rotas principais:

**POST - Cadastrar produto**

```javascript
app.post('/produtos', async (req,res) => {
    const novo_produto = await produtos.insertOne(req.body);
    res.status(201).json(novo_produto);
});
```

Recebe os dados de um novo produto no corpo da requisição e utiliza `insertOne()` para cadastrá-lo no banco de dados.

**GET - Listar todos os produtos**

```javascript
app.get('/produtos', async (req,res) => {
    const lista = await produtos.find().toArray();
    res.json(lista);
});
```

Utiliza `find()` para buscar todos os produtos cadastrados e `toArray()` para transformar os resultados em uma lista.

**GET - Consultar um produto específico**

```javascript
app.get('/produtos/:id', async (req,res) => {
    const produto_especifico = await produtos.findOne({
        _id: new ObjectId(req.params.id)
    });
    res.json(produto_especifico);
});
```

Permite consultar um produto específico utilizando seu ID, recebido pela URL.

**PUT - Atualizar produto**

```javascript
app.put('/produtos/:id', async (req,res) => {
    const produto_atualizado = await produtos.updateOne(
        {_id: new ObjectId(req.params.id)},
        {$set: req.body}
    );
    res.json(produto_atualizado);
});
```

Localiza um produto pelo ID e utiliza `updateOne()` junto com `$set` para atualizar suas informações.

**DELETE - Excluir produto**

```javascript
app.delete('/produtos/:id', async (req,res) => {
    const produto_excluido = await produtos.deleteOne({
        _id: new ObjectId(req.params.id)
    });
    res.json(produto_excluido);
});
```

Localiza o produto pelo ID e utiliza `deleteOne()` para removê-lo do banco de dados.

## ▶️ Como executar o projeto

### 1. Instalar as dependências

No terminal, execute:

```bash
npm install express mongodb
```

### 2. Configurar a conexão

Crie um arquivo `.env` na pasta principal do projeto e adicione sua conexão:

```env
MONGO_URI= sua_string_de_conexao_mongodb
```

### 3. Executar o servidor

Execute o comando:

```bash
node --watch --env-file=.env index.js
```

Se a conexão for realizada com sucesso, o terminal exibirá:

```text
CONECTADO AO MONGO COM SUCESSO
API RODANDO NA PORTA 3000
```

## 📡 Rotas da API

| Método | Rota | Função |
|---|---|---|
| POST | `/produtos` | Cadastrar produto |
| GET | `/produtos` | Listar produtos |
| GET | `/produtos/:id` | Consultar produto |
| PUT | `/produtos/:id` | Atualizar produto |
| DELETE | `/produtos/:id` | Excluir produto |

## 🗄️ Estrutura do banco de dados

O banco de dados utilizado possui a seguinte organização:

- Banco de dados: `loja`
- Coleção: `produtos`
- Documentos: informações dos produtos cadastrados.

## 🎯 Objetivo

O projeto tem como objetivo colocar em prática os conceitos de desenvolvimento de APIs REST, integração com banco de dados não relacional, utilização de métodos HTTP e operações CRUD utilizando JavaScript.
