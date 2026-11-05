# Contexto do Projeto – Mini Sistema de Gerenciamento de Academias

Este arquivo registra o contexto geral, o histórico de decisões técnicas, o estado atual do projeto, as funcionalidades desenvolvidas e as pendências. Ele é mantido atualizado a cada etapa concluída pelo agente.

---

## 📌 Visão Geral do Sistema

O **Mini Sistema de Gerenciamento de Academias** é uma aplicação completa (Full Stack) voltada à gestão de unidades de academia de ginástica, crossfit e musculação. O projeto é dividido de forma desacoplada em duas partes principais:

1. **Backend**: API REST desenvolvida em **Node.js + Express + Mongoose + MongoDB**, pronta para execução local e publicação serverless na plataforma **Vercel**.
2. **Frontend**: Aplicação web desenvolvida exclusivamente com tecnologias fundamentais da web (**HTML5, CSS3 e JavaScript puro**), com interface moderna, responsiva e identidade visual esportiva.

---

## 🛠️ Stack Tecnológica e Decisões de Arquitetura

### Backend
- **Ambiente de Execução**: Node.js (v24.x)
- **Framework Web**: Express.js (v4.21.x)
- **Banco de Dados**: MongoDB via Object Data Modeling (ODM) Mongoose (v8.8.x)
- **CORS**: Habilitado via middleware `cors` com suporte a todas as origens (permitindo consumo local ou em nuvem).
- **Variáveis de Ambiente**: Gerenciadas com biblioteca `dotenv`.
- **Estratégia Serverless (Vercel)**:
  - Criação de handler em `backend/api/index.js` exportando o app Express como serverless function.
  - Configuração do `backend/vercel.json` com regras de reescrita (`rewrites`) para direcionamento automático.
  - Conexão Mongoose em `backend/src/config/db.js` com cache singleton (`global.mongoose`), reutilizando pools de conexões e evitando sobrecarga no MongoDB Atlas durante invocações sucessivas.
  - Suporte duplo de roteamento no Express: rotas respondem tanto sob `/api/academias` quanto `/academias`, garantindo compatibilidade com qualquer configuração de proxy ou rewrite na Vercel.
- **Camadas Arquiteturais**:
  - `src/config/`: Conexão com MongoDB e pooling singleton.
  - `src/models/`: Esquema de dados Mongoose `Academia` com validações rigorosas de campos.
  - `src/controllers/`: Controladores de regras de negócio, tratamento de entradas e formatação de respostas JSON.
  - `src/routes/`: Definição dos endpoints REST.
  - `src/app.js`: Configuração do servidor Express e middlewares globais.
  - `src/server.js`: Ponto de entrada padrão para produção e ambiente local.
  - `src/dev-server.js`: Servidor de desenvolvimento com fallback inteligente para MongoDB em memória (`mongodb-memory-server`) e auto-seeding.
  - `src/seed.js`: Script de povoamento inicial com 4 academias completas e imagens em alta resolução.

### Frontend
- **HTML5**: Semântica estrita, acessibilidade (`aria-hidden`, `aria-label`), modais de diálogo e formulários estruturados.
- **CSS3 Moderno**: Flexbox, CSS Grid, variáveis CSS (Design Tokens de cores neon/dark), transições fluidas e total responsividade (Mobile First).
- **JavaScript Puro (Vanilla)**:
  - Padrão sem dependências externas ou frameworks pesados.
  - Comunicação assíncrona com `fetch` API e tratamento de `try/catch`.
  - Configuração dinâmica de URL base da API salva em `localStorage`, permitindo alternar entre Localhost e Vercel sem mexer no código.
  - Feedback visual através de sistema de notificações Toast flutuantes e spinners de carregamento.
  - Modais interativos com preview imediato de fotos e botões rápidos para preenchimento de modalidades esportivas.

---

## 🗃️ Modelo de Dados da Academia

