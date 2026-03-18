# Hospital Management System (HMS) - User Guide

## 👋 Introduction
Welcome to the **Hospital Management System**. Think of this software as the **digital brain** of a hospital. Just like a hospital has a reception desk, doctors' cabins, and a record room, this application has digital versions of all these things.

It helps:
- **Patients** find doctors and book appointments easily (like booking a movie ticket).
- **Doctors** manage their schedule and patient records (digital prescription pad).
- **Admins** (Hospital Management) keep track of everything happening in the hospital.

---

## 👥 Roles & How They Work

### 1. The Admin (Hospital Manager)
**Who is this?** The super-user who manages the hospital staff and improved the system.
**What can they do?**
- **Hire Doctors**: Add new doctors to the system with their specialization (e.g., Cardiologist, Dentist).
- **Monitor Stats**: See a dashboard showing how many patients, doctors, and appointments are in the system.
- **Cleanup**: Remove doctors if they leave the hospital.

### 2. The Doctor (Medical Specialist)
**Who is this?** The medical professional treating patients.
**What can they do?**
- **View Schedule**: See who is visiting them today or later.
- **Treat Patients**: During an appointment, they can write a "digital prescription" that includes the diagnosis (what's wrong) and treatment (medicine/advice).
- **History**: They only see patients assigned to them.

### 3. The Patient (You and Me)
**Who is this?** Anyone seeking medical help.
**What can they do?**
- **Register**: Create an account (like signing up for Facebook).
- **Search**: Find a doctor for a specific problem (e.g., search for "Dentist").
- **Book**: Choose a date and time to meet the doctor.
- **History**: View all past treatments and prescriptions anytime. No need to carry paper files!
- **Download Records**: Request a full history download (Excel file) for personal records.

---

## 🚀 How to Start the Project using VS Code (For Beginners)

This guide assumes you are setting this up on a computer for the first time, and no coding experience is required! We will use **Visual Studio Code (VS Code)**.

### Easiest Way to Install Prerequisites 
Before running the "car" (this app), you need to install some "engines". Here is the easiest way to get them:
1. **Python**: Open the **Microsoft Store** on Windows, search for "Python" (e.g., Python 3.12), and click **Install**.
2. **Node.js**: Go to [nodejs.org](https://nodejs.org/), download the "LTS" (Long Term Support) installer for Windows, and run it. Keep clicking "Next" to install it.
3. **Redis**: For Windows, the easiest way is to use a pre-compiled version like [Memurai](https://www.memurai.com/) (Developer version is free). Download the installer and click "Next" until it finishes. It will run in the background automatically.

### Running the Project using VS Code Terminal

Open the `Hospital Management` project folder in **VS Code**. 

**How to use the Terminal in VS Code:**
- Go to the top menu and click **Terminal** > **New Terminal**.
- A panel will open at the bottom. You can open multiple terminal tabs by clicking the **`+`** icon in that panel.

### Step 1: Backend Setup (The Brain)
1.  Open your first **New Terminal** in VS Code.
2.  Install the necessary background libraries by typing this and pressing **Enter**:
    ```bash
    pip install -r backend/requirements.txt
    ```
3.  **Create the Database** (the digital filing cabinets) by typing:
    ```bash
    python backend/init_db.py
    ```
    *Note: This creates a default Admin user: `admin@hospital.com` / `admin123`.*
4.  **Start the Backend**:
    ```bash
    python backend/app.py
    ```
    *Keep this terminal tab open!* You will see it running on `http://localhost:5000`.

### Step 2: Frontend Setup (The Face)
1.  Click the **`+`** icon in the terminal panel to open a **second terminal tab**.
2.  Go into the frontend folder by typing:
    ```bash
    cd frontend
    ```
3.  Install the interface tools by typing:
    ```bash
    npm install
    ```
4.  **Start the Website**:
    ```bash
    npm run dev
    ```
    *Keep this terminal tab open!* It will give you a link, usually `http://localhost:5173`. You can hold the `Ctrl` key on your keyboard and click the link to open it in your browser.

### Step 3: Background Worker (The Helper)
*This is required for features like daily reminders and scheduled reports to work.*
1.  Click the **`+`** icon in the terminal panel again to open a **third terminal tab**.
2.  Go into the backend folder:
    ```bash
    cd backend
    ```
3.  Start the background task worker:
    ```bash
    celery -A celery_worker.celery worker --pool=solo --loglevel=info
    ```
4.  Optionally, start the scheduler for daily reminders and monthly reports in a **fourth terminal tab**:
    ```bash
    celery -A celery_worker.celery beat --loglevel=info
    ```

---

## 🔄 How to Reset & Reinitialize the Project (Fresh Start)

If you want to **wipe all existing data** (appointments, patients, doctors, treatments) and start completely fresh, follow these steps:

> ⚠️ **Warning**: This permanently deletes ALL data from the database. This cannot be undone.

### Step 1: Stop All Running Servers
Stop all running terminals — the Flask backend, the frontend dev server, and any Celery workers. Press **`Ctrl + C`** in each terminal tab.

### Step 2: Reset the Database
Open a terminal in VS Code, navigate to the `backend` folder, and run the reset script:
```bash
cd backend
python reset_db.py
```
You will see:
```
Dropping all tables...
Database reset complete.
```

### Step 3: Reinitialize the Project
After the reset, start the application again from scratch:

1. **Start the Backend** (creates the DB tables and default admin on first run):
    ```bash
    python app.py
    ```
    *Default Admin credentials: `admin@hms.com` / `admin123`*

2. **Start the Frontend** (in a new terminal tab):
    ```bash
    cd frontend
    npm run dev
    ```

3. **Start the Celery Worker** (in a new terminal tab):
    ```bash
    cd backend
    celery -A celery_worker.celery worker --pool=solo --loglevel=info
    ```

The system is now clean and ready for fresh use!

---

## 🎮 Usage Scenarios

### Scenario A: A New Patient Needs a Checkup
1.  **Register**: Go to the website. Click "Register". Enter your name, email, and password.
2.  **Login**: Use those details to log in.
3.  **Book**: You see a dashboard. Search for "Cardiologist".
4.  **Select**: You see "Dr. Smith". Click "Book Now". Pick a time (e.g., tomorrow at 10 AM).
5.  **Done**: Your appointment status is "Booked".

### Scenario B: The Doctor's Day
1.  **Login**: Dr. Smith logs in.
2.  **View**: He sees "John Doe" has an appointment at 10 AM tomorrow.
3.  **Treat**: When John arrives, Dr. Smith clicks "Complete" on the appointment.
4.  **Prescribe**: He types: "Diagnosis: Mild Fever. Prescription: Paracetamol." and saves.
5.  **Finish**: The appointment moves to history.

### Scenario C: The Admin's Oversight
1.  **Login**: The Admin logs in.
2.  **Check**: Sees "Total Appointments: 50".
3.  **Hire**: Hires a new "Neurologist" by clicking "Add Doctor".

---

## 🛠 Tech Stack (For Geeks)
- **Backend**: Flask (Python) - Fast and reliable.
- **Frontend**: Vue.js 3 - Smooth and modern interface.
- **Database**: SQLite - Simple and file-based.
- **Styling**: Bootstrap 5 - Looks good on mobile and desktop.
- **Tasks**: Celery + Redis - Handles heavy lifting in the background.

