# ============================================
# BASE URL
# ============================================
# Development: http://localhost:8000/api/v1
# Production : https://yourdomain.com/api/v1

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
#   "success": true,
#   "message": "string",
#   "data": {} or []
# }
#
# Error:
# {
#   "success": false,
#   "message": "string",
#   "errors": {} or null
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

POST /api/auth/register
Description: Mendaftar akun baru
Headers:
  Content-Type: application/json
Body:
{
  "username": "string (3-50 karakter)",
  "pin": 123456
}
Response 201:
{
  "message": "Registration successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
Response 409:
{
  "message": "Username or email already exists",
  "data": null
}
Response 422:
{
  "message": "Validation error",
}

---

POST /api/auth/login
Description: Login ke akun
Headers:
  Content-Type: application/json
Body:
{
  "username": "string",
  "pin": 123456
}
Response 200:
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
Response 401:
{
  "message": "Invalid username or PIN",
  "data": null
}

# ============================================
# DASHBOARD
# ============================================

GET /api/dashboard
Description: Mendapatkan data ringkasan dashboard
Headers:
  Authorization: Bearer <token>
Response 200:
{
  "message": "Dashboard data retrieved",
  "data": {
    "toddlers": [
      {
        "id": 1,
        "toddler_full_name": "string",
        "date_of_birth": "2024-01-01",
        "gender": "male"
      },
      ....
    ]
  }
}
Response 401:
{
  "success": false,
  "message": "Invalid or expired token",
  "data": null
}

# ============================================
# TODDLERS
# ============================================

GET /api/toddlers
Description: Mendapatkan semua data balita
Headers:
  Authorization: Bearer <token>
Query Parameters:
  - gender: string (optional: "male" | "female")
Response 200:
{
  "success": true,
  "message": "Toddlers retrieved",
  "data": {
    "toddlers": [
      {
        "id": 1,
        "toddler_full_name": "Ahmad Fauzi",
        "biological_mother_name": "Siti Aminah",
        "date_of_birth": "2024-01-15",
        "gender": "male",
        "birth_weight": 3.2,
        "birth_length": 50.0,
        "created_at": "2024-01-20T10:00:00Z",
        "updated_at": "2024-01-20T10:00:00Z"
      },
      .....
    ]
  }
}

---

GET /api/toddlers/view/{toddler_id}
Description: Mendapatkan detail balita berdasarkan ID
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "message": "Toddler retrieved",
  "data": {
    "toddler": {
      "id": 1,
      "toddler_full_name": "Ahmad Fauzi",
      "biological_mother_name": "Siti Aminah",
      "date_of_birth": "2024-01-15",
      "gender": "male",
      "birth_weight": 3.2,
      "birth_length": 50.0,
    }
  }
}
Response 404:
{
  "message": "Toddler not found",
  "data": null
}

---

POST /api/toddlers/add
Description: Menambahkan data balita baru
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Body:
{
  "toddler_full_name": "Ahmad Fauzi",
  "biological_mother_name": "Siti Aminah",
  "date_of_birth": "2024-01-15",
  "gender": "male",
  "birth_weight": 3.2,
  "birth_length": 50.0
}
Response 201:
{
  "success": true,
  "message": "Toddler created successfully",
  "data": {
    "toddler": {
      "id": 1,
      "toddler_full_name": "Ahmad Fauzi",
      "biological_mother_name": "Siti Aminah",
      "date_of_birth": "2024-01-15",
      "gender": "male",
      "birth_weight": 3.2,
      "birth_length": 50.0,
      "created_at": "2024-01-20T10:00:00Z",
      "updated_at": "2024-01-20T10:00:00Z"
    }
  }
}
Response 422:
{
  "message": "Validation error",
}

---

PUT /api/toddlers/edit/{toddler_id}
Description: Mengubah data balita (full update)
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - toddler_id: integer
Body:
{
  "toddler_full_name": "Ahmad Fauzi bin Abdullah",
  "biological_mother_name": "Siti Aminah",
  "date_of_birth": "2024-01-15",
  "gender": "male",
  "birth_weight": 3.2,
  "birth_length": 50.0
}
Response 200:
{
  "message": "Toddler updated successfully",
  "data": {
    "toddler": {
      "id": 1,
      "toddler_full_name": "Ahmad Fauzi bin Abdullah",
      "biological_mother_name": "Siti Aminah",
      "date_of_birth": "2024-01-15",
      "gender": "male",
      "birth_weight": 3.2,
      "birth_length": 50.0,
      "created_at": "2024-01-20T10:00:00Z",
      "updated_at": "2024-01-25T14:30:00Z"
    }
  }
}
Response 404:
{
  "message": "Toddler not found",
  "data": null
}

---

DELETE /api/toddlers/delete/{toddler_id}
Description: Menghapus data balita
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "message": "Toddler deleted successfully",
  "data": null
}
Response 404:
{,
  "message": "Toddler not found",
  "data": null
}

# ============================================
# MEASUREMENTS
# ============================================

