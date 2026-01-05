# 🏥 CardioBot

---

## 🌟 Executive Summary

**CardioBot** is a critical software development project aimed at replacing the fragmented administrative solutions currently used by a cardiology practice.

The core goal is to deliver a **single, highly secure, web-based platform** that simplifies all practice workflows into an efficient **"1-click" experience**.

The project's unique selling proposition lies in its focus on high-level security compliance (**HDS, ISO 27001/27005**) combined with a specialized, embedded **ML AI agent** for patient triage.

| Summary Element | Value Proposition |
| --- | --- |
| **Core Goal** | Replace all existing, fragmented administrative solutions with a single, fast, **"1-click" web application**. |
| **Unique Selling Point** | High-level security focus combined with a specialized, embedded **ML AI agent**. |
| **Primary Risk** | Managing sensitive patient data (PHR) under HDS compliance. |
| **Path to Success** | Achieving a **** page load time and **** chatbot accuracy within the MVP scope. |

---

## ✨ Key Features (MVP Scope)

The Minimum Viable Product (MVP) integrates core practice management with specialized security and automated equipment logistics.

| Feature Area | IN-SCOPE (MUST Deliver) | Quantified Success Metric |
| --- | --- | --- |
| **Core Workflow** | Basic **Patient appointment booking** with minimal clicks and internal staff notifications. | **Booking Completion Rate ** |
| **ML AI Agent** | **Symptom Triage:** Analyze patient input via the Conversation Adapter to prioritize high-risk cases. | **Triage Accuracy ** |
| **Equipment Loan** | **Automated Reservation:** Hardware (Holter/MAPA) is automatically locked to appointments and released upon cancellation. | **Zero Double-Booking Errors** |
| **User Roles** | Three distinct roles: **Patient, Practitioner, and Receptionist.** | Secure Role-Based Access Control (RBAC). |

---

## ⚙️ Technical Stack

The architecture utilizes a "Modular Monolith" approach to balance high performance with health data hosting requirements.

| Component | Technology / Framework | Rationale |
| --- | --- | --- |
| **Backend / API** | **Python (Django)** | Secure ORM and middleware essential for **ISO 27001** and **HDS** compliance. |
| **Frontend** | **HTMX, Stimulus, Vanilla JS** | Reactive "SPA-like" experience without heavy framework overhead (LCP ). |
| **Database** | **PostgreSQL** | Enterprise-grade relational integrity for clinical and inventory data. |
| **Security** | **Trivy, GitHub Actions** | Automated CI/CD security scanning to block CRITICAL vulnerabilities. |
| **Architecture** | **Adapter/Entity Pattern** | Decouples ML logic from the web interface for modular maintenance. |

---

## 📅 Project Timeline (High-Level Plan)

The project is structured into five sequential stages, leading to the **Demo Day deadline of March 20, 2026.**

| Stage | Status | Date Range | Key Milestones & Deliverables |
| --- | --- | --- | --- |
| **Stage 1** | ✅ | *Nov 2025* | Project Idea & Team Formation. |
| **Stage 2** | ✅ | Dec 8 - 12, 2025 | **Project Planning.** Finalized Charter and High-Level Plan. |
| **Stage 3** | ✅ | Dec 15 - Jan 9, 2026 | **Technical Documentation.** Finalized ERD, API Specs, and Loaning Logic. |
| **Stage 4** | 🔵 | Jan 12 - Mar 6, 2026 | **MVP Development.** (Current: Sprinting on core modules). |
| **Stage 5** | 🗓️ | Mar 9 - 20, 2026 | **Project Closure.** Security Audit and **DEMODAY (March 20, 2026).** |

---

## 📚 Full Documentation

The repository maintains full traceability of the development lifecycle.

* **[Full Project Report (Main)](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main/Report_main.md)** - *Master record of project evolution.*
* **[Stage 1 Report](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main/Stage_1/Report.md)** - *Ideation & Brainstorming.*
* **[Stage 2 Report](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main/Stage_2/Report.md)** - *Planning & Timeline.*
* **[Stage 3 Report](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main/Stage_3/Report.md)** - *Architecture & Technical Design.*

---

## 👥 Team

| Member | Primary Project Role | Primary Technical Focus |
| --- | --- | --- |
| **Patrick** | **Project Manager (PM)** | CyberSecurity Student |
| **Benjamin** | PM Support | CyberSecurity Student |
| **Fjolla** | Team Member | Machine Learning (ML) Student |
| **Valentin** | Team Member | Full Stack Student |