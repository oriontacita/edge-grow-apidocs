# ============================================
# BASE URL
# ============================================
# Development: http://localhost:8000/api/
# Production : https://yourdomain.com/api/

# ============================================
# COMMON HEADERS
# ============================================
# Authorization: Bearer <token>
# Content-Type: application/json

# ============================================
# COMMON RESPONSE STRUCTURE
# ============================================
# Success:
# {
#   "success": true | boolean,
#   "message": "string" | string,
#   "data": {} | object or [] | array or null
# }
#
# Error:
# {
#   "success": false | boolean,
#   "message": "string" | string,
#   "errors": {} | object or null
# }

# ============================================
# ERROR STATUS CODES
# ============================================
# 400 - Bad Request (validasi gagal)
# 401 - Unauthorized (token tidak valid/habis)
# 403 - Forbidden (tidak punya akses)
# 404 - Not Found (resource tidak ditemukan)
# 409 - Conflict (data sudah ada, misal: username)
# 422 - Unprocessable Entity (format payload salah)
# 500 - Internal Server Error

# ============================================
# AUTHENTICATION
# ============================================

POST /api/auth/login <br>
Description: Login ke akun  <br>
Headers:
  Content-Type: application/json
Body:
{
  "username": "Ny. Sri Rahayu" | string,
  "pin": 123456 | integer
}

Response 200:
{
  "success": true | boolean,
  "message": "Login successful" | string,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." | string
}

Response 401:
{
  "success": false | boolean,
  "message": "Invalid username or PIN" | string,
  "data": null
}

# ============================================
# DASHBOARD
# ============================================

GET /api/dashboard  <br>
Description: Mendapatkan data ringkasan dashboard  <br>
Headers:
  Authorization: Bearer <token>
Response 200:
{
  "success": true | boolean,
  "message": "Dashboard data retrieved" | string,
  "data": {
    "toddlers": [
      {
        "id": 1 | integer,
        "toddler_full_name": "string" | string,
        "date_of_birth": "2024-01-01" | string,
        "gender": "male" | string
      },
      ....
    ] | array
  } | object
}
Response 401:
{
  "success": false | boolean,
  "message": "Invalid or expired token" | string,
  "data": null
}

# ============================================
# TODDLERS
# ============================================

GET /api/toddlers  <br>
Description: Mendapatkan semua data balita  <br>
Headers:
  Authorization: Bearer <token>
Query Parameters:
  - gender: string (optional: "male" | "female")
Response 200:
{
  "success": true | boolean,
  "message": "Toddlers retrieved" | string,
  "data": {
    "toddlers": [
      {
        "id": 1 | integer,
        "toddler_full_name": "Ahmad Fauzi" | string,
        "biological_mother_name": "Siti Aminah" | string,
        "date_of_birth": "2024-01-15" | string,
        "gender": "male" | string,
        "birth_weight": 3.2 | float,
        "birth_length": 50.0 | float,
        "created_at": "2024-01-20T10:00:00Z" | string,
        "updated_at": "2024-01-20T10:00:00Z" | string
      },
      .....
    ] | array
  } | object
}

---

GET /api/toddlers/view/{toddler_id}  <br>
Description: Mendapatkan detail balita berdasarkan ID  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "Toddler retrieved" | string,
  "data": {
    "toddler": {
      "id": 1 | integer,
      "toddler_full_name": "Ahmad Fauzi" | string,
      "biological_mother_name": "Siti Aminah" | string,
      "date_of_birth": "2024-01-15" | string,
      "gender": "male" | string,
      "birth_weight": 3.2 | float,
      "birth_length": 50.0 | float
    } | object
  } | object
}
Response 404:
{
  "success": false | boolean,
  "message": "Toddler not found" | string,
  "data": null
}

---

POST /api/toddlers/add  <br>
Description: Menambahkan data balita baru  <br>
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Body:
{
  "toddler_full_name": "Ahmad Fauzi" | string,
  "biological_mother_name": "Siti Aminah" | string,
  "date_of_birth": "2024-01-15" | string,
  "gender": "male" | string,
  "birth_weight": 3.2 | float,
  "birth_length": 50.0 | float
}
Response 201:
{
  "success": true | boolean,
  "message": "Toddler created successfully" | string,
  "data": {
    "toddler": {
      "id": 1 | integer,
      "toddler_full_name": "Ahmad Fauzi" | string,
      "biological_mother_name": "Siti Aminah" | string,
      "date_of_birth": "2024-01-15" | string,
      "gender": "male" | string,
      "birth_weight": 3.2 | float,
      "birth_length": 50.0 | float,
      "created_at": "2024-01-20T10:00:00Z" | string,
      "updated_at": "2024-01-20T10:00:00Z" | string
    } | object
  } | object
}
Response 422:
{
  "success": false | boolean,
  "message": "Validation error" | string,
  "errors": {} | object
}

