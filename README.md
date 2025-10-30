# 2.3 - Databases with MongoDB - Level 1 - "Cul d'Ampolla Optics" 👓  
_Management system for clients, suppliers, employees, and glasses sales_

---

## 📋 Project Overview

This project defines the MongoDB data model for the optical store **“Cul d’Ampolla”**, which manages information about **customers**, **employees**, **suppliers**, **glasses**, and **sales**.

The backend runs in **IntelliJ IDEA (Java)**, and the database is provided by **MongoDB**, launched via **Docker Compose**.  
All collection schemas are defined under `src/init/` as JSON validation schemas.

---

## 🧩 MongoDB Collections Structure

### 👤 `customers`
File: [`customer.json`](DB_Collections/customer.json)

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Customer’s full name |
| `address` | object | Postal address (street, number, floor, city, postalCode, country) |
| `telephone` | string | Customer phone number |
| `email` | string | Customer email |
| `registrationDate` | date | Date when the customer was registered |
| `recommendedBy` | objectId / null | Reference to another customer who made the recommendation |

> JSON schema reference: `${project_root}/init/customer.json`

---

### 💼 `employees`
File: [`employee.json`](DB_Collections/employee.json)

| Field | Type | Description |
|-------|------|-------------|
| `firstName` | string | Employee first name |
| `lastName` | string | Employee last name |
| `email` | string | Employee corporate email |
| `telephone` | string | Contact number |
| `hireDate` | date | Employment start date |
| `active` | boolean | Indicates whether the employee is currently active |

---

### 🏭 `providers`
File: [`provider.json`](DB_Collections/provider.json)

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Provider’s name |
| `address` | object | Full address (street, number, floor, apartment, city, postal_code, country) |
| `telephone` | string | Telephone number |
| `fax` | string | Fax number |
| `nif` | string | Provider identification (NIF or VAT number) |

---

### 👓 `glasses`
File: [`glasses.json`](DB_Collections/glasses.json)

| Field | Type | Description |
|-------|------|-------------|
| `brand` | string | Brand name |
| `frameType` | string (enum: `rimless`, `plastic`, `metal`) | Frame type |
| `frameColor` | string | Frame color |
| `lenses` | object | Left/right lenses with power and color |
| `price` | decimal | Price of the glasses |

---

### 🛒 `sales`
File: [`sale.json`](DB_Collections/sale.json)

| Field | Type | Description |
|-------|------|-------------|
| `customerId` | objectId | Reference to `customers._id` |
| `employeeId` | objectId | Reference to `employees._id` |
| `saleDate` | date | Date and time of the sale |
| `items` | array | Sold items (glassesId, quantity, unitPrice, discount, subtotal) |
| `paymentMethod` | string (enum: `cash`, `card`, `transfer`, `other`) | Payment type |
| `notes` | string | Optional sale notes |
| `total` | decimal | Total sale amount |

---

## 🗂️ Database Diagram Overview

All diagrams describing collection relationships are located in:

| Diagram | Description |
|----------|-------------|
| ![Customers](src/collections_diagrams/customer.png) | Customer schema |
| ![Employees](src/collections_diagrams/employee.png) | Employee schema |
| ![Providers](src/collections_diagrams/provider.png) | Provider schema |
| ![Glasses](src/collections_diagrams/glasses.png) | Glasses schema |
| ![Sales](src/collections_diagrams/sale.png) | Sales schema |

---

## 🐳 Running MongoDB with Docker

### 1️⃣. Prerequisites
- **Docker** and **Docker Compose** installed  
- **IntelliJ IDEA** (Java)  
- **MongoDB Compass**  

### 2️⃣. Start the MongoDB container

Run this command from the project root:

```bash
docker compose up -d
```

MongoDB will start on `localhost:27017`.

### 3️⃣. Access credentials

| Variable | Value |
|-----------|--------|
| `MONGO_INITDB_ROOT_USERNAME` | `admin` |
| `MONGO_INITDB_ROOT_PASSWORD` | `secret123` |

### 4️⃣. Connect using MongoDB Compass

1. Open **MongoDB Compass**  
2. Create a new connection with the following URI:

   ```
   mongodb://admin:secret123@localhost:27017/
   ```
3. Connect and verify the database and collections.

### 5️⃣. Initialization scripts
When the MongoDB container starts for the first time, it automatically executes them to create the collections and apply validation.

---

## ⚙️ Docker Compose Configuration

```yaml
version: "3.8"

services:
  mongodb:
    image: mongo:7.0
    container_name: mongodb
    restart: unless-stopped
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: secret123
    volumes:
      - mongodb_data:/data/db
      - ./init:/docker-entrypoint-initdb.d

volumes:
  mongodb_data:
```

---

## 🧠 IntelliJ IDEA Setup

1. Open the project folder in **IntelliJ IDEA**.  
2. Ensure **Docker** is running.  
3. Execute `docker compose up -d` to start MongoDB.  
4. Use **MongoDB Compass** to visually inspect collections and data.
