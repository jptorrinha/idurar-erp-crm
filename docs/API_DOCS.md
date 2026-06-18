# API Documentation

## Base URL

- Backend API endpoints are prefixed with `/core` for core features and `/app` for application features.

## Authentication

- **Login**: `POST /core/auth/login`
  - Request Body: `{ email: string, password: string, remember?: boolean }`
  - Response: JWT token set in HTTP-only cookie, admin profile info.

- **Logout**: `POST /core/auth/logout`
  - Requires valid JWT token.
  - Clears authentication cookie.

## Authorization

- Middleware `hasPermission(permission)` protects routes.
- Permissions include: `create`, `read`, `update`, `delete`, `upload`.

## Core API Endpoints

### Admin Management (`/core/admin`)

| Method | Path                      | Description                      | Auth Required | Permissions          |
|--------|---------------------------|--------------------------------|---------------|----------------------|
| POST   | /admin/create             | Create new admin               | Yes           | (default)            |
| GET    | /admin/read/:id           | Read admin by ID               | Yes           | read                 |
| PATCH  | /admin/update/:id         | Update admin info             | Yes           | (default)            |
| DELETE | /admin/delete/:id         | Soft delete admin             | Yes           | (default)            |
| GET    | /admin/search             | Search admins                 | Yes           | read                 |
| GET    | /admin/list               | Paginated list admins         | Yes           | read                 |
| GET    | /admin/listAll            | List all admins (limit 100)   | Yes           | read                 |
| PATCH  | /admin/password-update/:id| Update password               | Yes           | (default)            |
| GET    | /admin/profile            | Get current admin profile     | Yes           | read                 |
| PATCH  | /admin/status/:id         | Update enabled status         | Yes           | read                 |
| POST   | /admin/photo              | Upload admin photo            | Yes           | (default)            |
| PATCH  | /profile/update/:id       | Update profile (self only)    | Yes           | (default)            |

### Settings (`/core/setting`)

Standard CRUD plus:

- `GET /setting/readBySettingKey/:settingKey` - Get setting by key
- `PATCH /setting/updateBySettingKey/:settingKey` - Update setting by key
- `PATCH /setting/updateManySetting` - Bulk update settings

### Email Templates (`/core/email`)

Standard CRUD endpoints.

### File Uploads (`/core/upload`)

- `POST /upload/multiple/:model/:fieldId` - Upload multiple files
- `POST /upload/single/:model/:fieldId` - Upload single file

Requires `upload` permission.

## App API Endpoints

Each entity supports standard CRUD and additional custom endpoints.

### Employee (`/app/employee`)

- `POST /create` - Create employee
- `GET /read/:id` - Read employee
- `PATCH /update/:id` - Update employee
- `DELETE /delete/:id` - Delete employee
- `GET /search` - Search employees
- `GET /list` - List employees
- `GET /filter` - Filter employees

### Payment Mode (`/app/paymentMode`)

Same CRUD endpoints as Employee.

### Tax (`/app/taxes`)

- Custom create and update logic to manage default tax.
- Delete operation disabled.

### Client (`/app/client`)

- CRUD endpoints
- `GET /summary` - Get client summary
- Custom delete prevents deletion if client has quotes or invoices.

### Lead (`/app/lead`)

- CRUD endpoints
- `GET /summary` - Get lead summary

### Invoice (`/app/invoice`)

- CRUD endpoints
- `GET /pdf/:id` - Generate invoice PDF
- `POST /mail` - Send invoice email
- `GET /summary` - Get invoice summary

### Item (`/app/item`)

Standard CRUD endpoints.

### Quote (`/app/quote`)

- CRUD endpoints
- `GET /pdf/:id` - Generate quote PDF
- `POST /mail` - Send quote email
- `GET /summary` - Get quote summary
- `GET /convert/:id` - Convert quote to invoice

### Supplier (`/app/supplier`)

Standard CRUD endpoints.

### Supplier Order (`/app/supplierOrder`)

- CRUD endpoints
- `GET /summary` - Get supplier order summary

### Expense (`/app/expense`)

Standard CRUD endpoints.

### Expense Category (`/app/expenseCategory`)

Standard CRUD endpoints.

### Payment (`/app/payment`)

- CRUD endpoints
- `GET /pdf/:id` - Generate payment PDF
- `GET /summary` - Get payment summary

### Offer (`/app/offer`)

- CRUD endpoints
- `GET /pdf/:id` - Generate offer PDF
- `GET /summary` - Get offer summary

### Order (`/app/order`)

Standard CRUD endpoints.

### Inventory (`/app/inventory`)

Standard CRUD endpoints.

### KYC (`/app/kyc`)

- CRUD endpoints with file upload handling.

## Error Handling

- All endpoints return JSON with `success`, `result`, and `message` fields.
- Validation errors return HTTP 400 with details.
- Server errors return HTTP 500 with error message.

## Request and Response Examples

Refer to source code for detailed request body schemas and response formats.

---

*End of API Documentation*