---

PUT /api/toddlers/edit/{toddler_id}  <br>
Description: Mengubah data balita (full update)  <br>
Headers: 
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - toddler_id: integer
Body:
{
  "toddler_full_name": "Ahmad Fauzi bin Abdullah" | string,
  "biological_mother_name": "Siti Aminah" | string,
  "date_of_birth": "2024-01-15" | string,
  "gender": "male" | string,
  "birth_weight": 3.2 | float,
  "birth_length": 50.0 | float
}
Response 200:
{
  "success": true | boolean,
  "message": "Toddler updated successfully" | string,
  "data": {
    "toddler": {
      "id": 1 | integer,
      "toddler_full_name": "Ahmad Fauzi bin Abdullah" | string,
      "biological_mother_name": "Siti Aminah" | string,
      "date_of_birth": "2024-01-15" | string,
      "gender": "male" | string,
      "birth_weight": 3.2 | float,
      "birth_length": 50.0 | float,
      "created_at": "2024-01-20T10:00:00Z" | string,
      "updated_at": "2024-01-25T14:30:00Z" | string
    } | object
  } | object
}
Response 404:
{
  "success": false | boolean,
  "message": "Toddler not found" | string,
  "data": null
}

---

DELETE /api/toddlers/delete/{toddler_id}  <br>
Description: Menghapus data balita  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "Toddler deleted successfully" | string,
  "data": null
}
Response 404:
{
  "success": false | boolean,
  "message": "Toddler not found" | string,
  "data": null
}

# ============================================
# MEASUREMENTS
# ============================================

GET /api/toddlers/{toddler_id}/measurements  <br>
Description: Mendapatkan semua pengukuran balita tertentu  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "Measurements retrieved" | string,
  "data": {
    "toddler_id": 1 | integer,
    "measurements": [
      {
        "id": 1 | integer,
        "measurement_date": "2024-06-15" | string,
        "current_age": 5 | integer,
        "current_weight": 7.5 | float,
        "current_length": 65.0 | float,
        "breastfeeding": "exclusive" | string,
        "created_at": "2024-06-15T10:00:00Z" | string
      }
    ] | array
  } | object
}

---

GET /api/measurements/{measurement_id}/detail  <br>
Description: Mendapatkan detail pengukuran berdasarkan ID  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - measurement_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "Measurement retrieved" | string,
  "data": {
    "measurement": {
      "id": 1 | integer,
      "toddler_id": 1 | integer,
      "toddler_name": "Ahmad Fauzi" | string,
      "measurement_date": "2024-06-15" | string,
      "current_age": 5 | integer,
      "current_weight": 7.5 | float,
      "current_length": 65.0 | float,
      "breastfeeding": "exclusive" | string,
      "weight_status": "normal" | string,
      "length_status": "normal" | string,
      "created_at": "2024-06-15T10:00:00Z" | string,
      "updated_at": "2024-06-15T10:00:00Z" | string
    } | object
  } | object
}
Response 404:
{
  "success": false | boolean,
  "message": "Measurement not found" | string,
  "data": null
}

---

POST /api/toddlers/{toddler_id}/measurements/add  <br>
Description: Menambahkan pengukuran baru untuk balita tertentu  <br>
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - toddler_id: integer
Body:
{
  "measurement_date": "2024-06-15" | string,
  "current_weight": 7.5 | float,
  "current_length": 65.0 | float,
  "breastfeeding": "exclusive" | string
}
Response 201:
{
  "success": true | boolean,
  "message": "Measurement created successfully" | string,
  "data": {
    "measurement": {
      "id": 1 | integer,
      "toddler_id": 1 | integer,
      "measurement_date": "2024-06-15" | string,
      "current_age": 5 | integer,
      "current_weight": 7.5 | float,
      "current_length": 65.0 | float,
      "breastfeeding": "exclusive" | string
    } | object
  } | object
}
Response 404:
{
  "success": false | boolean,
  "message": "Toddler not found" | string,
  "data": null
}
Response 422:
{
  "success": false | boolean,
  "message": "Validation error" | string,
  "errors": {} | object
}

