# 🏋️‍♂️ Mini Sistema de Gerenciamento de Academias

Sistema completo, moderno e responsivo para cadastro, consulta, edição e exclusão de unidades de academias de ginástica, musculação e crossfit.

O projeto foi construído seguindo rigorosos padrões de arquitetura desacoplada:
- **Backend**: API REST em **Node.js + Express + Mongoose + MongoDB**, pronta para deploy serverless na **Vercel**.
- **Frontend**: Aplicação independente em **HTML5, CSS3 moderno e JavaScript puro (Vanilla JS)** com design esportivo e dark theme premium.

---

## 📁 Estrutura do Projeto

```text
sistema-academia/
├── backend/                        # API REST e conexão com MongoDB
│   ├── api/
│   │   └── index.js                # Handler Serverless para publicação na Vercel
│   ├── src/
│   │   ├── config/
│   │   │   └── db.js               # Conexão Mongoose com cache para Serverless
│   │   ├── controllers/
│   │   │   └── academiaController.js # Lógica de negócio e CRUD
│   │   ├── models/
│   │   │   └── Academia.js         # Schema Mongoose com validações
│   │   ├── routes/
│   │   │   └── academiaRoutes.js   # Rotas /api/academias
│   │   ├── app.js                  # Configurações do Express e Middlewares
│   │   ├── server.js               # Servidor para produção / listener HTTP
│   │   ├── dev-server.js           # Servidor de desenvolvimento com Mongo em memória
│   │   └── seed.js                 # Povoamento inicial com academias de amostra
│   ├── tests/
│   │   └── academia.test.js        # Testes automatizados (Jest + Supertest)
│   ├── .env.example                # Modelo de variáveis de ambiente
│   ├── vercel.json                 # Roteamento e configurações da Vercel
│   └── package.json                # Dependências e scripts do backend
│
├── frontend/                       # Interface web do usuário
│   ├── index.html                  # Estrutura semântica e modais
│   ├── style.css                   # Design system fitness (Dark/Neon, responsivo)
│   ├── script.js                   # Integração com a API REST, DOM e Toasts
│   └── package.json                # Configuração para deploy/execução estática
│
├── Roadmap.md                      # Rastreamento de todas as etapas de desenvolvimento
├── Contexto.md                     # Resumo técnico contínuo e histórico de decisões
├── api.md                          # Documentação detalhada da API REST
├── README.md                       # Manual de instalação, execução e deploy
└── package.json                    # Scripts raiz para execução facilitada
```

---

## 🚀 Como Executar o Projeto Localmente

### Pré-requisitos
- **Node.js** (versão 18 ou superior instalada);
- **npm** (incluso com o Node.js);
- Navegador web moderno (Chrome, Firefox, Edge, Safari).

---

### Opção 1: Inicialização Rápida (Zero Configuração)

Para iniciar imediatamente sem precisar configurar previamente o MongoDB Atlas:

```bash
# 1. Instalar as dependências do backend
cd backend
npm install

# 2. Iniciar o servidor de desenvolvimento
# O dev-server inicia automaticamente um banco de dados em memória e popula 4 academias
npm run dev
```

O terminal exibirá:
```text
🚀 Servidor de Desenvolvimento rodando na porta 3000
🌐 API Endpoint: http://localhost:3000/api/academias
🩺 Health Check: http://localhost:3000/api/health
```

Agora, basta abrir o arquivo `frontend/index.html` diretamente no seu navegador, ou servir via HTTP:
```bash
# No diretório frontend:
npx serve -l 8080 .
# Acesse no navegador: http://localhost:8080
```

---

### Opção 2: Conexão com MongoDB Atlas ou MongoDB Local

1. No diretório `backend/`, crie o arquivo `.env` baseado no `.env.example`:
```bash
cp .env.example .env
```

2. Edite o arquivo `.env` com a sua string de conexão:
```env
PORT=3000
MONGODB_URI=mongodb+srv://<usuario>:<senha>@cluster0.mongodb.net/academias?retryWrites=true&w=majority
NODE_ENV=development
```

3. (Opcional) Popule o banco com academias de exemplo:
```bash
npm run seed
```

4. Inicie o servidor:
```bash
npm start
```

---

## 🧪 Como Rodar os Testes Automatizados

O projeto conta com **19 testes automatizados** cobrindo todas as rotas da API, regras de negócio, validações de campos e códigos de status HTTP:

```bash
cd backend
npm test
```

