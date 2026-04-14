# TypeScript CRUD API with Node.js, Express & MySQL

A fully typed REST API for managing users with CRUD operations, built with TypeScript, Express, MySQL (Sequelize), and JWT authentication support.

## 🚀 Features

- ✅ **TypeScript** - Full type safety with strict mode enabled
- ✅ **Express.js** - Lightweight web framework
- ✅ **MySQL + Sequelize** - Type-safe ORM for database operations
- ✅ **Bcrypt** - Secure password hashing
- ✅ **Joi** - Request validation
- ✅ **CORS** - Cross-origin resource sharing
- ✅ **Global Error Handler** - Centralized error handling
- ✅ **Role-Based System** - Admin and User roles with enum type safety

## 📦 Prerequisites

- **Node.js** (v14+)
- **npm** (v6+)
- **MySQL 5.7+** running locally on port 3306
- **Postman** or **curl** for API testing

## 🔧 Setup Instructions

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Database

Edit `config.json` and update database credentials:

```json
{
  "database": {
    "host": "localhost",
    "port": 3306,
    "user": "root",
    "password": "", // your MySQL password
    "database": "typescript_crud_api"
  },
  "jwtSecret": "change-this-in-production-123!"
}
```

### 3. Start Development Server

```bash
npm run start:dev
```

You should see:

```
✅ Server running on http://localhost:4000
```

## 🏗️ Project Structure

```
typescript-crud-api/
├── src/
│   ├── _helpers/
│   │   ├── db.ts           # Database initialization
│   │   └── role.ts         # Role enum (Admin, User)
│   ├── _middleware/
│   │   ├── errorHandler.ts # Global error handler
│   │   └── validateRequest.ts # Joi validation middleware
│   ├── users/
│   │   ├── user.model.ts   # Sequelize User model (typed)
│   │   ├── user.service.ts # Business logic (CRUD)
│   │   └── users.controller.ts # Routes & validation schemas
│   └── server.ts           # Express app entry point
├── dist/                   # Compiled JavaScript (generated)
├── config.json            # Database & JWT config
├── tsconfig.json          # TypeScript configuration
└── package.json
```

## 🧪 API Testing Guide

### Test 1: Create a User (POST /users)

**Request:**

```bash
curl -X POST http://localhost:4000/users \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Mr",
    "firstName": "Jane",
    "lastName": "Smith",
    "email": "jane@example.com",
    "password": "secret123",
    "confirmPassword": "secret123",
    "role": "User"
  }'
```

**Expected Response (200 OK):**

```json
{
  "message": "User created"
}
```

### Test 2: Get All Users (GET /users)

**Request:**

```bash
curl http://localhost:4000/users
```

**Expected Response (200 OK):**

```json
[
  {
    "id": 1,
    "email": "jane@example.com",
    "title": "Mr",
    "firstName": "Jane",
    "lastName": "Smith",
    "role": "User",
    "createdAt": "2026-02-18T10:00:00.000Z",
    "updatedAt": "2026-02-18T10:00:00.000Z"
  }
]
```

**Note:** `passwordHash` is excluded by default (Sequelize `defaultScope`)

### Test 3: Get User by ID (GET /users/:id)

**Request:**

```bash
curl http://localhost:4000/users/1
```

**Expected Response (200 OK):**
Same as single user object above

**Error Case (404 Not Found):**

```bash
curl http://localhost:4000/users/999
```

**Error Response:**

```json
{
  "message": "User not found"
}
```

### Test 4: Update User (PUT /users/:id)

**Request:**

```bash
curl -X PUT http://localhost:4000/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Janet",
    "password": "newsecret456",
    "confirmPassword": "newsecret456"
  }'
```

**Expected Response (200 OK):**

```json
{
  "message": "User updated"
}
```

### Test 5: Delete User (DELETE /users/:id)

**Request:**

```bash
curl -X DELETE http://localhost:4000/users/1
```

**Expected Response (200 OK):**

```json
{
  "message": "User deleted"
}
```

### Test 6: Validation Errors (Bad Request)

**Request with missing required field:**

```bash
curl -X POST http://localhost:4000/users \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Bob"
  }'
```

**Expected Response (400 Bad Request):**

```json
{
  "message": "Validation error: email is required, password is required, ..."
}
```

## 🛠️ NPM Scripts

```bash
# Development with auto-reload
npm run start:dev

# Production build
npm run build

# Run compiled server
npm start

# Run tests
npm run test
```

## 📝 TypeScript Highlights

- **Interfaces** for data shapes (`UserAttributes`, `UserCreationAttributes`)
- **Enums** for type-safe role selection (`Role.Admin`, `Role.User`)
- **Strict mode** enabled - catches errors at compile time
- **Path aliases** - clean imports: `import { db } from '_helpers/db'`
- **Typed route handlers** - all Express handlers are fully typed
- **Generic types** - `Promise<User[]>`, `Optional<>` for optional fields

## 🔐 Security Notes

- ⚠️ **Passwords** are hashed with bcrypt (10 rounds)
- ⚠️ **JWT Secret** in `config.json` - change for production!
- ⚠️ **Database URL** in `config.json` - use environment variables in production
- ⚠️ **Env Variables** - store sensitive data in `.env` files (add to `.gitignore`)

## 🐛 Troubleshooting

| Problem                                               | Cause                              | Solution                                      |
| ----------------------------------------------------- | ---------------------------------- | --------------------------------------------- |
| `Cannot find module`                                  | Wrong import path                  | Check `tsconfig.json` paths config            |
| `Property 'X' does not exist on type`                 | Missing type definition            | Add interface or use `as Type` assertion      |
| `TS2345: Argument of type 'string' is not assignable` | Type mismatch                      | Check function signatures match Express types |
| `Error: connect ECONNREFUSED`                         | MySQL not running                  | Start MySQL server on port 3306               |
| `Database connection failed`                          | Wrong credentials in `config.json` | Update database host, user, password          |

## 📚 Resources

- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Sequelize + TypeScript](https://sequelize.org/master/manual/typescript.html)
- [Express.js Guide](https://expressjs.com/)
- [Joi Validation](https://joi.dev/)
- [MySQL Documentation](https://dev.mysql.com/doc/)

## 📄 License

ISC

---

**Built by RONAN ANTOQUE**
