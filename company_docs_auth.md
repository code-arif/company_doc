# 🛡️ Developer Guide: User Authentication System

Welcome to the team! This document provides a comprehensive overview of how **User Authentication** is handled in our backend system. We use a robust, secure, and scalable architecture based on **Laravel** and **JWT (JSON Web Token)**.

---

![Authentication Flow Diagram](/C:/Users/rufuz/.gemini/antigravity/brain/d4cc4d45-eb64-45a9-a4ca-4aa88f0a9ba1/auth_flow_diagram_1777185264751.png)

---

## 🚀 Overview

Our authentication system is split into two main layers:
1.  **API Guard (`api`)**: Used for mobile applications and REST clients. It utilizes **Tymon/JWTAuth** for stateless authentication.
2.  **Web Guard (`web`)**: Used for the Admin Panel. It uses standard Laravel session-based authentication.

---

## 🔑 Authentication Flow

### 1. Registration & Email Verification
Users register via the API. We implement a two-step verification process using OTP (One-Time Password).

**Files:**
- [RegisterController.php](file:///c:/Users/rufuz/Herd/fsd_all_project/gtasign_backend/app/Http/Controllers/Api/Auth/RegisterController.php)
- [User.php](file:///c:/Users/rufuz/Herd/fsd_all_project/gtasign_backend/app/Models/User.php)

**Logic Snippet:**
```php
// Step 1: User fills registration form
// Step 2: System creates user with status 'inactive'
// Step 3: Send OTP to user's email
// Step 4: User submits OTP via /v1/verify-email
```

---

### 2. Secure Login (JWT)
We use JWT for the API to ensure scalability and cross-platform compatibility.

**Controller:** [LoginController.php](file:///c:/Users/rufuz/Herd/fsd_all_project/gtasign_backend/app/Http/Controllers/Api/Auth/LoginController.php)

**How it works:**
```php
public function login(Request $request)
{
    // 1. Validate Email & Password
    // 2. Check if User is 'active'
    // 3. Generate JWT Token using auth('api')
    $token = auth('api')->attempt($credentials);

    // 4. Return User Profile + Token
    return $this->success('Login successful', [
        'user'  => new UserResource($user),
        'token' => $token
    ]);
}
```

---

## 🛠️ Key Technologies

| Technology | Purpose |
| :--- | :--- |
| **Tymon/JWTAuth** | Provides the `Bearer` tokens for API security. |
| **Spatie Permission** | Manages roles (Admin, Expert, Client) and permissions. |
| **ApiResponse Trait** | Ensures consistent JSON responses across all auth endpoints. |

---

## 📂 Project Structure for Auth

- `app/Models/User.php`: The core User model implementing `JWTSubject`.
- `app/Http/Controllers/Api/Auth/`: Contains all authentication logic.
- `routes/api.php`: Defines the v1 authentication routes.

---

## 🎨 Visual Representation (Conceptual)

> [!NOTE]
> **API Login Request Example**
> **POST** `/api/v1/login`
> ```json
> {
>   "email": "expert@example.com",
>   "password": "secret_password"
> }
> ```

> [!TIP]
> **Security Best Practice**
> Always ensure the `Authorization` header is sent as `Bearer <token>` for all protected routes (like `/v1/profile` or `/v1/notifications`).

---

## 📢 Notification Integration
As part of our workflow, authentication events (like password resets or account approvals) are tied into our **Database Notification System**. This allows us to track user activity and keep them informed in real-time.

---

*Prepared by: Antigravity AI & The Development Team*
