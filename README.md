# 🏥 CardioBot

-----

## 🌟 Executive Summary

**CardioBot** is a critical software development project aimed at replacing the fragmented administrative solutions currently used by a cardiology practice.

The core goal is to deliver a **single, highly secure, web-based platform** that simplifies all practice workflows into an efficient **"1-click" experience**.

The project's unique selling proposition lies in its focus on high-level security compliance (**HDS, ISO 27005**) combined with a specialized, embedded **ML AI agent** for patient triage.

| Summary Element | Value Proposition |
| :--- | :--- |
| **Core Goal** | Replace all existing, fragmented administrative solutions with a single, fast, **"1-click" web application**. |
| **Unique Selling Point** | High-level security focus combined with a specialized, embedded **ML AI agent**. |
| **Primary Risk** | Managing sensitive patient data (PHR) under HDS compliance. |
| **Path to Success** | Achieving a **$\le 2.0s$** page load time and **$\ge 90\%$** chatbot accuracy within the MVP scope. |

-----

## ✨ Key Features (MVP Scope)

The Minimum Viable Product (MVP) is focused on integrating core practice management functionality with the specialized security and ML requirements.

| Feature Area | IN-SCOPE (MUST Deliver) | Quantified Success Metric |
| :--- | :--- | :--- |
| **Core Workflow** | Basic **Patient appointment booking** with minimal clicks and internal staff notifications. | **Booking Completion Rate $\ge 85\%$** |
| **ML AI Agent** | **Symptom Triage:** Analyze patient input and escalate high-risk cases to emergency services (SAMU/15) when necessary. | **Triage Accuracy $\ge 90\%$** |
| **Equipment Loan** | Web interface for Admins/Practitioners to **manually change** loan status across 5 minimum states (*Available, Loaned, etc.*). | Defined CRUD API & Database Schema. |
| **User Roles** | Implementation of three distinct roles: **Patient, Practitioner, and Sub-Admin (Receptionist).** | Secure Role-Based Access Control (RBAC). |

-----

## ⚙️ Technical Stack

The choice of technologies prioritizes security, stability, and scalability for handling Protected Health Records (PHR).

| Component | Technology / Framework | Rationale |
| :--- | :--- | :--- |
| **Backend / API** | **Python (Django)** | Robust, built-in security features essential for **ISO 27005** and **HDS** compliance. |
| **Frontend** | **Vanilla JS, HTML5, CSS (BEM)** | Lightweight design focused on superior performance for the "1-click" user experience (LCP $\le 2.0s$). |
| **Database** | **PostgreSQL** | Trusted, enterprise-grade open-source database with advanced security and integrity features. |
| **Machine Learning** | **Python (TensorFlow / NLTK)** | Necessary for developing the complex Natural Language Processing (NLP) models for the **CardioBot** AI agent. |

-----

## 📅 Project Timeline (High-Level Plan)

The project is structured into five sequential stages with a non-negotiable **Demo Day deadline of March 20, 2026.**

| Stage | Duration | Date Range | Key Milestones & Deliverables |
| :--- | :--- | :--- | :--- |
| **Stage 1** | *Completed* | *Nov 2025* | Project Idea & Team Formation. |
| **Stage 2** | 1 Week | Dec 8 - Dec 12, 2025 | **Project Planning.** (Current) |
| **Stage 3** | 4 Weeks | Dec 15, 2025 - Jan 9, 2026 | **Technical Documentation.** (Milestone: Loaning module analysis - Jan 15, 2026) |
| **Stage 4** | 7 Weeks | Jan 12 - Mar 6, 2026 | **MVP Development.** (Milestone: Frontend Responsivity - Feb 15, 2026; Staff Commentary - Mar 1, 2026) |
| **Stage 5** | 2 Weeks | Mar 9 - Mar 20, 2026 | **Project Closure & Demo.** **(DEMODAY: March 20, 2026)** |

-----

## 📚 Full Documentation

The complete project charter, technical documentation, and stage-by-stage reports are maintained in the repository.

  * **[Full Project Report](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main//Report_main.md)**
  * **[Stage 1 Report](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main//Stage_1/Report.md)**
  * **[Stage 2 Report (Planning)](https://github.com/vtiquet/holbertonschool-portfolio_project/blob/main//Stage_2/Report.md)**

-----

## 👥 Team

| Member | Primary Project Role | Primary Technical Focus |
| :--- | :--- | :--- |
| **Patrick** | **Project Manager (PM)** | CyberSecurity Student |
| **Benjamin** | PM Support | CyberSecurity Student |
| **Fjolla** | Team Member | Machine Learning (ML) Student |
| **Valentin** | Team Member | Full Stack Student |