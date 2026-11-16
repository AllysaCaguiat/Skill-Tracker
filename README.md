# SKILLTRACK — Online Learning and Assessment System

Full-stack **React + Vite**, **Node.js + Express**, and **MySQL** application. MySQL runs in **XAMPP**; React and Node.js run with npm (XAMPP does not run the Node server).

## Features
- Student registration and sign-in; secure bcrypt password hashes and JWT sessions
- Three roles: **student**, **instructor**, **admin**
- Courses: catalog, search, categories, creation, edit, publish/unpublish, deletion
- Instructor lesson management, including optional video links
- Student enrollment, lessons, completion tracking, dashboard, progress percentages
- Multiple-choice quizzes, automatic grading, passing score, repeat attempts
- Admin user listing and role assignment
- Responsive desktop and mobile UI

## Requirements
- Node.js **20+** and npm
- XAMPP with **MySQL / MariaDB** running (Apache is optional)
- A browser

## Setup

### 1. Start MySQL in XAMPP
Open the XAMPP Control Panel and click **Start** next to **MySQL**. Open `http://localhost/phpmyadmin` (start Apache if using phpMyAdmin). Click **Import**, select `server/sql/skilltrack.sql`, and import it. The SQL file creates the `skilltrack` database and all tables. Alternatively run the SQL using the MySQL command line.

### 2. Configure and start the backend
From the project folder:

```bash
cd server
cp .env.example .env
npm install
npm run seed
npm run dev
```

On Windows Command Prompt use `copy .env.example .env` instead of `cp`.

**IMPORTANT:** In `server/.env`, replace `JWT_SECRET` with your own unpredictable value of at least 32 characters. The `DB_USER=root` and blank `DB_PASSWORD` defaults work only with a default local XAMPP installation. Update them if you have configured a MySQL password or port.

The API runs at `http://localhost:5000`; verify `http://localhost:5000/api/health` displays a successful connection.

### 3. Start the frontend
Open a **second terminal** from the project folder:

```bash
cd client
cp .env.example .env
npm install
npm run dev
```

On Windows Command Prompt use `copy .env.example .env` instead of `cp`. Open `http://localhost:5173`.

### Demo accounts (created by `npm run seed`)
| Role | Email | Password |
| --- | --- | --- |
| Admin | `admin@skilltrack.test` | `Password123!` |
| Instructor | `teacher@skilltrack.test` | `Password123!` |
| Student | `student@skilltrack.test` | `Password123!` |

The seed also creates an introductory web-development course, three lessons, and a 3-question assessment. Re-running it does not duplicate the sample course. Change the passwords and remove demo users before real deployment.

### Workflow
1. Sign in as admin to promote an account to instructor via **User Management**.
2. An instructor creates a draft course, adds lessons and quiz questions, and checks **Publish** then saves.
3. A student browses the catalog, enrolls, views lessons, marks them complete, and takes the quiz.
4. Dashboard and **My Learning** show progress and stats.

## API overview
`POST /api/auth/register`, `POST /api/auth/login`, `GET /api/auth/me`  
`GET /api/dashboard`, `GET /api/courses`, `POST /api/courses`, `GET/PATCH/DELETE /api/courses/:id`  
`POST /api/courses/:id/enroll`, `GET /api/learning`  
`POST /api/courses/:id/lessons`, `GET/PATCH/DELETE /api/lessons/:id`, `POST /api/lessons/:id/complete`  
`GET/PUT /api/courses/:id/quiz`, `POST /api/courses/:id/quiz/submit`  
`GET /api/admin/users`, `PATCH /api/admin/users/:id/role`

All application endpoints except registration, login and health require `Authorization: Bearer <token>`. Instructor/admin edits enforce ownership, and quiz answer keys are not returned to students.

## Notes
- This is a locally runnable project starter, not a hosted service. You must install dependencies and start XAMPP MySQL.
- `npm run build` in `client/` creates the frontend production bundle in `dist/`.
- Production deployment additionally requires HTTPS, appropriate CORS settings, real credentials, backups, monitoring, and rate limiting beyond local development settings.
