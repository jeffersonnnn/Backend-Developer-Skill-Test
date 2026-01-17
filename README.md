# School Management System - Backend Skill Test

A comprehensive full-stack web application for managing school operations including students, staff, classes, notices, and leave management.

## Skill Test Completion

### Task: Complete CRUD Operations in Student Management

**Location:** `backend/src/modules/students/students-controller.js`

**Implemented Functions:**

| Function | HTTP Method | Endpoint | Description |
|----------|-------------|----------|-------------|
| `handleGetAllStudents` | GET | `/api/v1/students` | List all students with optional filtering |
| `handleAddStudent` | POST | `/api/v1/students` | Create a new student record |
| `handleGetStudentDetail` | GET | `/api/v1/students/:id` | Retrieve student details by ID |
| `handleUpdateStudent` | PUT | `/api/v1/students/:id` | Update an existing student |
| `handleStudentStatus` | POST | `/api/v1/students/:id/status` | Toggle student active/inactive status |

### Implementation Details

```javascript
// GET /api/v1/students - List students with filtering
const handleGetAllStudents = asyncHandler(async (req, res) => {
    const { name, className, section, roll } = req.query;
    const payload = { name, className, section, roll };
    const students = await getAllStudents(payload);
    res.json(students);
});

// POST /api/v1/students - Create new student
const handleAddStudent = asyncHandler(async (req, res) => {
    const payload = req.body;
    const result = await addNewStudent(payload);
    res.status(201).json(result);
});

// GET /api/v1/students/:id - Get student details
const handleGetStudentDetail = asyncHandler(async (req, res) => {
    const { id } = req.params;
    const student = await getStudentDetail(id);
    res.json(student);
});

// PUT /api/v1/students/:id - Update student
const handleUpdateStudent = asyncHandler(async (req, res) => {
    const { id } = req.params;
    const payload = { ...req.body, id };
    const result = await updateStudent(payload);
    res.json(result);
});

// POST /api/v1/students/:id/status - Change student status
const handleStudentStatus = asyncHandler(async (req, res) => {
    const { id: userId } = req.params;
    const { status } = req.body;
    const reviewerId = req.user?.id;
    const result = await setStudentStatus({ userId, reviewerId, status });
    res.json(result);
});
```

---

## Project Architecture

```
skill-test/
├── frontend/           # React + TypeScript + Material-UI
├── backend/            # Node.js + Express + PostgreSQL
├── seed_db/            # Database schema and seed data
└── README.md
```

## Tech Stack

### Backend
- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** PostgreSQL
- **Authentication:** JWT + CSRF protection
- **Password Hashing:** Argon2
- **Validation:** Zod

### Frontend
- **Framework:** React 18 + TypeScript
- **UI Library:** Material-UI (MUI) v6
- **State Management:** Redux Toolkit + RTK Query
- **Form Handling:** React Hook Form + Zod
- **Build Tool:** Vite

## Quick Start

### Prerequisites
- Node.js (v16 or higher)
- PostgreSQL (v12 or higher)
- npm or yarn

### Database Setup

```bash
# Create PostgreSQL database
createdb school_mgmt

# Run database migrations
psql -d school_mgmt -f seed_db/tables.sql
psql -d school_mgmt -f seed_db/seed-db.sql
```

### Backend Setup

```bash
cd backend
npm install
cp .env.example .env  # Configure your environment variables
npm start
```

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

### Access the Application

| Service | URL |
|---------|-----|
| Frontend | http://localhost:5173 |
| Backend API | http://localhost:5007 |

### Demo Credentials

| Field | Value |
|-------|-------|
| Email | admin@school-admin.com |
| Password | 3OU4zn3q6Zh9 |

## API Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/auth/login` | User login |
| POST | `/api/v1/auth/logout` | User logout |
| GET | `/api/v1/auth/refresh` | Refresh access token |

### Students (Skill Test Focus)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/students` | List all students |
| POST | `/api/v1/students` | Create new student |
| GET | `/api/v1/students/:id` | Get student by ID |
| PUT | `/api/v1/students/:id` | Update student |
| POST | `/api/v1/students/:id/status` | Change student status |

### Query Parameters for GET /api/v1/students

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Filter by student name |
| `className` | string | Filter by class name |
| `section` | string | Filter by section |
| `roll` | string | Filter by roll number |

## Testing the Implementation

### Using cURL

```bash
# Get all students
curl -X GET http://localhost:5007/api/v1/students \
  -H "Authorization: Bearer <token>"

# Get students filtered by class
curl -X GET "http://localhost:5007/api/v1/students?className=10A" \
  -H "Authorization: Bearer <token>"

# Get student by ID
curl -X GET http://localhost:5007/api/v1/students/1 \
  -H "Authorization: Bearer <token>"

# Create new student
curl -X POST http://localhost:5007/api/v1/students \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com", ...}'

# Update student
curl -X PUT http://localhost:5007/api/v1/students/1 \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"basicDetails": {"name": "John Updated", "email": "john@example.com"}}'

# Change student status
curl -X POST http://localhost:5007/api/v1/students/1/status \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"status": false}'
```

## Project Structure

```
backend/src/
├── config/           # Database and app configuration
├── middlewares/      # Express middlewares (auth, CSRF, error handling)
├── modules/
│   ├── auth/         # Authentication endpoints
│   ├── students/     # Student CRUD operations ← SKILL TEST
│   ├── classes/      # Class management
│   ├── sections/     # Section management
│   ├── notices/      # Notice system
│   └── ...
├── shared/           # Shared utilities and repositories
├── templates/        # Email templates
└── utils/            # Helper functions
```

## Skills Demonstrated

- ✅ Node.js & Express.js REST API development
- ✅ PostgreSQL database integration
- ✅ Clean controller-service-repository architecture
- ✅ Proper HTTP status codes (200, 201, 404, 500)
- ✅ Query parameter handling for filtering
- ✅ Request body and URL parameter extraction
- ✅ Async/await error handling with express-async-handler
- ✅ RESTful API design principles

## License

MIT
