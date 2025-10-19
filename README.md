# 🧠 Stock Alert Dashboard — Detailed Team README

**Team:**

* **Aseem** → Frontend (React Dashboard)
* **Atulya** → Backend (FastAPI + PostgreSQL)
* **Zaid** → Scraper (Data from Blinkit, Zepto, etc.)
* **Manoj** → Alerts & Deployment (Twilio + Hosting)

---

## 🌐 1. FRONTEND (Aseem) — React Dashboard

### 🎯 Objective

Create an intuitive **dashboard interface** for D2C brands to monitor stock status across dark stores, pincodes, and SKUs in real-time.

### ⚙️ Tech Stack

* **React.js (Vite or CRA)**
* **Axios / Fetch API** for backend calls
* **Chart.js / Recharts** for visualizations
* **TailwindCSS / Material UI** for styling
* **React Router** for navigation

### 🧩 Folder Structure

```
frontend/
├── public/
├── src/
│   ├── components/        # Reusable UI components (Cards, Charts)
│   ├── pages/             # Pages like Dashboard, AlertsHistory, Login
│   ├── services/          # API call helpers
│   ├── App.js
│   └── main.jsx
└── package.json
```

### 🚀 Setup Steps

1. **Navigate to folder:**

   ```bash
   cd frontend
   ```
2. **Install dependencies:**

   ```bash
   npm install
   ```
3. **Run app locally:**

   ```bash
   npm run dev
   ```
4. **Environment variables:**
   Create `.env` file:

   ```
   REACT_APP_API_BASE=http://localhost:8000
   ```
5. **API Connections:**

   * `/pincodes` → List of monitored pincodes
   * `/darkstores/{pin}` → Get all dark stores for a pin
   * `/skus/{store_id}` → Get SKU details
   * `/alerts` → Get alert history

### 🧠 Key Responsibilities

* Create dashboard layout with **Sidebar (Pincode list)** and **Main Panel (Dark store cards)**
* Integrate backend API
* Use charts to show stock % (donut/pie)
* Ensure **mobile responsiveness**

---

## ⚙️ 2. BACKEND (Atulya) — FastAPI + PostgreSQL

### 🎯 Objective

Provide REST APIs that serve the latest stock data, alert history, and analytics to the frontend.

### 🧰 Tech Stack

* **FastAPI (Python)**
* **PostgreSQL** (or SQLite for testing)
* **SQLAlchemy** ORM
* **Pydantic** for request/response models
* **Uvicorn** for local server

### 🧩 Folder Structure

```
backend/
├── main.py                  # Entry point
├── models/                  # SQLAlchemy models
├── routes/                  # API endpoints
├── schemas/                 # Pydantic models
├── database.py              # DB connection logic
├── requirements.txt
└── .env.example
```

### 🚀 Setup Steps

1. **Navigate to backend:**

   ```bash
   cd backend
   ```
2. **Create and activate virtual environment:**

   ```bash
   python -m venv venv
   source venv/bin/activate  # macOS/Linux
   venv\\Scripts\\activate   # Windows
   ```
3. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```
4. **Run FastAPI server:**

   ```bash
   uvicorn main:app --reload
   ```
5. **Example `.env.example`:**

   ```
   DATABASE_URL=postgresql://user:password@localhost:5432/stockdb
   TWILIO_ACCOUNT_SID=xxxxx
   TWILIO_AUTH_TOKEN=xxxxx
   TWILIO_PHONE_NUMBER=xxxxx
   ```

### 🧠 Endpoints (to be implemented)

| Endpoint            | Method | Description                           |
| ------------------- | ------ | ------------------------------------- |
| `/pincodes`         | GET    | Get list of monitored pincodes        |
| `/darkstores/{pin}` | GET    | Get dark stores under that pincode    |
| `/skus/{store_id}`  | GET    | Get SKU availability for a dark store |
| `/alerts`           | GET    | Fetch alert history                   |
| `/alerts`           | POST   | Add a new alert triggered by scraper  |

---

## 🤖 3. SCRAPER (Zaid) — Data Collection Layer

### 🎯 Objective

Collect stock availability data from **Blinkit**, **Zepto**, and **Swiggy Instamart**, simulate hyperlocal data per pincode, and output in a **standardized JSON** used by the backend.

### ⚙️ Tech Stack

* **Python + Selenium / Playwright**
* **BeautifulSoup4** for parsing HTML
* **JSON / CSV** for output
* Optional: **Proxy rotation** for safe scraping

### 🧩 Folder Structure

```
scraper/
├── scripts/
│   ├── blinkit_scraper.py
│   ├── zepto_scraper.py
│   └── instamart_scraper.py
├── data/
│   └── sample_output.json
├── requirements.txt
└── README.md
```

### 🚀 Setup Steps

1. **Navigate:**

   ```bash
   cd scraper
   ```
2. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```
3. **Run scraper:**

   ```bash
   python scripts/blinkit_scraper.py
   ```
4. **Output Format (`sample_output.json`):**

   ```json
   {
     "pincode": "400001",
     "dark_stores": [
       {
         "store_id": "BL123",
         "platform": "Blinkit",
         "skus": [
           { "name": "Mango Oatmeal", "price": 120, "stock": 0, "competitor_stock": "Full" }
         ]
       }
     ]
   }
   ```

### 🧠 Responsibilities

* Maintain data accuracy (95% target).
* Avoid overloading platforms — add delay/random sleep.
* Push JSON to backend API every 10 mins via cron/scheduler.

---

## 💬 4. ALERTS & DEPLOYMENT (Manoj)

### 🎯 Objective

Send **WhatsApp alerts** when SKUs go out of stock and handle deployment of backend + frontend.

### ⚙️ Tech Stack

* **Twilio API** (WhatsApp messaging)
* **Python (requests)** for sending alerts
* **Render / AWS / Railway** for backend deployment
* **Vercel / Netlify** for frontend deployment

### 🧩 Folder Structure

```
alerts/
├── whatsapp_alerts.py
├── deploy_notes.md
└── .env.example
```

### 🚀 Setup Steps

1. **Create `.env` file:**

   ```
   TWILIO_ACCOUNT_SID=xxxx
   TWILIO_AUTH_TOKEN=xxxx
   TWILIO_PHONE_NUMBER=whatsapp:+14155238886
   BRAND_PHONE_NUMBER=whatsapp:+91XXXXXXXXXX
   ```

2. **Run test alert:**

   ```bash
   python whatsapp_alerts.py
   ```

3. **Sample Alert Message:**

   ```
   🚨 Stock Alert 🚨
   You're OOS in Blinkit Store #123 (Pin 400001)
   Last week demand: 50 units
   Rivals fully stocked — restock ASAP!
   ```

4. **Deployment Notes:**

   * **Backend:** Use Render → Connect GitHub repo → Auto deploy on push
   * **Frontend:** Deploy to Vercel → Connect to same repo
   * **Scraper:** Set up cron job on Railway or Replit for auto-run every 10 mins
   * Ensure `.env` variables are added to hosting environments

---

## 🗓️ Suggested 4-Week Plan

| Week | Focus                                             | Owner(s)     |
| ---- | ------------------------------------------------- | ------------ |
| 1    | Set up project structure + dummy APIs + sample UI | All          |
| 2    | Backend endpoints + scraper JSON format finalized | Atulya, Zaid |
| 3    | Alerts integration + UI linking with backend      | Aseem, Manoj |
| 4    | Testing + deploy + pilot demo with 2–3 brands     | All          |

---

## 🧭 Summary

| Module       | Owner  | Deliverable                  |
| ------------ | ------ | ---------------------------- |
| **Frontend** | Aseem  | React Dashboard (3–4 pages)  |
| **Backend**  | Atulya | FastAPI APIs + Database      |
| **Scraper**  | Zaid   | JSON feed every 10 mins      |
| **Alerts**   | Manoj  | WhatsApp alerts + Deployment |