Cenários validados na suíte:
- [x] `GET /api/health` - Status do serviço e integridade da conexão;
- [x] `POST /api/academias` - Cadastro bem-sucedido com todos os campos;
- [x] `POST /api/academias` - Suporte a modalidades como array ou string por vírgula;
- [x] `POST /api/academias` - Rejeição por campos obrigatórios ausentes (400);
- [x] `POST /api/academias` - Rejeição de valores negativos em mensalidade e aparelhos (400);
- [x] `GET /api/academias` - Listagem completa de academias;
- [x] `GET /api/academias` - Filtro textual por termo de busca no nome;
- [x] `GET /api/academias` - Filtro por modalidade específica;
- [x] `GET /api/academias/:id` - Consulta por ID existente (200);
- [x] `GET /api/academias/:id` - Consulta com ID não existente (404);
- [x] `GET /api/academias/:id` - Consulta com formato de ID inválido (400);
- [x] `PUT /api/academias/:id` - Atualização com sucesso (200);
- [x] `PUT /api/academias/:id` - Rejeição de campos inválidos (400);
- [x] `PUT /api/academias/:id` - ID inexistente (404) e ID malformado (400);
- [x] `DELETE /api/academias/:id` - Exclusão com sucesso (200) e confirmação no banco;
- [x] `DELETE /api/academias/:id` - ID inexistente (404) e ID malformado (400).

---

## ☁️ Orientações para Publicar o Backend na Vercel

O backend foi preparado especificamente para execução serverless na **Vercel** através dos arquivos `backend/api/index.js` e `backend/vercel.json`, utilizando conexões Mongoose cacheadas.

### Passo 1: Criar um Cluster Gratuito no MongoDB Atlas
1. Acesse [mongodb.com/atlas](https://www.mongodb.com/atlas) e crie uma conta gratuita;
2. Crie um novo Cluster (Tier M0 Free);
3. Em **Database Access**, crie um usuário e senha para a API;
4. Em **Network Access**, adicione o IP `0.0.0.0/0` (permitir acesso de qualquer IP para acomodar as lambdas da Vercel);
5. Clique em **Connect** > **Drivers (Node.js)** e copie a Connection String (`mongodb+srv://...`).

### Passo 2: Fazer Deploy na Vercel via Vercel CLI ou GitHub

#### Método A: Pelo Painel Web da Vercel (Recomendado)
1. Suba a pasta `backend` para um repositório no seu GitHub (ex: `sistema-academia-backend`);
2. Acesse [vercel.com](https://vercel.com) e clique em **Add New Project**;
3. Importe o repositório do backend;
4. Em **Root Directory**, selecione `.` ou `backend` (se estiver no mesmo repositório);
5. Em **Environment Variables**, adicione:
   - **Key**: `MONGODB_URI`
   - **Value**: `mongodb+srv://<usuario>:<senha>@cluster0.mongodb.net/academias?retryWrites=true&w=majority`
   - **Key**: `NODE_ENV`
   - **Value**: `production`
6. Clique em **Deploy**.

#### Método B: Pela Vercel CLI
```bash
cd backend
npx vercel
# Siga as instruções no terminal e configure a variável de ambiente:
npx vercel env add MONGODB_URI
npx vercel --prod
```

### Passo 3: Conectar o Frontend à API na Vercel
1. Após a publicação, você receberá a URL de produção da Vercel (ex: `https://sua-api.vercel.app`);
2. Abra o frontend, clique no indicador de status da API no cabeçalho (ou no ícone de engrenagem) e insira:
   `https://sua-api.vercel.app/api`
3. Pronto! O frontend se comunicará automaticamente com a API na nuvem e salvará a preferência no navegador (`localStorage`).

---

## 📦 Como Criar Repositórios Separados no GitHub

Para manter o Backend e o Frontend em repositórios separados no GitHub, execute os seguintes comandos no terminal:

### Repositório do Backend
```bash
# 1. Acesse a pasta do backend
cd c:\Users\Aluno\Desktop\sistema-academia\backend

# 2. Inicialize o repositório git
git init
git add .
git commit -m "feat: backend API REST com Node.js, Express e MongoDB"

# 3. Vincule ao seu repositório remoto criado no GitHub
git branch -M main
git remote add origin https://github.com/<seu-usuario>/sistema-academia-backend.git
git push -u origin main
```

### Repositório do Frontend
```bash
# 1. Acesse a pasta do frontend
cd c:\Users\Aluno\Desktop\sistema-academia\frontend

# 2. Inicialize o repositório git
git init
git add .
git commit -m "feat: frontend responsivo em HTML, CSS e Vanilla JS"

# 3. Vincule ao seu repositório remoto criado no GitHub
git branch -M main
git remote add origin https://github.com/<seu-usuario>/sistema-academia-frontend.git
git push -u origin main
```

---

## 📋 Documentações Complementares

- 📄 [Roadmap.md](Roadmap.md): Registro das 8 etapas de desenvolvimento e progresso;
- 📄 [Contexto.md](Contexto.md): Visão técnica, histórico de decisões arquiteturais e pendências;
- 📄 [api.md](api.md): Documentação detalhada dos endpoints, payloads, cURLs e respostas.
