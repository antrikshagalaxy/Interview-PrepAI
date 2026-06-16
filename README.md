# Interview AI - Custom Interview Preparation & Resume Tailoring Platform

Interview AI is a production-ready, full-stack web application that leverages artificial intelligence to prepare candidates for job applications. The application extracts content from a user's resume and compares it against a target job description, generating a comprehensive mock interview preparation plan, match score, skill gap assessment, and custom-tailored PDF resume.

This codebase is split into two main decoupled workspaces: an Express API backend and a Vite + React client.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Environment Variables](#environment-variables)
- [Installation & Setup](#installation--setup)
- [Running the Project](#running-the-project)
- [API Endpoints](#api-endpoints)
- [Code Conventions](#code-conventions)
- [License](#license)

---

## Project Overview
The platform connects two main systems:
1. **Express & Node.js API Backend**: Handles secure token-based user authentication, parses uploaded PDF resumes, defines Zod schemas for structured AI generation, integrates the official Google GenAI SDK, and compiles resumes into PDF streams using Puppeteer.
2. **Customer Storefront (Vite + React)**: A high-fidelity, dark-themed dashboard built using custom Sass styling. Users can register, log in, paste job details, upload resumes with live file indicators, track preparation roadmaps, and download tailored resumes.

---

## Tech Stack

### Frontend Client
* **React (v19)** - Interactive component-driven UI
* **Vite** - High-performance frontend bundler
* **React Router (v7)** - Single Page Application routing
* **Axios** - Promise-based HTTP client for API requests
* **Sass** - Advanced styling and themes with custom SCSS variables

### Backend API
* **Node.js** - JavaScript runtime environment
* **Express.js (v5)** - RESTful backend router and handler
* **MongoDB & Mongoose (v9)** - ODM database management
* **JSON Web Tokens (JWT) & Cookie Parser** - Cookie-based secure stateless sessions
* **Bcryptjs** - Salted password hashing
* **Multer** - Multipart file uploads and memory buffer parsing
* **PDF Parse** - Resume file text extraction
* **Puppeteer** - Headless Chrome compiler for PDF generation
* **Zod & Zod-to-JSON-Schema** - Structured output parsing for Gemini requests

### AI Integration
* **Google GenAI SDK (`@google/genai`)** - Integration with Google's Gemini models for structured data outputs (matching scores, roadmaps, questions).

---

## Features

### 🔒 1. Authentication & Security
* **Secure Registration & Login**: Uses Bcryptjs for password hashing.
* **Stateless Auth**: Implements JWT authorization stored in secure, HTTP-only client-side cookies.

### 🤖 2. Intelligent Matcher & Day-Wise Roadmap
* **Resume Text Extraction**: Automatically extracts text from uploaded resumes using `pdf-parse`.
* **Structured Zod Validation**: Enforces JSON structured responses from Gemini models containing:
  * **Match Score**: 0 to 100 indicator.
  * **Technical & Behavioral Questions**: Questions, interviewer intention, and step-by-step guidance on how to structure answers.
  * **Skill Gaps**: Lists lacking requirements sorted by severity levels (low, medium, high).
  * **Preparation Plan**: Day-by-day learning roadmap focusing on technical topics and tasks.

### 📄 3. Headless Resume PDF Generator
* **AI Resume Tailoring**: Prompts Gemini to tailor a resume specifically pointing out relevant experience matching the job description.
* **HTML-to-PDF Compiling**: Renders Gemini's custom resume HTML layout to a high-quality PDF binary using Puppeteer and pipes it directly to the client's browser for download.

### 📊 4. Interactive Dashboard
* **Dynamic File Dropzone**: Features a responsive upload box that validates PDF size constraints and highlights selected files in real-time.
* **Timeline Timelines**: Visually traces day-by-day learning plans with timeline layouts.

---

## Project Structure

```
Interview-PrepAI/
├── Backend/
│   ├── src/
│   │   ├── config/          # MongoDB connector
│   │   ├── controllers/     # Auth and Interview API controllers
│   │   ├── middlewares/     # JWT authentication and Multer configuration
│   │   ├── models/          # Mongoose database schemas (User, InterviewReport)
│   │   ├── routes/          # Express route definitions
│   │   └── services/        # Gemini API connections and Puppeteer PDF renderer
│   ├── server.js            # Node backend entry point
│   └── .env                 # Private backend variables
│
└── Frontend/
    ├── src/
    │   ├── features/
    │   │   ├── auth/        # Context providers, hooks, pages, and API calls for users
    │   │   └── interview/   # Context, custom hooks, pages, and SCSS styles for dashboards
    │   ├── App.jsx          # Route map layout
    │   ├── main.jsx         # Client mount entry point
    │   └── style.scss       # Global CSS definitions
    ├── vite.config.js       # Vite build configurations
    └── .env                 # Client API variables
```

---

## Environment Variables

### Backend Configuration
Create a `.env` file in the `Backend/` directory:
```env
MONGO_URI="mongodb+srv://<username>:<password>@cluster.mongodb.net/interview-ai"
JWT_SECRET="your_custom_jwt_secret"
GOOGLE_GENAI_API_KEY="your_google_studio_gemini_api_key"
```

### Frontend Configuration
Create a `.env` file in the `Frontend/` directory:
```env
VITE_API_URL="http://localhost:3000"
```

---

## Installation & Setup

### Step 1: Clone the Repository
```bash
git clone https://github.com/antrikshagalaxy/Interview-PrepAI.git
cd Interview-PrepAI
```

### Step 2: Install Backend Dependencies
```bash
cd Backend
npm install
```

### Step 3: Install Frontend Dependencies
```bash
cd ../Frontend
npm install
```

---

## Running the Project

Start both servers in separate terminal tabs:

### 1. Backend Server (Port: 3000)
```bash
cd Backend
npm run dev
```

### 2. Frontend Client (Port: 5173)
```bash
cd Frontend
npm run dev
```

Open your browser and navigate to `http://localhost:5173`.

---

## API Endpoints

### User Authentication (`/api/auth`)
* `POST /register` - Registers a new user.
* `POST /login` - Log in a user and set cookies.
* `GET /logout` - Log out a user.
* `GET /get-me` - Get information about the authenticated user.

### Interview Strategy & Resume (`/api/interview`)
* `POST /` - Accepts resume file (multipart), selfDescription, and jobDescription to generate a strategy report.
* `GET /` - Fetches historical interview reports for the logged-in user.
* `GET /report/:interviewId` - Fetches details for a specific interview plan.
* `POST /resume/pdf/:interviewReportId` - Generates a tailored PDF document.

---

## Code Conventions
* **State Management**: React Context (`InterviewProvider` and `AuthProvider`) controls global session and plan caching.
* **Modular Features**: Feature logic (pages, styles, hooks, and api services) is fully grouped inside `features/auth` and `features/interview` directories.
* **Sass Styling**: Styles use modern, deprecation-free Sass syntax (`color.adjust` module).

---
