# 💰 Wallet App API

[English](#english) | [Español](#español)

---

<a name="english"></a>

## 🇬🇧 English

### 📖 Description

**Wallet App API** is a robust and scalable backend designed for digital wallet applications. It provides a complete RESTful API to manage user financial transactions, including categorization features, financial summaries, and intelligent rate limiting protection against abuse.

### 🎯 Problem It Solves

- **Centralized financial transaction management** - Allows creating, querying, and deleting transactions securely and efficiently
- **Income and expense categorization** - Facilitates tracking and analyzing spending patterns
- **Automatic balance calculation** - Provides instant summaries of balance, income, and expenses
- **API abuse protection** - Implements advanced rate limiting per user and IP
- **Serverless architecture** - Designed for scalability and optimized costs
- **Automatic keep-alive** - Prevents cold starts of serverless services through cron jobs
- **Multi-user support** - Supports multiple users with data isolation by user_id

### 🛠️ Technologies Used

#### Backend Stack

- **Node.js (v18+)** - Server-side JavaScript runtime environment
- **Express.js (v4.21)** - Minimalist and flexible web framework for Node.js
- **ES Modules** - Modern JavaScript syntax with import/export

#### Database

- **Neon Database** - High-performance serverless PostgreSQL
  - `@neondatabase/serverless` - SQL client optimized for serverless environments
  - Automatic schema with auto-created tables at startup

#### Cache and Rate Limiting

- **Upstash Redis** - Serverless Redis for distributed cache
  - `@upstash/redis` - Optimized Redis client
  - `@upstash/ratelimit` - Rate limiting system with sliding window
  - Configuration: 100 requests per 60 seconds per identifier

#### Scheduled Tasks

- **Cron** - Task scheduler for keep-alive
  - Automatic ping every 14 minutes
  - Prevents cold starts on serverless platforms

#### Security and Middleware

- **CORS** - Cross-origin resource sharing control
- **dotenv** - Secure environment variable management

#### Development Tools

- **Nodemon** - Auto-reload in development for increased productivity

### 📋 Prerequisites

- Node.js (v18 or higher)
- npm (v9 or higher)
- Neon Database account
- Upstash Redis account

### ⚙️ Installation

1. Clone the repository:

```bash
git clone https://github.com/Boris-Espinosa/Wallet-App-Api.git
cd Wallet-App-Api
```

2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file in the root directory:

```env
# Database Configuration
DATABASE_URL=postgresql://user:password@host/database?sslmode=require

# Upstash Redis Configuration
UPSTASH_REDIS_REST_URL=https://your-redis-url.upstash.io
UPSTASH_REDIS_REST_TOKEN=your-redis-token

# Server Configuration
PORT=5001
NODE_ENV=development  # development | production

# API URL for Cron Job (production only)
API_URL=https://your-deployed-api-url.com/api/health
```

4. Start the development server:

```bash
npm run dev
```

### 🚀 Available Scripts

- `npm start` - Start the production server
- `npm run dev` - Start the development server with auto-restart

### 📡 API Endpoints

#### Health Check (`/api`)

| Method | Endpoint  | Description         | Auth Required |
| ------ | --------- | ------------------- | ------------- |
| GET    | `/health` | Check server status | No            |

#### Transactions (`/api/transactions`)

| Method | Endpoint           | Description              | Auth Required |
| ------ | ------------------ | ------------------------ | ------------- |
| GET    | `/:userId`         | Get user transactions    | No            |
| POST   | `/`                | Create a new transaction | No            |
| DELETE | `/:id`             | Delete a transaction     | No            |
| GET    | `/summary/:userId` | Get financial summary    | No            |

### 📝 API Request Examples

#### Health Check

```bash
GET /api/health
```

**Response:**

```json
{
  "status": "ok"
}
```

#### Get User Transactions

```bash
GET /api/transactions/user123
```

**Response:**

```json
[
  {
    "id": 1,
    "user_id": "user123",
    "title": "Grocery shopping",
    "amount": -45.5,
    "category": "food",
    "created_at": "2025-12-16"
  }
]
```

#### Create a Transaction

```bash
POST /api/transactions
Content-Type: application/json

{
  "user_id": "user123",
  "title": "Netflix payment",
  "amount": -15.99,
  "category": "entertainment"
}
```

**Response:**

```json
{
  "id": 3,
  "user_id": "user123",
  "title": "Netflix payment",
  "amount": -15.99,
  "category": "entertainment",
  "created_at": "2025-12-16"
}
```

#### Delete Transaction

```bash
DELETE /api/transactions/3
```

**Response:**

```json
{
  "message": "Transaction deleted succesfully"
}
```

#### Get Financial Summary

```bash
GET /api/transactions/summary/user123
```

**Response:**

```json
{
  "balance": 2954.5,
  "income": 3000.0,
  "expenses": -45.5
}
```

### 🗂️ Project Structure

```
backend/
├── src/
│   ├── server.js             # Main application entry point
│   ├── config/
│   │   ├── db.js             # Neon Database configuration
│   │   ├── upstash.js        # Redis and Rate Limiter configuration
│   │   └── cron.js           # Scheduled tasks (keep-alive)
│   ├── controllers/
│   │   └── transactionsController.js  # Business logic
│   ├── middleware/
│   │   └── rateLimiter.js    # Rate limiting middleware
│   └── routes/
│       └── transactionsRoute.js       # Route definitions
├── .env                      # Environment variables
├── package.json              # Project dependencies
└── README.md                 # This file
```

### 🔐 Authentication

**Current Status:** This API currently does not implement authentication. The `user_id` is sent as part of the request.

**Production Recommendations:**

- Implement JWT (JSON Web Tokens)
- Add authentication middleware
- Validate tokens on each request
- Integrate with services like Auth0, Clerk, or Firebase Auth
- Implement roles and permissions

### 📦 Data Models

#### Transaction Model

```javascript
{
  id: SERIAL PRIMARY KEY,
  user_id: VARCHAR(255) NOT NULL,
  title: VARCHAR(255) NOT NULL,
  amount: DECIMAL(10,2) NOT NULL,
  category: VARCHAR(255) NOT NULL,
  created_at: DATE NOT NULL DEFAULT CURRENT_DATE
}
```

**Fields:**

- `id` - Unique auto-incrementing identifier
- `user_id` - User identifier (allows multi-user)
- `title` - Transaction description
- `amount` - Amount (decimal with 2 decimal places)
  - Positive values = Income
  - Negative values = Expenses
- `category` - Category for classification
- `created_at` - Creation date (automatic)

### 🔧 Features

- ✅ RESTful API with clean design
- ✅ Intelligent rate limiting per user and IP (100 requests/60s)
- ✅ Complete input data validation
- ✅ Robust error handling
- ✅ Detailed logging for better debugging
- ✅ Serverless database with auto-initialization
- ✅ Modular and scalable architecture
- ✅ Automatic keep-alive to prevent cold starts
- ✅ Multi-user support with data isolation
- ✅ Precise financial calculations (DECIMAL for amounts)
- ✅ Automatic date sorting
- ✅ CORS enabled for frontend integration

### 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### 📄 License

ISC

### 👤 Author

Boris Espinosa

- GitHub: [@Boris-Espinosa](https://github.com/Boris-Espinosa)

---

<a name="español"></a>

## 🇪🇸 Español

### 📖 Descripción

**Wallet App API** es un backend robusto y escalable diseñado para aplicaciones de billetera digital. Proporciona una API RESTful completa para gestionar transacciones financieras de usuarios, incluyendo funcionalidades de categorización, resúmenes financieros, y protección contra abuso mediante rate limiting inteligente.

### 🎯 Problema que Resuelve

- **Gestión centralizada de transacciones financieras** - Permite crear, consultar y eliminar transacciones de forma segura y eficiente
- **Categorización de gastos e ingresos** - Facilita el seguimiento y análisis de patrones de gasto
- **Cálculo automático de balances** - Proporciona resúmenes instantáneos de balance, ingresos y gastos
- **Protección contra abuso de API** - Implementa rate limiting avanzado por usuario y por IP
- **Arquitectura serverless** - Diseñado para escalabilidad y costos optimizados
- **Keep-alive automático** - Previene el cold start de servicios serverless mediante cron jobs
- **Multi-usuario** - Soporte para múltiples usuarios con aislamiento de datos por user_id

### 🛠️ Tecnologías Utilizadas

#### Stack Backend

- **Node.js (v18+)** - Entorno de ejecución JavaScript del lado del servidor
- **Express.js (v4.21)** - Framework web minimalista y flexible para Node.js
- **ES Modules** - Sintaxis moderna de JavaScript con import/export

#### Base de Datos

- **Neon Database** - PostgreSQL serverless de alto rendimiento
  - `@neondatabase/serverless` - Cliente SQL optimizado para entornos serverless
  - Schema automático con tablas auto-creadas al inicio

#### Caché y Rate Limiting

- **Upstash Redis** - Redis serverless para caché distribuido
  - `@upstash/redis` - Cliente Redis optimizado
  - `@upstash/ratelimit` - Sistema de rate limiting con ventana deslizante
  - Configuración: 100 requests por 60 segundos por identificador

#### Tareas Programadas

- **Cron** - Scheduler de tareas para keep-alive
  - Ping automático cada 14 minutos
  - Previene cold starts en plataformas serverless

#### Seguridad y Middleware

- **CORS** - Control de acceso de origen cruzado
- **dotenv** - Gestión segura de variables de entorno

#### Herramientas de Desarrollo

- **Nodemon** - Auto-reload en desarrollo para mayor productividad

### 📋 Prerequisitos

- Node.js (v18 o superior)
- npm (v9 o superior)
- Cuenta en Neon Database
- Cuenta en Upstash Redis

### ⚙️ Instalación

1. Clona el repositorio:

```bash
git clone https://github.com/Boris-Espinosa/Wallet-App-Api.git
cd Wallet-App-Api
```

2. Instala las dependencias:

```bash
npm install
```

3. Crea un archivo `.env` en el directorio raíz:

```env
# Configuración de Base de Datos
DATABASE_URL=postgresql://user:password@host/database?sslmode=require

# Configuración de Upstash Redis
UPSTASH_REDIS_REST_URL=https://your-redis-url.upstash.io
UPSTASH_REDIS_REST_TOKEN=your-redis-token

# Configuración del Servidor
PORT=5001
NODE_ENV=development  # development | production

# URL de la API para Cron Job (solo producción)
API_URL=https://your-deployed-api-url.com/api/health
```

4. Inicia el servidor de desarrollo:

```bash
npm run dev
```

### 🚀 Scripts Disponibles

- `npm start` - Inicia el servidor de producción
- `npm run dev` - Inicia el servidor de desarrollo con reinicio automático

### 📡 Endpoints de la API

#### Health Check (`/api`)

| Método | Endpoint  | Descripción               | Requiere Auth |
| ------ | --------- | ------------------------- | ------------- |
| GET    | `/health` | Verificar estado servidor | No            |

#### Transacciones (`/api/transactions`)

| Método | Endpoint           | Descripción                   | Requiere Auth |
| ------ | ------------------ | ----------------------------- | ------------- |
| GET    | `/:userId`         | Obtener transacciones usuario | No            |
| POST   | `/`                | Crear nueva transacción       | No            |
| DELETE | `/:id`             | Eliminar una transacción      | No            |
| GET    | `/summary/:userId` | Obtener resumen financiero    | No            |

### 📝 Ejemplos de Peticiones a la API

#### Health Check

```bash
GET /api/health
```

**Respuesta:**

```json
{
  "status": "ok"
}
```

#### Obtener Transacciones de Usuario

```bash
GET /api/transactions/user123
```

**Respuesta:**

```json
[
  {
    "id": 1,
    "user_id": "user123",
    "title": "Compra en supermercado",
    "amount": -45.5,
    "category": "food",
    "created_at": "2025-12-16"
  }
]
```

#### Crear una Transacción

```bash
POST /api/transactions
Content-Type: application/json

{
  "user_id": "user123",
  "title": "Pago de Netflix",
  "amount": -15.99,
  "category": "entertainment"
}
```

**Respuesta:**

```json
{
  "id": 3,
  "user_id": "user123",
  "title": "Pago de Netflix",
  "amount": -15.99,
  "category": "entertainment",
  "created_at": "2025-12-16"
}
```

#### Eliminar Transacción

```bash
DELETE /api/transactions/3
```

**Respuesta:**

```json
{
  "message": "Transaction deleted succesfully"
}
```

#### Obtener Resumen Financiero

```bash
GET /api/transactions/summary/user123
```

**Respuesta:**

```json
{
  "balance": 2954.5,
  "income": 3000.0,
  "expenses": -45.5
}
```

### 🗂️ Estructura del Proyecto

```
backend/
├── src/
│   ├── server.js             # Punto de entrada principal
│   ├── config/
│   │   ├── db.js             # Configuración de Neon Database
│   │   ├── upstash.js        # Configuración de Redis y Rate Limiter
│   │   └── cron.js           # Tareas programadas (keep-alive)
│   ├── controllers/
│   │   └── transactionsController.js  # Lógica de negocio
│   ├── middleware/
│   │   └── rateLimiter.js    # Middleware de rate limiting
│   └── routes/
│       └── transactionsRoute.js       # Definición de rutas
├── .env                      # Variables de entorno
├── package.json              # Dependencias del proyecto
└── README.md                 # Este archivo
```

### 🔐 Autenticación

**Estado Actual:** Esta API actualmente no implementa autenticación. El `user_id` se envía como parte de la petición.

**Recomendaciones para Producción:**

- Implementar JWT (JSON Web Tokens)
- Agregar middleware de autenticación
- Validar tokens en cada request
- Integrar con servicios como Auth0, Clerk, o Firebase Auth
- Implementar roles y permisos

### 📦 Modelos de Datos

#### Modelo de Transacción

```javascript
{
  id: SERIAL PRIMARY KEY,
  user_id: VARCHAR(255) NOT NULL,
  title: VARCHAR(255) NOT NULL,
  amount: DECIMAL(10,2) NOT NULL,
  category: VARCHAR(255) NOT NULL,
  created_at: DATE NOT NULL DEFAULT CURRENT_DATE
}
```

**Campos:**

- `id` - Identificador único autoincremental
- `user_id` - Identificador del usuario (permite multi-usuario)
- `title` - Descripción de la transacción
- `amount` - Monto (decimal con 2 decimales)
  - Valores positivos = Ingresos
  - Valores negativos = Gastos
- `category` - Categoría para clasificación
- `created_at` - Fecha de creación (automática)

### 🔧 Características

- ✅ API RESTful con diseño limpio
- ✅ Rate limiting inteligente por usuario e IP (100 requests/60s)
- ✅ Validación completa de datos de entrada
- ✅ Manejo robusto de errores
- ✅ Logging detallado para mejor debugging
- ✅ Base de datos serverless con auto-inicialización
- ✅ Arquitectura modular y escalable
- ✅ Keep-alive automático para prevenir cold starts
- ✅ Soporte multi-usuario con aislamiento de datos
- ✅ Cálculos financieros precisos (DECIMAL para montos)
- ✅ Ordenamiento automático por fecha
- ✅ CORS configurado para integración con frontend

### 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Por favor, siéntete libre de enviar un Pull Request.

### 📄 Licencia

ISC

### 👤 Autor

Boris Espinosa

- GitHub: [@Boris-Espinosa](https://github.com/Boris-Espinosa)
