# EcommerceHub - E-commerce Backend Application

EcommerceHub is a robust, secure, and scalable backend REST API for an e-commerce platform built using the **Spring Boot** framework. It handles core e-commerce functionalities such as user authentication, product catalog management, cart operations, address management, and checkout/order processing.

## 🚀 Technologies Used

- **Java 17**
- **Spring Boot 3.5.x**
  - **Spring Web** (REST API)
  - **Spring Security** (Authentication and role-based access control)
  - **Spring Data JPA** (Database ORM)
  - **Spring Validation** (Data validation)
- **PostgreSQL** (Relational Database)
- **JSON Web Tokens (JWT)** (Secure stateless cookie-based authentication)
- **ModelMapper** (DTO-to-Entity mapping)
- **Lombok** (Boilerplate code reduction)
- **Maven** (Project build and dependency management)

---

## 🛠️ Key Features

1. **Authentication & Authorization**
   - Secure login (`signin`) and registration (`signup`) utilizing BCrypt password encoding.
   - State-managed JWT authentication stored in HTTP-Only cookies.
   - Role-Based Access Control (RBAC) with three levels of access:
     - `ROLE_USER`: Can browse products, manage their cart, manage addresses, and place orders.
     - `ROLE_SELLER`: Can manage their own products, update stock/images, and see stats.
     - `ROLE_ADMIN`: Full access to database, categories, products, and user roles.
   - Auto-seeded default roles and test users (`admin`, `seller1`, `user1`) on startup.

2. **Catalog Management**
   - Paginated and sorted retrieval of products and categories.
   - Admin/Seller actions to add, update, and delete products.
   - Product search by category and keyword.
   - Image upload support for product listings.

3. **Cart Management**
   - Add items to the cart, modify quantities, and remove items.
   - Automatic calculation of cart total, discounts, and final pricing.

4. **Address Management**
   - Add, edit, delete, and list multiple shipping/billing addresses for users.

5. **Order & Payment Processing**
   - Complete checkout system linking carts, shipping addresses, and orders.
   - Stock deduction verification upon order placement.
   - Payment record creation (supports Payment Methods: Cash on Delivery, Credit/Debit Cards, etc.).

---

## ⚙️ Local Configuration & Run

### Prerequisites
- **Java JDK 17** or higher
- **PostgreSQL** database server running locally or accessible on port `5432`

### 1. Database Setup
1. Create a database named `myapp` in your PostgreSQL instance:
   ```sql
   CREATE DATABASE myapp;
   ```
2. Open the [application.properties](src/main/resources/application.properties) file and configure your PostgreSQL connection settings:
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/myapp
   spring.datasource.username=your_postgres_username
   spring.datasource.password=your_postgres_password
   ```

### 2. Running the Application
Use the Maven wrapper to build and start the server:

```bash
# Compile and package the application
./mvnw clean package

# Run the Spring Boot application
./mvnw spring-boot:run
```

The server will start up by default on port **`5000`** (e.g., `http://localhost:5000/`).

---

## 🛣️ API Endpoints Reference

### 🔐 Authentication (`/api/auth`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| **POST** | `/api/auth/signup` | Register a new user | Public |
| **POST** | `/api/auth/signin` | Sign in user and set JWT cookie | Public |
| **POST** | `/api/auth/signout` | Sign out user and clear JWT cookie | Public |
| **GET** | `/api/auth/user` | Get currently signed-in user profile details | Authenticated |
| **GET** | `/api/auth/username` | Get currently signed-in username | Authenticated |

### 📦 Products & Categories (`/api`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| **GET** | `/api/public/products` | Get paginated, sorted list of all products | Public |
| **GET** | `/api/public/categories/{categoryId}/products` | Get products belonging to a category | Public |
| **GET** | `/api/public/products/keyword/{keyword}` | Search products by name/keyword | Public |
| **POST** | `/api/admin/categories/{categoryId}/product` | Add a product under a category | Admin/Seller |
| **PUT** | `/api/admin/products/{productId}` | Update product details | Admin/Seller |
| **DELETE** | `/api/admin/products/{productId}` | Delete a product | Admin/Seller |
| **PUT** | `/api/products/{productId}/image` | Upload/Update product image | Admin/Seller |

### 🛒 Cart & Checkout (`/api`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| **POST** | `/api/carts/products/{productId}/quantity/{quantity}` | Add/update item quantity in cart | User |
| **GET** | `/api/carts/user/cart` | Get current user's active cart | User |
| **PUT** | `/api/cart/products/{productId}/quantity/{quantity}` | Update quantity of a product in cart | User |
| **DELETE** | `/api/cart/products/{productId}` | Remove product from user's cart | User |
| **POST** | `/api/users/addresses/{addressId}/orders/{paymentMethod}` | Place an order from the cart | User |

### 📍 Addresses (`/api`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| **POST** | `/api/addresses` | Create a new shipping address | User |
| **GET** | `/api/addresses` | List all addresses for user | User |
| **GET** | `/api/addresses/{addressId}` | Get specific address details | User |
| **PUT** | `/api/addresses/{addressId}` | Update an existing address | User |
| **DELETE** | `/api/addresses/{addressId}` | Delete an address | User |

---

## 👥 Seed Accounts (Default on startup)
The application automatically creates the following database roles and test accounts on startup:
* **Admin User:**
  - **Username:** `admin` | **Password:** `adminPass` | **Email:** `admin@example.com`
* **Seller User:**
  - **Username:** `seller1` | **Password:** `password2` | **Email:** `seller1@example.com`
* **Regular User:**
  - **Username:** `user1` | **Password:** `password1` | **Email:** `user1@example.com`

