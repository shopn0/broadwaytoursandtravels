# 🏢 Broadway OMS — Office & Travel Operations ERP

> A lightweight, cloud-integrated Enterprise Resource Planning (ERP) and Office Management System tailored for Hajj & Umrah agencies and travel enterprise operations.

**Live Application:** [officebroadway.web.app](https://officebroadway.web.app)  
**Developed by:** Rizwan Shariare Shopno ([Samplex IT Solutions](https://www.samplex.rf.gd))

---

## 📌 Executive Summary

**Broadway OMS (Broadway Office Management System)** centralizes multi-branch agency workflows into a single high-performance web application. It combines real-time document issuance (invoices, money receipts, Umrah quotations), live accounting synchronization with Google Workspace spreadsheets, Firestore real-time NoSQL storage, and contextual regional tools like an astronomical prayer time engine and milestone event countdowns.

---

## 🛠️ Technology Stack & Architecture

### **Frontend**
- **Language / Framework:** JavaScript (ES6+), React 18, Vite
- **Styling:** Modular CSS, Flexbox/Grid responsive design, CSS variables
- **Icons & UI:** Lucide React icons
- **Document Generation:** `jspdf`, `html2canvas`, DOM canvas rendering
- **Data Parsing:** `papaparse` for high-speed client-side CSV streaming

### **Backend & Cloud Infrastructure**
- **Hosting:** Firebase Hosting (production edge CDN)
- **Database:** Firebase Cloud Firestore (NoSQL, real-time sync)
- **Authentication:** Firebase Authentication & local session guards
- **Spreadsheet Bridge:** Google Apps Script (REST Webhook / JSON endpoints)
- **Data Storage (Sync):** Google Sheets as a live business ledger (Broadway-27)

---

## 🧩 Key System Modules & Responsibilities

| Module | Responsibilities | Data Layer |
| :--- | :--- | :--- |
| **Dashboard** | Bird's-eye metrics across all modules, interactive event milestone countdowns, and quick actions | Firestore + Google Sheets API |
| **Office Invoices & Receipts** | Generate invoices, track Due vs. Received status, generate branded printable A4 PDFs with company seals & authorized signatures | Cloud Firestore |
| **Office Bills** | Track operational expenses, real-time Status toggle (`Due` ↔ `Paid`), and dynamic CRUD operations on sheet ledger | Google Apps Script + Google Sheets |
| **Hajj Pilgrims Registry** | Group-by-group pilgrim roster, tracking payments across multiple installments, discounts, and outstanding dues | Google Sheets (GViz CSV Engine) |
| **Umrah Cost Calculator & Quotations** | Live BDT/SAR currency exchange conversion, room sharing pax math, customizable quotation generator with saved presets | LocalStorage & Client State |
| **Prayer Time Engine** | Astronomical solar calculation tailored to Dhaka coordinates (23.8103°N, 90.4125°E) with live active Waqt display and countdown | Pure Math / Client Engine |

---

## 🔄 System Flowchart & Data Architecture

```mermaid
flowchart TD
    subgraph Client["🖥️ React Frontend (Vite + Firebase Hosting)"]
        UI["User Interface (Dashboard / Invoices / Bills / Pilgrims)"]
        PrayerEngine["🕌 Dhaka Astronomical Prayer Engine"]
        PDFEngine["📄 Client-side PDF Generator (jspdf/html2canvas)"]
    end

    subgraph Firebase["🔥 Google Firebase Services"]
        Auth["Firebase Authentication (Admin RBAC)"]
        Firestore["Cloud Firestore (Invoices, Events, Status Records)"]
    end

    subgraph GoogleCloud["📊 Google Workspace Layer"]
        GAS["Google Apps Script (REST Webhook / JSON API)"]
        GSheet[("Google Sheets Central Ledger (Broadway-27)")]
    end

    UI --> Auth
    UI <-->|Real-time Snapshot Sync| Firestore
    UI -->|Print / Save PDF| PDFEngine
    UI -->|Solar Math Calculation| PrayerEngine

    UI -->|Direct CSV Stream Query| GSheet
    UI -->|POST: Add / Edit / Toggle Bills| GAS
    GAS -->|Row Insertion & Formatting| GSheet
