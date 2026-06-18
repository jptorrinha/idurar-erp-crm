# IDURAR ERP/CRM Architecture

## Overview

IDURAR ERP/CRM is a modular MERN stack application designed for extensibility, maintainability, and scalability. It separates concerns clearly between backend and frontend, with reusable components and middleware.

## Backend Architecture

### Modular Design

- **Core Controllers:** Handle system-wide functionalities such as admin management, settings, authentication, and email templates.
- **App Controllers:** Manage business domain entities like clients, invoices, quotes, payments, offers, suppliers, and inventory.

### CRUD Middleware

- A generic CRUD controller factory (`createCRUDController`) dynamically generates standard CRUD operations for any Mongoose model.
- This reduces code duplication and centralizes error handling and response formatting.

### Custom Business Logic

- Certain controllers extend the generic CRUD with custom methods, e.g., taxController disables delete and manages default tax logic.
- Invoice and quote controllers handle PDF generation, email sending, and quote-to-invoice conversion.

### Middleware

- **Authentication:** JWT tokens validated via `isValidAdminToken` middleware.
- **Authorization:** `hasPermission` middleware checks user roles and permissions.
- **File Uploads:** `multer` middleware configured for different entities, with custom storage and file path injection.
- **Error Handling:** Centralized error handler wraps async controller functions.

### PDF Generation

- Uses Pug templates for invoice, quote, offer, and payment PDFs.
- PDFs are generated on create/update operations and saved to disk.

### Database

- MongoDB with Mongoose ODM.
- Models include Admin, Client, Invoice, Quote, Payment, Offer, Supplier, Employee, etc.
- Soft delete implemented via `removed` flag.

## Frontend Architecture

### React with Vite

- Uses React 18 with functional components and hooks.
- Vite for fast development and optimized builds.

### Routing

- React Router v6 with lazy loading for routes.
- Public and private route components control access based on login state.

### State Management

- Redux with thunk middleware manages global state and async actions.

### UI Components

- Ant Design (AntD) for consistent UI elements.

### Folder Structure

- Components and pages organized by feature.
- Router components define route hierarchy and access control.

## Deployment Architecture

- Backend and frontend are separate projects.
- Backend runs on Node.js server with MongoDB.
- Frontend served via development server or built static files.

## Security

- JWT authentication with secure cookie storage.
- Passwords hashed with bcrypt.
- Role-based access control enforced.

## Summary

The architecture balances modularity and reuse, with a clear separation between generic CRUD logic and custom business rules. The frontend leverages modern React patterns and lazy loading for performance.

---

*End of Architecture Document*