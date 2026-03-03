# Univote — University Student Voting System

Univote is a full-stack digital voting platform built for university student council elections. It replaces traditional paper-based voting with a secure, transparent, and easy-to-use system that covers everything from candidate registration to real-time result statistics.

---

## Table of Contents

1. [Introduction](#introduction)
2. [How It Works](#how-it-works)
3. [High-Level Architecture](#high-level-architecture)
4. [Tech Stack](#tech-stack)
5. [Prerequisites](#prerequisites)
6. [Setup Instructions](#setup-instructions)
   - [Frontend](#frontend)
   - [Backend](#backend)
   - [Scanner App (Mobile)](#scanner-app-mobile)
7. [Running the Project](#running-the-project)
   - [Frontend](#frontend-1)
   - [Backend](#backend-1)
   - [Scanner App](#scanner-app)

---

## Introduction

Univote is a three-part system consisting of a **web frontend** for students, a **Laravel REST API backend**, and a **React Native mobile scanner app** for polling station staff. Together they create a complete end-to-end election workflow — from student sign-up and candidate application to QR-code-based voter verification and live result tracking.

Key capabilities:
- 🎓 **Student identity verification** using university student IDs
- 📧 **OTP-based authentication** to confirm student email before voting
- 🏛️ **Candidate application portal** where eligible students can register to run
- 🗳️ **Secure digital voting** with duplicate-vote prevention
- 📷 **QR-code voter passes** generated for each eligible voter
- 📱 **Mobile scanner app** for polling staff to verify voters on-site
- 📊 **Live election statistics** visible to all students after polls close
- 🔑 **Admin dashboard** to manage elections, approve candidates, and upload by-laws

---

## How It Works

The election process follows these steps:

1. **Student Registration**  
   A student visits the web app and signs up using their university student ID. The system verifies the ID against the university database. An OTP is sent to their registered email for identity confirmation.

2. **Candidate Application**  
   Any verified student can apply to become a candidate through the *Candidate Apply* page. The admin reviews and approves or rejects applications.

3. **Voter QR Code Generation**  
   Once a student is verified and the election is open, they receive a unique QR code voter pass that is tied to their identity.

4. **On-Site Voter Verification (Scanner App)**  
   At the physical polling station, an election staff member uses the **React Native Scanner app** to scan a student's QR code. The backend confirms the voter's eligibility and prevents double-voting.

5. **Casting a Vote**  
   After physical verification, the student casts their vote via the web app by selecting their preferred candidate. The vote is recorded securely in the database.

6. **Results & Statistics**  
   Once the election ends, the admin closes the election and the results/statistics page becomes available, showing vote counts per candidate.

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Students / Voters                     │
└───────────────┬──────────────────────────┬──────────────────┘
                │  Web Browser             │  Physical Polling Station
                ▼                          ▼
┌───────────────────────┐      ┌───────────────────────┐
│   Web Frontend        │      │  Scanner Mobile App   │
│   (React + Vite)      │      │  (React Native/Expo)  │
│                       │      │                       │
│  • Login / Sign Up    │      │  • Admin Login        │
│  • Candidate Apply    │      │  • QR Code Scanner    │
│  • Vote Page          │      │  • Voter Verification │
│  • Statistics         │      └──────────┬────────────┘
│  • Profile            │                 │
└──────────┬────────────┘                 │
           │  REST API (HTTP/JSON)         │
           └──────────────┬───────────────┘
                          ▼
           ┌──────────────────────────────┐
           │     Laravel Backend API      │
           │       (PHP / Laravel)        │
           │                              │
           │  • Student Verification      │
           │  • OTP Authentication        │
           │  • Candidate Management      │
           │  • Vote Processing           │
           │  • QR Code Generation        │
           │  • Election Management       │
           │  • Statistics Endpoint       │
           └──────────────┬───────────────┘
                          ▼
           ┌──────────────────────────────┐
           │         MySQL Database       │
           │  • Users / Students          │
           │  • Candidates / Elections    │
           │  • Votes / Voters            │
           └──────────────────────────────┘
```

---

## Tech Stack

| Component        | Technology              |
|-----------------|-------------------------|
| Web Frontend     | React 18, Vite          |
| Backend API      | Laravel 11 (PHP ≥ 8.2)  |
| Mobile Scanner   | React Native, Expo      |
| Database         | MySQL                   |
| Auth             | Laravel Sanctum + OTP   |
| Styling          | Tailwind CSS            |

---

## Prerequisites

Ensure you have the following installed on your machine:

| Requirement | Purpose |
|---|---|
| **Node.js** (v18+) | Frontend & Scanner app |
| **npm** or **yarn** | Package management |
| **PHP** (≥ 8.2) | Backend |
| **Composer** | PHP dependency management |
| **MySQL** | Database (via XAMPP or standalone) |
| **XAMPP** *(or equivalent)* | Easy MySQL + Apache setup |
| **Expo CLI** | Running the React Native scanner app |
| **Android/iOS device or emulator** | Testing the Scanner app |

## Setup Instructions

**1. Clone the Repository**

```bash
git clone https://github.com/Thizh/univote.git
cd univote
```

---

### Frontend

**2. Navigate to the frontend folder and install dependencies**

```bash
cd univote_frontend
npm install
# or
yarn install
```

**3. Configure environment**

Create a `.env` file in `univote_frontend/` and set the backend URL:

```env
VITE_API_BASE_URL=http://localhost:8000
```

---

### Backend

**4. Navigate to the backend folder and install dependencies**

```bash
cd univote_backend
composer install
```

**5. Configure environment**

Copy the example environment file and update it with your database credentials:

```bash
cp .env.example .env
```

Edit `.env` and set:

```env
DB_DATABASE=univote
DB_USERNAME=root
DB_PASSWORD=
```

**6. Start MySQL**

Ensure your MySQL server is running (start it via XAMPP Control Panel or your system service).

**7. Create the database**

Create a database named `univote` in your MySQL server (via phpMyAdmin or MySQL CLI):

```sql
CREATE DATABASE univote;
```

**8. Generate application key**

```bash
php artisan key:generate
```

**9. Run migrations**

```bash
php artisan migrate
```

---

### Scanner App (Mobile)

**10. Navigate to the Scanner folder and install dependencies**

```bash
cd Scanner
npm install
```

**11. Configure the backend URL**

Open `Scanner/baseurl.js` and update it to point to your machine's local IP address so the mobile device can reach the backend:

```js
// Example — replace with your machine's actual local IP
const baseurl = "http://192.168.1.100:8000";
export default baseurl;
```

> ⚠️ Use your local network IP (e.g. `192.168.x.x`), not `localhost`, so a physical device or emulator can connect.

---

## Running the Project

> Run all three parts simultaneously for full functionality.

### Frontend

```bash
cd univote_frontend
npm run dev
# or
yarn dev
```

The web app will be accessible at **http://localhost:5173**

---

### Backend

```bash
cd univote_backend
php artisan serve
```

The API will be accessible at **http://localhost:8000**

---

### Scanner App

```bash
cd Scanner
npx expo start
```

Scan the QR code shown in the terminal with the **Expo Go** app on your mobile device, or press `a` to open in an Android emulator.

---

> ✅ Ensure **MySQL is running**, **the backend is serving**, and **the frontend is running** for the full voting experience to work correctly.

GOOD LUCK! 🎉