---

PUT /api/measurements/edit/{measurement_id}  <br>
Description: Mengubah data pengukuran  <br>
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - measurement_id: integer
Body:
{
  "measurement_date": "2024-06-15" | string,
  "current_weight": 7.8 | float,
  "current_length": 65.5 | float,
  "breastfeeding": "partial" | string
}
Response 200:
{
  "success": true | boolean,
  "message": "Measurement updated successfully" | string,
  "data": {
    "measurement": {
      "id": 1 | integer,
      "toddler_id": 1 | integer,
      "measurement_date": "2024-06-15" | string,
      "current_age": 5 | integer,
      "current_weight": 7.8 | float,
      "current_length": 65.5 | float,
      "breastfeeding": "partial" | string,
      "created_at": "2024-06-15T10:00:00Z" | string,
      "updated_at": "2024-06-20T14:30:00Z" | string
    } | object
  } | object
}

---

DELETE /api/measurements/delete/{measurement_id}  <br>
Description: Menghapus data pengukuran  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - measurement_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "Measurement deleted successfully" | string,
  "data": null
}
Response 404:
{
  "success": false | boolean,
  "message": "Measurement not found" | string,
  "data": null
}

# ============================================
# USERS
# ============================================

GET /api/users  <br>
Description: Mendapatkan semua data user  <br>
Headers:
  Authorization: Bearer <token>
Query Parameters:
  - gender: string (optional: "male" | "female")
Response 200:
{
  "success": true | boolean,
  "message": "Users retrieved" | string,
  "data": {
    "users": [
      {
        "id": 1 | integer,
        "full_name": "Siti Aminah" | string,
        "username": "Siti Aminah" | string,
        "pin": "123" | string,
        "gender": "male" | string
      },
      .....
    ] | array
  } | object
}

---

GET /api/users/view/{user_id}  <br>
Description: Mendapatkan detail user berdasarkan ID  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - user_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "User retrieved" | string,
  "data": {
    "user": {
      "id": 1 | integer,
      "full_name": "Siti Aminah" | string,
      "username": "Siti Aminah" | string,
      "pin": "123" | string,
      "gender": "male" | string
    } | object
  } | object
}
Response 404:
{
  "success": false | boolean,
  "message": "User not found" | string,
  "data": null
}

---

POST /api/users/add  <br>
Description: Menambahkan data user baru  <br>
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Body:
{
  "full_name": "Siti Aminah" | string,
  "username": "Siti Aminah" | string,
  "pin": "123" | string,
  "gender": "male" | string
}
Response 201:
{
  "success": true | boolean,
  "message": "User created successfully" | string,
  "data": {
    "user": {
      "id": 1 | integer,
      "full_name": "Siti Aminah" | string,
      "username": "Siti Aminah" | string,
      "pin": "123" | string,
      "gender": "male" | string,
      "created_at": "2024-01-20T10:00:00Z" | string,
      "updated_at": "2024-01-20T10:00:00Z" | string
    } | object
  } | object
}
Response 422:
{
  "success": false | boolean,
  "message": "Validation error" | string,
  "errors": {} | object
}

---

PUT /api/users/edit/{user_id}  <br>
Description: Mengubah data user (full update)  <br>
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - user_id: integer
Body:
{
  "full_name": "Siti Aminah" | string,
  "username": "Siti Aminah" | string,
  "pin": "123" | string,
  "gender": "male" | string
}
Response 200:
{
  "success": true | boolean,
  "message": "User updated successfully" | string,
  "data": {
    "user": {
      "id": 1 | integer,
      "full_name": "Siti Aminah" | string,
      "username": "Siti Aminah" | string,
      "pin": "123" | string,
      "gender": "male" | string,
      "created_at": "2024-01-20T10:00:00Z" | string,
      "updated_at": "2024-01-25T14:30:00Z" | string
    } | object
  } | object
}
Response 404:
{
  "success": false | boolean,
  "message": "User not found" | string,
  "data": null
}

---

DELETE /api/users/delete/{user_id}  <br>
Description: Menghapus data user  <br>
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - user_id: integer
Response 200:
{
  "success": true | boolean,
  "message": "User deleted successfully" | string,
  "data": null
}
Response 404:
{
  "success": false | boolean,
  "message": "User not found" | string,
  "data": null
}
