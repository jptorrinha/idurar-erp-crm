# IDURAR ERP CRM - Technical README

## Visão Geral do Sistema

IDURAR é um sistema Open "Fair-Code" Source ERP / CRM que oferece funcionalidades para gerenciamento de Faturas, Estoque, Contabilidade e Recursos Humanos. Baseado na stack MERN (Node.js, Express.js, MongoDB, React.js) com Ant Design (AntD) e Redux para frontend.

Destinado a empresas que buscam uma solução integrada para ERP e CRM com código aberto e possibilidade de customização.

## Como Rodar Localmente

1. Clone o repositório:
   ```bash
   git clone https://github.com/idurar/idurar-erp-crm.git
   cd idurar-erp-crm
   ```

2. Crie uma conta e cluster no MongoDB, obtenha a URI de conexão.

3. Configure o arquivo `.env` na pasta `/backend` com a variável:
   ```
   DATABASE="your-mongodb-uri"
   ```

4. Instale dependências backend:
   ```bash
   cd backend
   npm install
   ```

5. Execute o script de setup para criar dados iniciais:
   ```bash
   node setup/setup.js
   ```

6. Inicie o servidor backend:
   ```bash
   npm run dev
   ```

7. Em outro terminal, instale dependências frontend:
   ```bash
   cd frontend
   npm install
   ```

8. Inicie o servidor frontend:
   ```bash
   npm run start
   ```

9. Acesse `http://localhost:3000` no navegador.

> Caso encontre erro relacionado ao OpenSSL no Node.js v17+, siga as instruções para habilitar o legacy OpenSSL provider ou faça downgrade para Node.js v16.

## Variáveis de Ambiente

- `DATABASE`: URI de conexão MongoDB (obrigatório)
- `PORT`: Porta do servidor backend (default 8888)

Não foi possível confirmar outras variáveis de ambiente no código analisado.

## Estrutura de Pastas (Alto Nível)

- `/backend`: Código do servidor Node.js (Express.js)
  - `app.js`: Configuração do Express
  - `server.js`: Inicialização do servidor e conexão com MongoDB
  - `controllers/`: Controladores da API
  - `models/`: Modelos Mongoose para dados
  - `routes/`: Definição das rotas API
  - `middlewares/`: Middlewares para autenticação, upload, permissões
  - `setup/`: Scripts de setup e reset
  - `handlers/`: Tratamento de erros e downloads

- `/frontend`: Código React.js
  - `src/`: Código fonte React
  - `forms/`: Formulários React para entidades
  - `apps/`: Aplicações React (ErpApp, IdurarOs)
  - `locale/`: Internacionalização

## Principais Fluxos

- Autenticação JWT via `/api/login` e `/api/logout` (backend/routes/coreRoutes/coreAuth.js)
- API REST para entidades ERP/CRM (ex: Employee, Invoice, Client) via `/api/*` (backend/routes/appRoutes/appApi.js)
- Upload de arquivos com multer e armazenamento local (backend/middlewares/uploadMiddleware)
- Geração e download de PDFs para documentos financeiros (backend/handlers/downloadHandler/downloadPdf.js)
- Frontend React com roteamento condicional baseado em autenticação (frontend/src/RootApp.jsx, frontend/src/apps/IdurarOs.jsx)

## Dependências Relevantes

- Backend:
  - express, mongoose, bcryptjs, jsonwebtoken, multer, helmet, cors
  - mongoose-autopopulate, mongoose-sequence

- Frontend:
  - react, react-dom, redux, react-redux, redux-thunk
  - antd, @ant-design/icons, react-router-dom, axios
  - vite para bundling

## Observações, Riscos e Próximos Passos

- O sistema depende fortemente do MongoDB e da configuração correta do `.env` para conexão.
- Uploads são armazenados localmente em `public/uploads` e `upload/` (backend), o que pode ser um risco para escalabilidade e segurança em produção.
- Não há evidência de testes automatizados no contexto fornecido.
- Próximos passos recomendados:
  - Implementar monitoramento e logs para produção
  - Avaliar uso de armazenamento em nuvem para arquivos
  - Adicionar testes unitários e integração
  - Documentar variáveis de ambiente adicionais e configurações

(Evidência: README.md, backend/app.js, backend/server.js, backend/routes/, frontend/src/)