GET /api/toddlers/{toddler_id}/measurements
Description: Mendapatkan semua pengukuran balita tertentu
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "success": true,
  "message": "Measurements retrieved",
  "data": {
    "toddler_id": 1,
    "measurements": [
      {
        "id": 1,
        "measurement_date": "2024-06-15",
        "current_age": 5,
        "current_weight": 7.5,
        "current_length": 65.0,
        "breastfeeding": "exclusive",
        "created_at": "2024-06-15T10:00:00Z"
      }
    ]
  }
}

---

GET /api/measurements/{measurement_id}/detail
Description: Mendapatkan detail pengukuran berdasarkan ID
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - measurement_id: integer
Response 200:
{
  "message": "Measurement retrieved",
  "data": {
    "measurement": {
      "id": 1,
      "toddler_id": 1,
      "toddler_name": "Ahmad Fauzi",
      "measurement_date": "2024-06-15",
      "current_age": 5,
      "current_weight": 7.5,
      "current_length": 65.0,
      "breastfeeding": "exclusive",
      "weight_status": "normal",
      "length_status": "normal",
      "created_at": "2024-06-15T10:00:00Z",
      "updated_at": "2024-06-15T10:00:00Z"
    }
  }
}
Response 404:
{
  "message": "Measurement not found",
  "data": null
}

---

POST /api/toddlers/{toddler_id}/measurements/add
Description: Menambahkan pengukuran baru untuk balita tertentu
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - toddler_id: integer
Body:
{
  "measurement_date": "2024-06-15",
  "current_weight": 7.5,
  "current_length": 65.0,
  "breastfeeding": "exclusive"
}
Response 201:
{
  "message": "Measurement created successfully",
  "data": {
    "measurement": {
      "id": 1,
      "toddler_id": 1,
      "measurement_date": "2024-06-15",
      "current_age": 5,
      "current_weight": 7.5,
      "current_length": 65.0,
      "breastfeeding": "exclusive",
    }
  }
}
Response 404:
{
  "success": false,
  "message": "Toddler not found",
  "data": null
}
Response 422:
{
  "message": "Validation error",
}

---

PUT /api/measurements/edit/{measurement_id}
Description: Mengubah data pengukuran
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - measurement_id: integer
Body:
{
  "measurement_date": "2024-06-15",
  "current_weight": 7.8,
  "current_length": 65.5,
  "breastfeeding": "partial"
}
Response 200:
{
  "message": "Measurement updated successfully",
  "data": {
    "measurement": { ... }
  }
}

---

DELETE /api/measurements/{measurement_id}/delete
Description: Menghapus data pengukuran
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - measurement_id: integer
Response 200:
{
  "message": "Measurement deleted successfully",
  "data": null
}
Response 404:
{
  "message": "Measurement not found",
  "data": null
}

# ============================================
# USERS
# ============================================

GET /api/users
Description: Mendapatkan semua data user
Headers:
  Authorization: Bearer <token>
Query Parameters:
)
  - gender: string (optional: "male" | "female")
Response 200:
{
  "message": "users retrieved",
  "data": {
    "users": [
      {
        "id": 1,
        "full_name": "Siti Aminah",
        "username": "Siti Aminah",
        "pin": "123",
        "gender": "male",
      },
      .....
    ]
  }
}

---

GET /api/users/view/{toddler_id}
Description: Mendapatkan detail user berdasarkan ID
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - user_id: integer
Response 200:
{
  "success": true,
  "message": "user retrieved",
  "data": {
    "user": {
        "id": 1,
        "full_name": "Siti Aminah",
        "username": "Siti Aminah",
        "pin": "123",
        "gender": "male",
    }
  }
}
Response 404:
{
  "message": "user not found",
  "data": null
}

---

POST /api/userss/add
Description: Menambahkan data user baru
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Body:
{
  id": 1,
        "full_name": "Siti Aminah",
        "username": "Siti Aminah",
        "pin": "123",
        "gender": "male",
}
Response 201:
{
  "success": true,
  "message": "user created successfully",
}
Response 422:
{
  "message": "Validation error",
}

---

PUT /api/users/edit/{user_id}
Description: Mengubah data user (full update)
Headers:
  Authorization: Bearer <token>
  Content-Type: application/json
Path Parameters:
  - toddler_id: integer
Body:
{
        "id": 1,
        "full_name": "Siti Aminah",
        "username": "Siti Aminah",
        "pin": "123",
        "gender": "male",
}
Response 200:
{
  
  "message": "user updated successfully",
  "data": {
    "user": {
        "id": 1,
        "full_name": "Siti Aminah",
        "username": "Siti Aminah",
        "pin": "123",
        "gender": "male",
    }
  }
}
Response 404:
{
  "message": "Toddler not found",
  "data": null
}

---

DELETE /api/uses/delete/{user_id}
Description: Menghapus data user
Headers:
  Authorization: Bearer <token>
Path Parameters:
  - toddler_id: integer
Response 200:
{
  "message": "user deleted successfully",
  "data": null
}
Response 404:
{
  "message": "user not found",
  "data": null
}
