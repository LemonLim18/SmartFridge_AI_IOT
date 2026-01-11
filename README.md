# SmartFridge AI IoT 🧊🤖

A modern, AI-powered Smart Fridge ecosystem built for the ultimate kitchen management experience. This project integrates real-time IoT sensor monitoring, computer vision for inventory tracking, and a Gemini-powered Chef AI to suggest recipes based on what’s actually in your fridge.

> **Course Prototype (CST357 Real-time Systems & IoT)**: This repo contains the full source code for the **frontend**, **backend**, and **edge device programs**, including MQTT-based device-to-cloud messaging, Supabase persistence, and a web dashboard.

---

## 📚 Table of Contents

1. 📌 Project Overview
2. 🚀 Key Features
3. 🛠️ Technology Stack
4. ⚙️ Setup & Installation
5. 🔐 Environment Variables (and how to get them)
6. 🔌 Hardware & Wiring
7. 🗄️ Supabase Setup
8. 📁 Project Structure
9. 🎥 Demo Links
10. 📄 License

---

## 📌 Project Overview

This prototype uses **two edge devices**:

### 🥧 Raspberry Pi 2 Model B

- Reads **DHT11** (temperature + humidity)
- Reads **infrared door sensor** (door open/close events)
- Triggers the **camera capture workflow**
- Controls a small **LED** as a temporary light assist during snapshots

### 🪶 Maker Feather AIoT S3

- Reads the **analog water level sensor**
- Infers **freezer defrost / melting risk**
- Publishes freezer status updates

Both devices publish updates to an **MQTT broker** hosted on a **Google Cloud Platform VM**.  
The backend subscribes to these MQTT topics, applies event-based logic, stores meaningful records in **Supabase**, and pushes real-time updates to the **frontend dashboard**.

For architecture + system interaction details, refer to the project report in this repository.

---

## 🚀 Key Features

- **🏠 Interactive Dashboard**: Monitor real-time telemetry (temperature, humidity, door status, freezer status).
- **🔔 Notifications & Alerts**: Reminders/alerts when unsafe conditions persist.
- **🧾 Alert Logs**: Stores resolved incidents as short summaries.
- **📦 Smart Inventory Management**: Track food items with categories, stock levels, and expiry estimation.
- **📸 IoT Snapshot Analysis**: Scan your fridge using AI to automatically identify and log new items (**Gemini 1.5 Flash**).
- **👨‍🍳 Gemini Chef AI**: Recipe suggestions + detailed cooking steps based on available inventory and preferences.
- **🛒 Smart Shopping List**: Identifies low-stock items and supports shopping decisions.
- **📍 Local Store Finder**: Uses real-world price data (**Hargapedia PriceCatcher**) to recommend nearby and cheaper stores.

---

## 🛠️ Technology Stack

### Frontend

