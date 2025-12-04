# 🚀 Stage 1 Report: CardioBot

This document formalizes the initial stage of development for **CardioBot**, a high-security solution designed to consolidate and simplify all administrative and patient interaction workflows for a cardiology practice.

***

## Team, Governance & Work Standards 👥

### Team Composition and Roles

| Member | Primary Project Role | Primary Technical Focus | Key Rationale & Competence |
| :--- | :--- | :--- | :--- |
| **Patrick** | **Project Manager (PM)** | CyberSecurity Student | **Final Decision Maker** on scope and technical feasibility. Focuses on **ISO 27005** compliance, risk analysis, and overall security posture. |
| **Benjamin** | PM Support | CyberSecurity Student | Focuses on **HDS Certification** compliance, secure deployment (DevOps), infrastructure hardening, and system architecture design. |
| **Fjolla** | Team Member | Machine Learning (ML) Student | Responsible for developing, training, and integrating the specialized **CardioBot** model for the AI agent. |
| **Valentin** | Team Member | Full Stack Student | Responsible for API implementation, system integration, and ensuring a fast, simple, and intuitive **"1-click" UI** (JS, CSS, HTML). |

---

### Collaboration, Decision, and Work Standards

| Standard Category | Norm / Tool | Actionable Work Standard |
| :--- | :--- | :--- |
| **Communication** | Discord, Weekly Stand-ups | Bi-weekly client meeting with Dr. Tzvetkov to review progress and adjust the **MVP scope.** |
| **Decision Process** | PM-led (Patrick) | Patrick is the final authority on technical and scope decisions. Client requests are prioritized by the PM for feasibility. |
| **Code Quality** | Private GitHub, Dev Branches | **Mandatory Peer Review (PR):** All feature code requires approval from a non-author before merging to the `main` branch. |
| **Security & Testing** | Agile Board | **Security Unit Tests:** All new API endpoints must have associated unit tests covering security validation (authentication/authorization). |
| **Productivity** | Advancement Meetings | **Feature Definition First:** Technical specifications for a feature must be written, reviewed, and approved before any development code is written. |

***

## Project Vision & Objectives

### Client & Core Problem

* **Primary Stakeholder:** Dr. Tzvetkov (Cardiologist, Product Owner).
* **Core Problem:** Existing solutions (scheduling, billing, equipment tracking, patient data) are fragmented, complex, and lack high-level security certification required for medical data.
* **Vision:** To create a **single, secure, web-based UCPM platform** that simplifies all practice workflows into an efficient "1-click" experience.

### Technical Stack

The choice of technologies prioritizes security, stability, and scalability for handling Protected Health Records (PHR).

| Component | Technology / Framework | Rationale |
| :--- | :--- | :--- |
| **Backend / API** | **Python (Django)** | Robust, built-in security features (ORM, Auth, CSRF) essential for **ISO 27005** and **HDS** compliance. |
| **Frontend** | **Vanilla JS, HTML5, CSS (BEM)** | Focus on lightweight design for superior performance and flexibility to implement the simple "1-click" user experience. |
| **Database** | **PostgreSQL** | Trusted, enterprise-grade open-source database with advanced security and integrity features required for PHR data. |
| **Machine Learning** | **Python (TensorFlow / NLTK)** | Necessary for developing the complex Natural Language Processing (NLP) models for the **CardioBot** AI agent. |

***

## Minimum Viable Product (MVP) Scope

### Feature Definition and Boundaries

The MVP focuses on integrating core practice management functionality with the specialized security and ML requirements.

| Feature Area | IN-SCOPE (MUST Deliver) | OUT-OF-SCOPE (NOT for MVP) |
| :--- | :--- | :--- |
| **User Roles** | Implementation of three distinct roles: **Patient, Practitioner, and Sub-Admin (Receptionist).** | Complex, granular, data-level permissioning (e.g., viewing rights based on specific data sets). |
| **Appointment/Notification** | Basic **Patient appointment booking** from the index page with minimal clicks. Internal notifications for Receptionist/Practitioner. | Real-time calendar synchronization with external tools (Google Calendar, external EMRs). |
| **ML AI Agent** | **Symptom Triage:** The model must analyze patient input and escalate high-risk cases to emergency services (SAMU/15) when necessary. | Providing small medical diagnostics, managing appointment cancellations/rescheduling. |
| **Equipment Loan Module** | A web interface for Admins/Practitioners to **manually change** loan status. **Minimum Statuses:** *Available, Loaned, In Transit, Data Recovering, Cleaning.* | Automated inventory alerts, integrated GPS tracking or a scan system, maintenance scheduling based on usage. |
| **Billing Process** | A module that allows the Practitioner/Sub-Admin to **generate, print, and/or prepare PDF invoice documents** for consultation fees, replacing the legacy system. | Full integration with external accounting software or automated payment processing/claim submission. |

### Quantified Success Metrics

These metrics will be used to objectively validate the success of the UCPM MVP in later stages.

| Metric | Proposed Target | Justification |
| :--- | :--- | :--- |
| **Core Page Load Time (LCP)** | **$\le 2.0$ seconds** | A fast user experience is non-negotiable for the "1-click" goal. This meets the performance standards of top-tier web applications. |
| **Chatbot Triage Accuracy** | **$\ge 90\%$** | Essential for a medical application. A minimum of 90% accuracy in correctly identifying and triaging high-risk symptoms ensures patient safety and trust. |
| **Appointment Booking Completion Rate (GCR)** | **$\ge 85\%$** | Measures the effectiveness of the simple UI. If 85% of patients who start booking successfully complete it, the UCPM has successfully replaced the previous fragmented systems. |

***

## Executive Summary

The **CardiBot** is a critical internal software development project for Dr. Tzvetkov's practice. Led by Patrick, the project team leverages its specialized skills in Full Stack, Machine Learning, and high-level Cybersecurity (ISO 27005, HDS) to deliver a single, secure, and user-friendly platform.

| Summary Element | Value Proposition |
| :--- | :--- |
| **Core Goal** | Replace all existing, fragmented administrative solutions with a single, fast, **"1-click" web application**. |
| **Unique Selling Point** | High-level security focus combined with a specialized, embedded **ML AI agent**. |
| **Key Modules** | Appointment Booking, Sub-Admin Roles, Equipment Loan Tracker, Basic Billing, and ML AI agent. |
| **Primary Risk** | Managing sensitive patient data (PHR) under HDS compliance. |
| **Path to Success** | Achieving a **$\le 2.0s$** page load time and **$\ge 90\%$** chatbot accuracy within the MVP scope. |