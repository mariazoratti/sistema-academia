# Documentação da API REST – Mini Sistema de Gerenciamento de Academias

Esta documentação especifica detalhadamente os endpoints, métodos HTTP, parâmetros, formatos de dados, códigos de status e exemplos práticos para consumo da API de gerenciamento de academias.

---

## 🌐 Informações Gerais

- **Base URL Local**: `http://localhost:3000/api`
- **Base URL Produção (Vercel)**: `https://<seu-projeto>.vercel.app/api`
- **Formato de Dados**: `application/json`
- **Autenticação**: Pública (para os fins do mini sistema)

---

## 📌 Sumário de Endpoints

| Método | Endpoint | Descrição | Status Sucesso | Status Erros |
|:---:|:---|:---|:---:|:---:|
| `GET` | `/academias` | Listar todas as academias (com filtros opcionais) | `200 OK` | `500` |
| `GET` | `/academias/:id` | Buscar uma academia específica pelo ID | `200 OK` | `400`, `404`, `500` |
| `POST` | `/academias` | Cadastrar uma nova academia | `201 Created` | `400`, `500` |
| `PUT` | `/academias/:id` | Atualizar todos ou parte dos dados de uma academia | `200 OK` | `400`, `404`, `500` |
| `DELETE` | `/academias/:id` | Excluir uma academia do banco de dados | `200 OK` | `400`, `404`, `500` |
| `GET` | `/health` | Checagem de integridade da API e conexão com banco | `200 OK` | `500` |

---

## 📋 Detalhamento dos Endpoints

### 1. Listar Academias
Retorna a lista de todas as academias cadastradas no sistema. Suporta filtro por nome via query param.

- **Método**: `GET`
- **Endpoint**: `/api/academias`
- **Query Parameters (opcionais)**:
  - `busca` (string): Busca textual case-insensitive no nome ou endereço da academia.
  - `modalidade` (string): Filtra academias que oferecem uma modalidade específica.

#### Exemplo de Requisição:
```http
GET /api/academias HTTP/1.1
Host: localhost:3000
Accept: application/json
```

#### Exemplo de Resposta (`200 OK`):
```json
{
  "sucesso": true,
  "total": 2,
  "dados": [
    {
      "_id": "6729e2f9d86b5155f81a7b11",
      "nome": "Iron Fit Academia",
      "endereco": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP",
      "telefone": "(11) 98765-4321",
      "horarioFuncionamento": "Seg a Sex: 06h às 23h | Sáb e Dom: 08h às 18h",
      "quantidadeAparelhos": 85,
      "modalidades": ["Musculação", "Crossfit", "Spinning"],
      "precoMensalidade": 129.90,
      "fotoUrl": "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=800",
      "createdAt": "2026-11-05T10:00:00.000Z",
      "updatedAt": "2026-11-05T10:00:00.000Z"
    }
  ]
}
```

---

### 2. Buscar Academia por ID
Retorna os dados detalhados de uma única academia a partir de seu identificador MongoDB (`ObjectId`).

- **Método**: `GET`
- **Endpoint**: `/api/academias/:id`
- **Parâmetros de Rota**:
  - `id` (string, obrigatório): Identificador único do documento MongoDB (24 caracteres hexadecimais).

#### Exemplo de Resposta (`200 OK`):
```json
{
  "sucesso": true,
  "dados": {
    "_id": "6729e2f9d86b5155f81a7b11",
    "nome": "Iron Fit Academia",
    "endereco": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP",
    "telefone": "(11) 98765-4321",
    "horarioFuncionamento": "Seg a Sex: 06h às 23h | Sáb e Dom: 08h às 18h",
    "quantidadeAparelhos": 85,
    "modalidades": ["Musculação", "Crossfit", "Spinning"],
    "precoMensalidade": 129.90,
    "fotoUrl": "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=800"
  }
}
```

#### Exemplo de Erro (`404 Not Found`):
```json
{
  "sucesso": false,
  "mensagem": "Academia não encontrada."
}
```

---

### 3. Cadastrar Academia
Cria um novo registro de academia no MongoDB.

- **Método**: `POST`
- **Endpoint**: `/api/academias`
- **Headers**:
  - `Content-Type: application/json`

#### Corpo da Requisição (JSON):
```json
{
  "nome": "Power Gym Premium",
  "endereco": "Rua das Flores, 450 - Centro, Curitiba - PR",
  "telefone": "(41) 3222-1100",
  "horarioFuncionamento": "Seg a Sex: 05h às 00h | Sáb: 08h às 20h",
  "quantidadeAparelhos": 120,
  "modalidades": ["Musculação", "Pilates", "Natação", "Muay Thai"],
  "precoMensalidade": 179.90,
  "fotoUrl": "https://images.unsplash.com/photo-1540497077202-7c8a3999166f?w=800"
}
```

