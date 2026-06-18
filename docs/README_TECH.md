# IDURAR ERP/CRM Technical Documentation

## Overview

IDURAR is an open "Fair-Code" source ERP/CRM system built on the MERN stack (Node.js, Express.js, MongoDB, React.js) with Ant Design and Redux. It supports core business functions such as invoicing, inventory, accounting, and HR.

This documentation covers the backend and frontend architecture, API routes, deployment instructions, security, PDF generation, file uploads, error handling, reporting, licensing, and contribution guidelines.

---

## Table of Contents

- [Backend Architecture](#backend-architecture)
- [API Documentation](#api-documentation)
- [Frontend Architecture](#frontend-architecture)
- [Setup and Deployment](#setup-and-deployment)
- [PDF Generation](#pdf-generation)
- [Security](#security)
- [File Upload Handling](#file-upload-handling)
- [Error Handling and Validation](#error-handling-and-validation)
- [Summary and Reporting APIs](#summary-and-reporting-apis)
- [Licensing and Contribution](#licensing-and-contribution)
- [Scripts and Workflows](#scripts-and-workflows)
- [Maintenance and Version Control](#maintenance-and-version-control)

---

## Backend Architecture

The backend is built with Node.js and Express.js, structured into modular core and app controllers. It uses MongoDB with Mongoose ODM.

- **Modular Core and App Controllers:** The backend separates core features (e.g., admin, settings, authentication) and app-specific features (e.g., clients, invoices, offers).
- **Reusable CRUD Middleware:** A generic CRUD controller factory (`createCRUDController`) dynamically generates CRUD operations for models, reducing code duplication.
- **Custom Business Logic:** Specific controllers extend the generic CRUD with custom logic, such as tax management, invoice PDF generation, and quote conversion.
- **Middleware:** Includes permission checks, error handling, file upload handling, and JWT authentication.

---

## API Documentation

### Base URL

The backend API base URL is typically `/api` or `/core` and `/app` routes (depending on deployment).

### Authentication

- JWT-based authentication with tokens stored in HTTP-only cookies.
- Login endpoint: `/core/auth/login` (POST)
- Logout endpoint: `/core/auth/logout` (POST)
- Middleware `hasPermission` enforces authorization on routes.

### Key Endpoints

**Admin Management** (`/core/admin`): Create, read, update, delete, search, filter, profile, photo upload, password update.

**Settings** (`/core/setting`): CRUD operations, including update by setting key.

**Email Templates** (`/core/email`): CRUD operations.

**File Uploads** (`/core/upload`): Single and multiple file uploads with permission checks.

**App Entities** (`/app/*`): CRUD and additional operations for:
- Employee
- Payment Mode
- Tax (custom create and update logic)
- Client (custom delete and summary)
- Lead
- Invoice (CRUD, PDF generation, mail sending, summary)
- Item
- Quote (CRUD, PDF generation, mail sending, convert to invoice, summary)
- Supplier
- Supplier Order
- Expense
- Expense Category
- Payment (CRUD, PDF generation, summary)
- Offer (CRUD, PDF generation, summary)
- Order
- Inventory
- KYC (file upload handling)

Each entity supports standard RESTful operations with permission checks and error handling.

---

## Frontend Architecture

The frontend is built with React.js using Vite as the build tool.

- **React Components:** Organized by pages and features (e.g., Invoice, Quote, Payment, Admin).
- **React Router:** Uses `react-router-dom` v6 with lazy loading for routes to optimize bundle size.
- **Authentication:** Public and private routes managed via `PublicRoute` and `PrivateRoute` components checking login state.
- **State Management:** Uses Redux with thunk middleware for asynchronous actions.
- **UI Library:** Ant Design (AntD) components for consistent UI.

---

## Setup and Deployment

### Prerequisites

- Node.js v16.15.0
- npm 8.5.5
- MongoDB account and cluster

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/idurar/idurar-erp-crm.git
   cd idurar-erp-crm
   ```

2. Configure MongoDB:
   - Create MongoDB cluster and database.
   - Whitelist your IP.
   - Obtain connection URI.

3. Backend environment variables:
   - Edit `/backend/.env` file.
   - Set `DATABASE` to your MongoDB URI.
   - Set `JWT_SECRET` and other secrets.

4. Install backend dependencies:
   ```bash
   cd backend
   npm install
   ```

5. Run setup script:
   ```bash
   node setup/setup.js
   ```

6. Start backend server:
   ```bash
   npm run dev
   ```

7. Install frontend dependencies:
   ```bash
   cd ../frontend
   npm install
   ```

8. Start frontend server:
   ```bash
   npm run start
   ```

### Troubleshooting

- OpenSSL errors on Node.js v17+:
  - Downgrade to Node.js v16 or
  - Enable legacy OpenSSL provider:
    - Unix:
      ```bash
      export NODE_OPTIONS=--openssl-legacy-provider
      ```
    - Windows CMD:
      ```cmd
      set NODE_OPTIONS=--openssl-legacy-provider
      ```
    - PowerShell:
      ```powershell
      $env:NODE_OPTIONS = "--openssl-legacy-provider"
      ```

---

## PDF Generation

- Uses `pug` templates located in `/backend/views/pdf/` for invoices, quotes, offers, and payments.
- PDF generation is handled by `html-pdf` with custom middleware.
- Generated PDFs are saved in `/public/download/{entity}/`.
- Templates can be customized by editing the corresponding `.pug` files.
- PDF generation is triggered on create and update operations.

---

## Security

- JWT authentication with tokens stored in HTTP-only cookies.
- Middleware `isValidAdminToken` verifies tokens and admin status.
- Role-based permission checks via `hasPermission` middleware.
- Passwords hashed with bcrypt.
- Admin sessions tracked and managed.
- Secure cookie settings vary by environment.

---

## File Upload Handling

- Uses `multer` for file uploads.
- Custom middleware sets file paths in request body.
- Separate storage configurations for different entities (e.g., KYC files).
- Upload routes protected by permission middleware.
- Security best practices include file type restrictions and storage outside public root.

---

## Error Handling and Validation

- Uses `Joi` for request body validation.
- Mongoose schema validations enforced.
- Centralized error handler middleware.
- API responses include success flag, message, and error details where appropriate.

---

## Summary and Reporting APIs

- Summary endpoints exist for clients, invoices, quotes, offers, payments, and supplier orders.
- Support filtering by time periods (week, month, year).
- Provide aggregated counts, totals, and performance metrics.
- Example: `/app/invoice/summary?type=month`

---

## Licensing and Contribution

- Distributed under [Fair-Code](http://faircode.io) license.
- Developer Trial Use License and Enterprise License available.
- Commercial licenses available by contacting hello@idurarapp.com.
- Contribution guidelines available in `CONTRIBUTING.md`.

---

## Scripts and Workflows

- Backend scripts:
  - `npm run dev` - Start backend in development mode.
  - `npm start` - Start backend in production mode.
  - `npm run setup` - Run setup tasks.
  - `npm run reset` - Reset database.

- Frontend scripts:
  - `npm run dev` or `yarn start` - Start frontend development server.
  - `npm run build` - Build frontend for production.
  - `npm run preview` - Preview production build.

---

## Maintenance and Version Control

- Documentation should be updated alongside code changes.
- Use Git for version control.
- Follow commit and coding guidelines as per contribution docs.
- Regular reviews to ensure documentation accuracy and completeness.

---

For further details, please refer to the source code and inline comments.

---

*End of Technical Documentation*