# Reservation System - Booking Management System (SPA)

A lightweight, high-performance Single Page Application (SPA) built with Vanilla JavaScript, Vite, and Tailwind CSS. The project implements a robust **CRUD** system for handling workspace reservations with dynamic role-based permissions and real-time data persistence powered by `json-server`.

---

## 🚀 Getting Started

Follow these steps to set up and run the application locally on your machine.

### 1. Prerequisites
Ensure you have [Node.js](https://nodejs.org) installed.

### 2. Installation
Clone the repository, open the project directory in Visual Studio Code, and install the dependencies:

```bash
npm install
```

### 3. Running the Project (Simultaneous Servers)
The system requires both the local data server and the frontend builder to be active at the same time. Open **two separate terminals** in VS Code:

*   **Terminal 1 (Mock Database API):**
    ```bash
    npx json-server --watch db.json --port 3000
    ```
*   **Terminal 2 (Vite Frontend Development):**
    ```bash
    npm run dev
    ```

Open the local URL provided by Vite (usually `http://localhost:5173`) in your web browser.

---

## 📂 Project Architecture

The codebase follows a modular **MVC-like / Component** design for native web client applications:

```text
├── db.json                 # Mock JSON Database (Users, Workspaces & Reservations)
├── index.html              # Core Entry Layout Document
├── package.json            # Node Dependencies & Build Scripts
└── src/
    ├── main.js             # Application Bootstrap & Root Template Mounting
    ├── style.css           # Global Tailwind CSS Customizations
    ├── api/
    │   └── http.js         # Configured Network Instance (Fetch/Axios Abstraction)
    ├── components/         # Reusable UI Snippets (Stateless Dynamic HTML templates)
    │   ├── ReservationCard.js
    │   ├── Sidebar.js
    │   └── WorkspaceCard.js
    ├── controllers/        # Logical Handlers (DOM Listeners, Forms, State, and API triggers)
    │   ├── home.controller.js
    │   ├── login.controller.js
    │   └── workspace.controller.js
    ├── router/             # Client-side SPA Hash Router Matrix and Access Guards
    │   └── router.js
    ├── services/           # Data Fetch Layer & Front-facing Validation Engine
    │   ├── reservation.service.js
    │   └── workspace.service.js
    ├── utils/              # Storage Helpers, Encryption, and Local Session Managers
    │   └── index.js
    └── views/              # Page View Layout Structural Strings
        ├── homeView.js
        ├── loginView.js
        └── workspaceView.js
```

---

## 🛠️ System Core Features

### 📋 Reservation Properties
Every booking structural object managed by the CRUD workflow contains:

*   **id**: Unique Identifier.
*   **userId**: Foreign ID of the requesting user.
*   **workspace**: Chosen reserved workspace environment.
*   **date**: Target calendar event date.
*   **startHour / endHour**: Timeframes block boundaries.
*   **reason**: Justification text string.
*   **status**: Dynamic state flags (`Pending` | `Approved` | `Rejected` | `Canceled`).

### ⚙️ Strict Business Rules (Domain Logic)
*   **Anti-Overlap Validation:** The service blocks any simultaneous booking duplicate attempts sharing the same workspace environment, same date, and overlapping hour ranges.
*   **Immutable States Validation:** Common users are completely restricted from changing any booking properties unless the target record remains strictly marked as `Pendiente`.
*   **Cancellation Rules:** Approved bookings (`Aprobada`) lose all modification properties except for the explicit option to be flagged as `Cancelada`.

---

## 🔐 Access Matrix & Role Permissions

### 👑 Administrator (`admin`)
*   Full HTTP Method **CRUD** capabilities globally over all entries.
*   Authorization to switch states (`Approved` / `Rejected`).
*   Privileges to fully create, patch, or remove physical Workspaces (`workspaces`).
*   Global dashboard read access over the full collection matrix.

### 👤 Regular User (`user`)
*   Capability to spawn requests (`POST`).
*   Read visibility limited strictly to self-owned bookings matching their user `id`.
*   Allowed to update data elements exclusively while marked as `Pending`.
*   Permission to toggle self-owned items to `Canceled`.