#### Exemplo de Resposta (`201 Created`):
```json
{
  "sucesso": true,
  "mensagem": "Academia cadastrada com sucesso!",
  "dados": {
    "_id": "6729e31ad86b5155f81a7b15",
    "nome": "Power Gym Premium",
    "endereco": "Rua das Flores, 450 - Centro, Curitiba - PR",
    "telefone": "(41) 3222-1100",
    "horarioFuncionamento": "Seg a Sex: 05h às 00h | Sáb: 08h às 20h",
    "quantidadeAparelhos": 120,
    "modalidades": ["Musculação", "Pilates", "Natação", "Muay Thai"],
    "precoMensalidade": 179.90,
    "fotoUrl": "https://images.unsplash.com/photo-1540497077202-7c8a3999166f?w=800",
    "createdAt": "2026-11-05T10:15:00.000Z",
    "updatedAt": "2026-11-05T10:15:00.000Z"
  }
}
```

#### Exemplo de Erro de Validação (`400 Bad Request`):
```json
{
  "sucesso": false,
  "mensagem": "Erros de validação nos campos.",
  "erros": [
    "O campo 'nome' é obrigatório.",
    "O campo 'precoMensalidade' deve ser um número maior ou igual a zero."
  ]
}
```

---

### 4. Atualizar Academia
Atualiza os campos de uma academia existente.

- **Método**: `PUT`
- **Endpoint**: `/api/academias/:id`
- **Headers**:
  - `Content-Type: application/json`

#### Exemplo de Requisição:
```json
{
  "precoMensalidade": 159.90,
  "quantidadeAparelhos": 130
}
```

#### Exemplo de Resposta (`200 OK`):
```json
{
  "sucesso": true,
  "mensagem": "Academia atualizada com sucesso!",
  "dados": {
    "_id": "6729e31ad86b5155f81a7b15",
    "nome": "Power Gym Premium",
    "endereco": "Rua das Flores, 450 - Centro, Curitiba - PR",
    "telefone": "(41) 3222-1100",
    "horarioFuncionamento": "Seg a Sex: 05h às 00h | Sáb: 08h às 20h",
    "quantidadeAparelhos": 130,
    "modalidades": ["Musculação", "Pilates", "Natação", "Muay Thai"],
    "precoMensalidade": 159.90,
    "fotoUrl": "https://images.unsplash.com/photo-1540497077202-7c8a3999166f?w=800",
    "updatedAt": "2026-11-05T10:30:00.000Z"
  }
}
```

---

### 5. Excluir Academia
Remove permanentemente a academia do banco de dados.

- **Método**: `DELETE`
- **Endpoint**: `/api/academias/:id`

#### Exemplo de Resposta (`200 OK`):
```json
{
  "sucesso": true,
  "mensagem": "Academia excluída com sucesso!",
  "dados": {
    "_id": "6729e31ad86b5155f81a7b15",
    "nome": "Power Gym Premium"
  }
}
```

---

## 🧪 Instruções para Testar a API

### Via cURL no Terminal:

**1. Listar academias:**
```bash
curl -X GET http://localhost:3000/api/academias
```

**2. Cadastrar academia:**
```bash
curl -X POST http://localhost:3000/api/academias \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Alpha Fitness Club",
    "endereco": "Av. Brasil, 500 - Rio de Janeiro - RJ",
    "telefone": "(21) 2555-8899",
    "horarioFuncionamento": "Seg a Sex: 06h às 22h",
    "quantidadeAparelhos": 90,
    "modalidades": ["Musculação", "Dança", "Funcional"],
    "precoMensalidade": 119.90,
    "fotoUrl": "https://images.unsplash.com/photo-1517838277536-f5f99be501cd?w=800"
  }'
```

**3. Buscar por ID:**
```bash
curl -X GET http://localhost:3000/api/academias/<ID_AQUI>
```

**4. Atualizar por ID:**
```bash
curl -X PUT http://localhost:3000/api/academias/<ID_AQUI> \
  -H "Content-Type: application/json" \
  -d '{"precoMensalidade": 99.90}'
```

**5. Excluir por ID:**
```bash
curl -X DELETE http://localhost:3000/api/academias/<ID_AQUI>
```

---

## 🚦 Códigos de Status HTTP Utilizados

| Código | Nome | Quando é retornado |
|:---:|:---|:---|
| `200` | OK | Sucesso em requisições GET, PUT e DELETE |
| `201` | Created | Sucesso na criação com POST |
| `400` | Bad Request | Dados inválidos, campos obrigatórios ausentes ou ID com formato incorreto |
| `404` | Not Found | Recurso (academia) não encontrado para o ID fornecido |
| `500` | Internal Server Error | Erro inesperado do servidor ou falha de conexão com o banco |