- **Framework**: [React](https://react.dev/) + [Vite](https://vitejs.dev/)
- **Language**: [TypeScript](https://www.typescriptlang.org/)
- **Styling**: [Tailwind CSS](https://tailwindcss.com/) + [DaisyUI](https://daisyui.com/)
- **Icons**: [Lucide React](https://lucide.dev/)
- **Charts**: [Recharts](https://recharts.org/)

### Backend

- **Framework**: [FastAPI](https://fastapi.tiangolo.com/) + [Uvicorn](https://www.uvicorn.org/)
- **MQTT**: Python MQTT client for subscriptions
- **Database & Storage**: Supabase
- **AI/ML**: [Google Gemini AI (1.5 Flash)](https://ai.google.dev/)
- **Data Processing**: [Pandas](https://pandas.pydata.org/)
- **Geocoding**: [Geopy](https://geopy.readthedocs.io/) + [Geoapify API](https://www.geoapify.com/)
- **System**: Windows Geolocation Integration (via `winsdk`)

### Edge Devices

- **Raspberry Pi 2 Model B** + **Raspberry Pi Camera Module Rev 1.3**
- **Maker Feather AIoT S3** + analog water level sensor module

---

## ⚙️ Setup & Installation

### 1) Backend Setup

1. **Navigate to the backend directory**:

   ```bash
   cd backend
   ```

2. **Create and activate a virtual environment**:

   ```bash
   python -m venv .venv
   # Windows:
   .venv\Scripts\activate
   # macOS/Linux:
   source .venv/bin/activate
   ```

3. **Install dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Configure Environment Variables** (recommended)
   Copy the example file and fill in your values:

   ```bash
   # macOS/Linux
   cp .env.example .env

   # Windows PowerShell
   copy .env.example .env
   ```

5. **Run the FastAPI server**:
   ```bash
   uvicorn server:app --host 0.0.0.0 --port 8000 --reload
   ```

---

### 2) Frontend Setup

1. **Navigate to the frontend directory**:

   ```bash
   cd frontend
   ```

2. **Install dependencies**:

   ```bash
   npm install
   ```

3. **Configure Environment Variables** (recommended)
   Copy the example file and fill in your values:

   ```bash
   # macOS/Linux
   cp .env.example .env

   # Windows PowerShell
   copy .env.example .env
   ```

4. **Run the development server**:
   ```bash
   npm run dev
   ```

---

## 🔐 Environment Variables

This project uses `.env` files to store secrets (API keys) and configuration locally.

✅ Example templates are included:

- `frontend/.env.example`
- `backend/.env.example`

### 📌 Setup

Create your real `.env` files by copying the examples:

**Frontend**

```bash
cd frontend
cp .env.example .env
```

**Backend**

```bash
cd backend
cp .env.example .env
```

---

### 🗄️ Supabase Variables

You’ll need a Supabase project for **database + storage**.

**How to get them**

1. Create a project on Supabase
2. Go to **Project Settings → API**
3. Copy the **Project URL** and **API keys**

**Recommended variables**

- `SUPABASE_URL`  
  Your project URL (from Supabase settings)

- `SUPABASE_KEY`

  - Backend: use **service role key** for write operations (**do not expose this in frontend**)
  - Frontend: use **anon key**

- `SUPABASE_BUCKET`  
  Storage bucket name for camera images (e.g. `camera_images`)

---

### 🤖 AI / Vision / Recipe Features (`backend/.env`)

```env
GEMINI_API_KEY=your_google_ai_api_key
POLLINATIONS_API_KEY=your_pollinations_api_key
```

**What they’re for**

- `GEMINI_API_KEY`: Required for Gemini-powered features (Chef AI + snapshot item recognition).
- `POLLINATIONS_API_KEY`: Optional. Used for additional AI generation features (only required if your backend calls Pollinations).

**How to get `GEMINI_API_KEY`**

1. Open **Google AI Studio**
2. Create an API key
3. Paste it into `GEMINI_API_KEY`

---

### 🗺️ Location / Store Finder (`backend/.env`)

```env
GEOAPIFY_API_KEY=your_geoapify_api_key
```

**What it’s for**

- `GEOAPIFY_API_KEY`: Enables geocoding / store locator features (convert address/location → coordinates).

**How to get it**

1. Create an account at Geoapify
2. Create a project → generate an API key
3. Paste it into `GEOAPIFY_API_KEY`

---

### 📧 Email Notifications (`backend/.env`)

```env
RESEND_API_KEY=your_resend_api_key
```

**What it’s for**

- `RESEND_API_KEY`: Enables email delivery for alerts/reminders via Resend.

**How to get it**

1. Create a Resend account
2. Generate an API key in the dashboard
3. Paste it into `RESEND_API_KEY`

---

## 🗄️ Supabase Variables (`backend/.env`)

```env
SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_key
SUPABASE_BUCKET=camera_images
```

**What they’re for**

- `SUPABASE_URL`: Your Supabase project URL.
- `SUPABASE_KEY`: Supabase API key.
  - Backend: use **service role key** for write operations (**keep secret**).
  - Frontend (only if used): use **anon key** (**safe-ish**, but still treat carefully).
- `SUPABASE_BUCKET`: Storage bucket name for fridge camera images (example: `camera_images`).

**How to get them**

1. Create a project in Supabase
2. Go to **Project Settings → API**
3. Copy the **Project URL** and the **API keys**
4. Create a bucket under **Storage** (e.g. `camera_images`)

---

### 📡 MQTT Broker Variables (`backend/.env`)

If your backend subscribes to MQTT topics from Raspberry Pi / Maker Feather:

```env
MQTT_BROKER=your_broker_host
MQTT_PORT=1883
MQTT_USER=your_username
MQTT_PASS=your_password
MQTT_BASE=smartfridge
```

**What they’re for**

- `MQTT_BROKER`: Broker host/IP (e.g. your GCP VM public IP)
- `MQTT_PORT`: Usually `1883` (or `8883` for TLS)
- `MQTT_USER` / `MQTT_PASS`: Broker credentials (if enabled)
- `MQTT_BASE`: Base topic prefix (example: `smartfridge`)

---

## 🔌 Hardware & Wiring

### ✅ Hardware List

- Raspberry Pi 2 Model B
- Raspberry Pi Camera Module Rev 1.3 (OV5647)
- DHT11 temperature & humidity sensor
- Infrared obstacle detector module (door state)
- LED + resistor (temporary light assist during snapshots)
- Maker Feather AIoT S3
- Water level sensor module (analog output)
- Jumper wires + breadboard

### 📌 Pin Connection Diagram (Raspberry Pi)

This figure shows the pin connection overview for:

- DHT11 sensor
- Infrared door sensor
- Camera module (CSI)
- LED indicator

![Raspberry Pi pin connection diagram](docs/Hardware%20Diagram-Raspberry%20Pi%20wiring%20overview.jpg)

---

### 📌 Pin Connection Diagram (Maker Feather AIoT S3)

This figure shows the pin connection overview for:

- Water level sensor module (analog)

![Maker Feather pin connection diagram](docs/Hardware%20Diagram-Maker%20Feather%20wiring%20overview.png)

## 🗄️ Supabase Setup

### 1) Create Storage Bucket

1. In Supabase, open **Storage**
2. Create a bucket named `camera_images`
3. Configure access policy based on your demo needs:
   - Public read (simpler demo), or
   - Protected access with policies (safer)
4. Inside the `camera_images` bucket, create a folder/path called `captures`
   - This is where all fridge snapshot images will be stored
   - The system will automatically save captured images to `camera_images/captures/` when snapshots are taken

---

### 2) Create Database Tables

This repository includes a SQL file that creates the required tables:

- `docs/supabase.sql`

Run it in **Supabase SQL Editor**.

The database stores:

- inventory items extracted from snapshots
- anomaly events as incident summaries
- shopping list items
- saved recipes

---

## 📁 Project Structure

```text
├── backend/
│   ├── server.py               # FastAPI app and endpoints
│   ├── geminiRecipe.py         # Gemini logic for recipes and snapshots
│   ├── recommender.py          # Store recommendation engine
│   ├── location.py             # Windows geolocation integration
│   ├── hargapedia_cli.py       # PriceCatcher aggregation
│   ├── iot_code.py             # Raspberry Pi edge code
│   ├── uploadInventory.py      # Supabase database operations
│   ├── makerFeather.ino        # Maker Feather firmware
│   ├── requirements.txt
│   ├── requirements_iot.txt
│   ├── .env.example
│   └── Dockerfile              # Optional container for Raspberry Pi edge runtime
├── frontend/
│   ├── src/
│   │   ├── App.tsx
│   │   ├── components/
│   │   ├── services/
│   │   └── types.ts
│   ├── vite.config.ts
│   └── .env.example
├── docs/
│   ├── supabase.sql
│   └── wiring diagrams and architecture images
└── README.md
```

---

## 📄 License

This project is for educational purposes. All data sourced from [data.gov.my](https://data.gov.my) is subject to their licensing terms.
