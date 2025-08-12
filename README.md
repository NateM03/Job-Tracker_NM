# Job-Tracker_NM

A **Full-Stack Job Application Tracker** – A web application for tracking and managing tech job applications.  
Features include **CRUD operations**, dynamic filtering/sorting via a dashboard UI, work notes for each application, and metrics such as **MTTR** and application status distribution.  
Built with **Django**, **MySQL**, **vanilla JavaScript**, and deployed with **Nginx** on **Linux**.


---

## Table of Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Database Schema](#database-schema)
- [Setup & Installation](#setup--installation)
- [Usage](#usage)
- [Testing](#testing)
- [Debugging & Support Scenarios](#debugging--support-scenarios)
- [Roadmap](#roadmap)
- [License](#license)

---

## Features
- **CRUD Operations** for job postings (Create, Read, Update, Delete).
- **Dynamic Dashboard** for filtering and sorting applications by status, company, or date.
- **Work Notes** for each application with timestamps and author tracking.
- **Metrics Dashboard**:
  - Mean Time to Resolution (MTTR)
  - Reopen Rate
  - Application status distribution
- **Support Engineer Scenarios**:
  - Slow query detection and optimization
  - Handling CORS/CSRF errors
  - Fixing 500 errors from missing data
  - Debugging race conditions and frontend bugs
- Deployed with **Nginx reverse proxy** and **HTTPS**.

---

## Tech Stack
**Frontend:** HTML5, CSS3 (Tailwind), JavaScript (Vanilla / Optional Angular)  
**Backend:** Django (Python)  
**Database:** MySQL  
**Deployment:** Nginx, Linux (Ubuntu/WSL)  
**Version Control:** Git, GitHub  
**Tools:** Chrome DevTools, MySQL CLI, systemd, curl, dig

---

## Architecture

```mermaid
flowchart LR
    A[Browser (HTML/JS)] -->|HTTPS| B[Nginx Reverse Proxy]
    B -->|/api/* proxy| C[Django API (Gunicorn)]
    C -->|SQL| D[(MySQL)]
    B -->|serve| E[Static Assets (web/)]
    C -->|JSON logs / request-id| L[Log Files]

    subgraph Host
        B
        C
        L[(Logs)]
    end
erDiagram
    USERS ||--o{ JOB : creates
    COMPANY ||--o{ JOB : "posted by"
    JOB ||--o{ JOB_NOTE : has
    USERS ||--o{ JOB_NOTE : writes

    USERS {
        INT id PK
        VARCHAR email
        VARCHAR role
        DATETIME created_at
    }

    COMPANY {
        INT id PK
        VARCHAR name
        DATETIME created_at
    }

    JOB {
        INT id PK
        INT user_id FK
        INT company_id FK
        VARCHAR title
        ENUM status  "Saved|Applied|Interview|Offer|Rejected"
        VARCHAR source
        INT salary_min
        INT salary_max
        DATETIME created_at
        DATETIME updated_at
    }

    JOB_NOTE {
        INT id PK
        INT job_id FK
        INT author_id FK
        TEXT body
        DATETIME created_at
    }