| Campo | Tipo | Obrigatório | Descrição / Validação |
|:---|:---:|:---:|:---|
| `nome` | String | Sim | Nome da academia (mínimo 2, máximo 100 caracteres) |
| `endereco` | String | Sim | Endereço completo da unidade (mínimo 5 caracteres) |
| `telefone` | String | Sim | Telefone de contato formatado ou numérico (regex de validação) |
| `horarioFuncionamento` | String | Sim | Horários de atendimento (mínimo 3 caracteres) |
| `quantidadeAparelhos` | Number | Sim | Número total de aparelhos (inteiro, mínimo 0) |
| `modalidades` | [String] | Sim | Lista de modalidades esportivas (aceita array ou string separada por vírgula) |
| `precoMensalidade` | Number | Sim | Valor mensal em Reais (número maior ou igual a 0) |
| `fotoUrl` | String | Sim | URL válida da imagem da academia (http/https ou base64) |
| `createdAt` / `updatedAt` | Date | Automático | Timestamps gerados pelo Mongoose |

---

## 📊 Histórico de Progresso

### [2026-11-05] Etapa 1: Planejamento e Estruturação de Controle
- **Status**: Concluída ✅
- **Ações realizadas**:
  - Análise aprofundada dos requisitos do prompt do usuário.
  - Criação do `Roadmap.md`, `Contexto.md` e `api.md`.

### [2026-11-05] Etapa 2: Criação da Estrutura de Pastas e Configuração
- **Status**: Concluída ✅
- **Ações realizadas**:
  - Criação das pastas `backend/` e `frontend/`.
  - Configuração dos `package.json` de backend, frontend e raiz.
  - Instalação das dependências (`express`, `mongoose`, `cors`, `dotenv`, `jest`, `supertest`, `mongodb-memory-server`).
  - Criação dos arquivos `.env.example` e `.gitignore`.

### [2026-11-05] Etapa 3: Desenvolvimento do Backend
- **Status**: Concluída ✅
- **Ações realizadas**:
  - Implementado `src/config/db.js` com suporte a serverless (pooling global de conexão).
  - Implementado `src/models/Academia.js` com validações completas e setters flexíveis para modalidades.
  - Implementado `src/controllers/academiaController.js` com CRUD completo, filtros e tratamento de erros.
  - Implementado `src/routes/academiaRoutes.js` e montagem em `src/app.js`.
  - Configurado deploy serverless da Vercel em `backend/api/index.js` e `backend/vercel.json`.
  - Criado script de seed (`src/seed.js`) e dev server (`src/dev-server.js`).

### [2026-11-05] Etapa 4: Testes Automatizados da API
- **Status**: Concluída ✅
- **Ações realizadas**:
  - Desenvolvida suíte completa em `backend/tests/academia.test.js`.
  - Execução dos testes automatizados com Jest: **19 de 19 testes passaram com sucesso (100%)**.
  - Validação de todos os cenários de sucesso e erro (200, 201, 400, 404).

### [2026-11-05] Etapa 5: Desenvolvimento do Frontend
- **Status**: Concluída ✅
- **Ações realizadas**:
  - Desenvolvido `frontend/index.html` com estrutura semântica, cards de métricas, filtros, grid de cards e 4 modais (Cadastro/Edição, Exclusão, Detalhes, Configuração de API).
  - Desenvolvido `frontend/style.css` com tema fitness premium (dark theme, detalhes em neon e âmbar, total responsividade mobile-first).
  - Desenvolvido `frontend/script.js` com operações assíncronas completas, integração REST, toasts, previews de foto, chips de modalidades e persistência de configuração.

### [2026-11-05] Etapa 6: Integração e Testes Ponta a Ponta
- **Status**: Concluída ✅
- **Ações realizadas**:
  - Inicializado o servidor com MongoDB e 4 academias de amostra.
  - Executados testes de integração ponta a ponta simulando requisições HTTP reais.
  - Validados com sucesso: Criação (201), Listagem (200), Busca por ID (200), Atualização (200), Exclusão (200) e Verificação de 404.

### [2026-11-05] Etapa 7 & 8: Documentação, Vercel e Repositórios
- **Status**: Concluída ✅
- **Ações realizadas**:
  - `README.md` detalhado criado com instruções completas de execução local, deploy na Vercel e comandos de criação de repositórios separados no GitHub.
  - `Roadmap.md` e `Contexto.md` atualizados com status 100% concluído.
  - `api.md` atualizado com todas as informações e rotas.

---

## 🧭 Situação Final do Projeto
- **Status Geral**: 100% Concluído e Testado ✅
- **Pendências**: Nenhuma pendência funcional. O projeto está pronto para uso e publicação.
