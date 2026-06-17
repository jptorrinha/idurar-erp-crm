# Arquitetura do Sistema IDURAR ERP CRM

## Visão Arquitetural

IDURAR é uma aplicação web full-stack baseada na arquitetura MERN (MongoDB, Express.js, React.js, Node.js). O backend é um servidor REST API construído com Express.js, que se conecta a um banco de dados MongoDB usando Mongoose. O frontend é uma aplicação React.js que consome a API REST.

## Componentes/Camadas e Responsabilidades

- **Frontend (React.js):** Interface do usuário, formulários, roteamento, estado global via Redux, internacionalização.
- **Backend (Node.js/Express.js):** API REST, autenticação JWT, controle de permissões, lógica de negócio, manipulação de uploads, geração de PDFs.
- **Banco de Dados (MongoDB):** Armazenamento de dados de entidades como Admin, Employee, Invoice, Client, etc.
- **Middleware:** Segurança (helmet, cors), tratamento de erros, upload de arquivos (multer), permissões baseadas em roles.

## Integrações Externas e Pontos de Acoplamento

- MongoDB Atlas ou instância MongoDB externa (via URI em `.env`)
- SMTP para envio de emails (configurado via settings, evidência em setup/config/appConfig.json)
- Frontend proxy para backend configurado via Vite (frontend/vite.config.js)

## Fluxo Principal do Sistema

1. **Inicialização:**
   - Backend conecta ao MongoDB (backend/server.js).
   - Modelos Mongoose são carregados dinamicamente (glob em backend/server.js).
   - Servidor Express é iniciado (backend/app.js).

2. **Autenticação:**
   - Usuário envia credenciais para `/api/login` (backend/routes/coreRoutes/coreAuth.js).
   - JWT é gerado e validado em rotas protegidas (middleware isValidAdminToken).

3. **Operações CRUD:**
   - Frontend consome API REST para entidades (ex: `/api/employee/create`, `/api/invoice/list`).
   - Permissões são verificadas via middleware hasPermission.

4. **Uploads:**
   - Uploads de arquivos são tratados via multer, armazenados localmente.
   - Metadados de uploads são salvos no MongoDB (modelo Upload).

5. **Geração de PDFs:**
   - Documentos financeiros podem ser gerados e baixados via rotas específicas (ex: `/download/:directory/:file`).

6. **Frontend:**
   - React gerencia estado e roteamento, exibindo formulários e dados.
   - Internacionalização e temas são aplicados.

## Pontos de Falha, Observabilidade e Sugestões

- **Pontos de Falha:**
  - Conexão com MongoDB (ver tratamento de erro em backend/server.js).
  - Uploads locais podem falhar por permissões ou espaço.
  - Falhas na geração de PDFs podem causar erros 500.

- **Observabilidade:**
  - Atualmente, logs básicos no console (ex: backend/server.js).
  - Sugere-se implementar logging estruturado (ex: Winston) e monitoramento (ex: Prometheus, Grafana).
  - Adicionar métricas de uso e erros.

- **Sugestões:**
  - Implementar testes automatizados para backend e frontend.
  - Usar armazenamento em nuvem para uploads (S3, Azure Blob).
  - Melhorar tratamento de erros e mensagens para o usuário.

(Evidência: backend/app.js, backend/server.js, backend/routes/, backend/middlewares/, frontend/vite.config.js)
