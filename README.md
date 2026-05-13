<div align="center">

<img src="public/logo.png" alt="SMP Kristen Tabgha" width="180" />

# Tabgha CBT
### Academic Computer-Based Test System

**SMP Kristen Tabgha · Cerdas | Unggul | Beriman**

[![Next.js](https://img.shields.io/badge/Next.js-16.2.1-black?style=flat-square&logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.5-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Prisma](https://img.shields.io/badge/Prisma-5.21.1-2D3748?style=flat-square&logo=prisma)](https://prisma.io)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)](https://mysql.com)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-v4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)

*A high-performance, web-based assessment platform engineered exclusively for Tabgha Academic institutions.*

</div>

---

## 📋 Overview

Tabgha CBT is a full-stack web application that digitizes the entire examination workflow — from question creation to result reporting — for SMP Kristen Tabgha. Built on a modern TypeScript stack with strict role-based access control, automated anti-cheat detection, and high-volume data tools.

## ✨ Features

### 👥 Role-Based Access Control (RBAC)
| Role | Access |
|------|--------|
| **Admin** | Full system control — manage users, settings, monitor all exams |
| **Teacher** | Create subjects, question banks, exams; export results |
| **Student** | Join exams by token; view personal history |

### 📝 Exam Engine
- Multiple Choice & Essay question types
- Native image and video URL support per question
- Question bank reusable across multiple exams
- Token-based exam access (8-character unique code)
- Live countdown timer with auto-submit on expiry
- Question navigator sidebar with answered/unanswered indicators
- Auto-save answers every interaction

### 🔒 Anti-Cheat System
- **Forced fullscreen** — exam launches in fullscreen, cannot be exited until submission
- **Tab switch detection** — logs every visibility change
- **Window blur detection** — logs every focus loss
- **Keyboard shortcut blocking** — Ctrl+T, Ctrl+W, Ctrl+N, F5, Escape, Alt+F4 all blocked
- **Right-click disabled** during exam
- **Copy/paste disabled** during exam
- **Close tab warning** — browser confirmation dialog on exit attempt
- All violations logged to `CheatLog` with timestamp — exam is **not terminated**, allowing post-exam review by teacher

### 📊 Data Tools
- **CSV bulk import** — import thousands of students at once
- **Excel export** — download exam results as `.xlsx`
- **PDF export** — structured diagnostic reports as `.pdf`

### 🎨 UI / UX
- School logo watermark on all authenticated pages
- Responsive sidebar navigation per role
- Clean card-based dashboard with live statistics

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16.2.1 (App Router) |
| Language | TypeScript 5.5 |
| UI | React 18 + Tailwind CSS v4 |
| Database | MySQL 8.0 |
| ORM | Prisma 5.21.1 |
| Auth | JWT (jsonwebtoken) + bcryptjs |
| CSV Import | papaparse |
| Excel Export | exceljs |
| PDF Export | jspdf + jspdf-autotable |

---

## 🚀 Getting Started

### Requirements
- Node.js `>= 18.0.0` (recommended: v20 LTS)
- MySQL `>= 8.0`

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Dalon0209/tabgha-cbt.git
cd tabgha-cbt

# 2. Install dependencies
npm install

# 3. Configure environment
cp .env.example .env
```

Edit `.env` with your MySQL credentials:
```env
DATABASE_URL="mysql://root:YOUR_PASSWORD@localhost:3306/tabgha_cbt"
JWT_SECRET="your-long-random-secret-key"
```

```bash
# 4. Create database (MySQL)
mysql -u root -p
> CREATE DATABASE tabgha_cbt;
> exit;

# 5. Push schema to database
npx prisma generate
npx prisma db push

# 6. Seed demo data (optional)
npx ts-node prisma/seed.ts

# 7. Start development server
npm run dev
```

Open **http://localhost:3003** in your browser.

### Production Build
```bash
npm run build
npm run start
```

---

## 🔐 Default Accounts

> After running the seeder (`npx ts-node prisma/seed.ts`)

| Role | Username | Password |
|------|----------|----------|
| Admin | `admin` | `admin123` |
| Guru | `guru_matematika` | `guru123` |
| Siswa | `siswa_001` | `siswa123` |
| Siswa | `siswa_002` | `siswa123` |
| Siswa | `siswa_003` | `siswa123` |

---

## 📁 Project Structure

```
tabgha-cbt/
├── prisma/
│   ├── schema.prisma        # Database schema
│   └── seed.ts              # Demo data seeder
├── public/
│   └── logo.png             # School logo (watermark)
└── src/
    ├── app/
    │   ├── api/             # REST API routes
    │   │   ├── auth/        # Login, logout
    │   │   ├── admin/       # Admin endpoints
    │   │   ├── teacher/     # Teacher endpoints
    │   │   └── student/     # Student endpoints
    │   ├── admin/           # Admin pages
    │   ├── teacher/         # Teacher pages
    │   ├── student/         # Student pages
    │   ├── exam/            # Exam taking pages
    │   └── login/           # Login page
    ├── components/
    │   ├── admin/           # AdminSidebar
    │   ├── teacher/         # TeacherSidebar
    │   ├── student/         # StudentSidebar
    │   └── Watermark.tsx    # School logo watermark
    ├── lib/
    │   ├── prisma.ts        # DB client singleton
    │   └── auth.ts          # JWT helpers
    └── types/
        └── index.ts         # Shared TypeScript types
```

---

## 📥 CSV Import Format

For bulk student import via **Admin → Manajemen Pengguna → Import CSV**.

Save as `.csv` using **Notepad** (not Excel):

```
username,password,name,role,classRoom
siswa_004,password123,Andi Wijaya,STUDENT,7A
siswa_005,password123,Dewi Lestari,STUDENT,7B
guru_ipa,password123,Dr. Sari Dewi,TEACHER,
```

> ⚠️ Always use **Notepad** to save CSV files. Excel adds unwanted quote characters automatically.

---

## 🗃️ Database Schema

```
SystemSetting   — School name, active term (single row)
User            — Admin / Teacher / Student accounts
Subject         — Subjects created by teachers
Question        — Question bank (Multiple Choice + Essay)
Exam            — Exam container (DRAFT → PUBLISHED → CLOSED)
ExamQuestion    — Many-to-many join: Exam ↔ Question
ExamAttempt     — Student attempt with answers + score
CheatLog        — Anti-cheat violation log
```

---

## 📜 License

Private — SMP Kristen Tabgha Academic Use Only © 2024

---

<div align="center">
  <sub>Built with ❤️ for SMP Kristen Tabgha · Est. 2007</sub>
</div>
