Video Link:https://drive.google.com/file/d/1n3JorfxtPVB_Dt-y06dc0h8M0N8w-VSz/view?usp=sharing


So, as you can see, I was told to build a Bench Management System for employees to make it easier for the person to upload the Excel sheet which is here (bt.xlsx) and extract the data from bt.xlsx to show it in the UI. The sheet contains all details of employees, but we needed only some columns in the UI, which were needed as we required only some important details.

We have made the backend using Python Flask and the frontend using React. So, it is a full-stack project which extracts data from the sheet and puts it into a MySQL table. It extracts data in an intelligent way that matches the columns of the Excel sheet with the columns of the MySQL table and inserts the data into the MySQL table. This data is shown in specific columns in the frontend.

We have given a clean, beautiful UI which fetches the data dynamically from each column and shows it as options in the filter.

Also, you can see each row has a toggle button which is used to change from "Bench" to "NOT IN BENCH," and also for each row/employee, we have a download resume button which is used for downloading the employee's resume.

So, we can see in the video that the employee which I had marked as "Not in Bench" can be displayed in the Excel sheet.

Also, when I delete any column in the Excel sheet and upload it, it gets the data and shows it in the UI, which shows that it is an intelligent system and it can fetch the required data from the given Excel sheet irrespective of how many columns there are. Our aim was also to allow different column names, and it recognizes and matches them with the column names of the MySQL table, inserts the data, and shows it in the UI.

# Bench Management System

A full-stack application for managing employee bench status with Excel upload capabilities, dynamic filtering, and resume downloads.

##  Project Overview

This system was designed to simplify bench management by allowing HR to:
1. Upload Excel sheets (`bt.xlsx`) containing employee data
2. Automatically extract and display only the required columns in a clean UI
3. Dynamically filter employees by grade, location, skills, and skill buckets
4. Toggle bench status with real-time updates
5. Download employee resumes directly from the interface

The intelligent backend:
- Matches Excel columns with MySQL table columns regardless of column order
- Handles missing columns gracefully
- Preserves data integrity during updates
- Exports updated statuses back to Excel

##  Tech Stack
- **Frontend**: React, Vite, TanStack Query, Axios
- **Backend**: Python Flask, MySQL
- **Data Processing**: Pandas, OpenPyXL

##  Key Features

### Intelligent Excel Processing
- Auto-detects and maps Excel columns to database fields
- Handles variations in column names (e.g., "PS No" → "PS_No")
- Preserves original Excel structure while updating statuses

### Dynamic Employee Management
```jsx
// Example from EmployeeList.jsx
const toggleStatus = (ps_no) => {
  axios.post(`/employees/toggle-status/${ps_no}`)
    .then(response => {
      setFilteredEmployees(prev => 
        prev.map(emp => emp.PS_No === ps_no 
          ? {...emp, Status: response.data.new_status} 
          : emp
        )
      );
    });
};











A full‑stack bench management solution for HR/staffing teams. Upload Excel rosters, normalize inconsistent columns, persist to MySQL, and manage bench status from a clean React UI with fast filters, pagination, and resume downloads.



---

## Highlights
- Intelligent Excel ingestion with column normalization and duplicate‑safe upserts
- Dynamic filters: Grade, Base Location, Skills, Skill Bucket
- Real‑time bench status toggle (Bench ↔ Not in Bench)
- Resume download handling (Google Drive links → direct download)
- Export refreshed Excel with latest statuses
- Responsive UI with client‑side pagination (20/50/100)

---

## Architecture
```text
React (Vite) SPA
  └─ Axios + TanStack Query → Flask API (REST)
                              └─ Pandas + OpenPyXL → MySQL (benchresource.employee_data)
```

Data flow (core loop):
1) Upload Excel → save to `backend/uploads/` → transform → insert/update MySQL
2) List employees + populate filters → client‑side refine + paginate
3) Toggle status → update DB and original Excel → reflect instantly in UI
4) Export → serve Excel with updated statuses or selected profiles

---

## Tech Stack
- Frontend: React 19, Vite 6, React Router DOM 7, TanStack React Query 5, Axios 1
- Backend: Python 3.x, Flask 2.3, Flask‑CORS, Pandas 2, OpenPyXL 3, mysql‑connector‑python 8
- Database: MySQL 8 (`benchresource`)

---

## Project Structure
```text
LTTS Bench Management System/
├── README.md                     
├── readme2.md                    # GitHub‑ready README (this file)
├── project_analysis.md           # Resume/portfolio analysis
└── 24 March 2025/
    ├── Feature List.txt
    ├── PROJECT_DOCUMENTATION.md
    ├── RESUME_SUMMARY.md
    ├── backend/
    │   ├── app.py                # Flask app, endpoints, ingestion/export
    │   ├── sqlfile.sql           # Schema (benchresource.employee_data)
    │   ├── test_backend.py       # Requests-based smoke tests
    │   └── uploads/              # Uploaded and generated Excel files
    └── frontend/
        ├── src/
        │   ├── components/       # FileUpload, EmployeeList, Filters
        │   ├── services/api.jsx  # Axios client
        │   └── styles/           # CSS
        └── package.json
```

---

## Prerequisites
- Node.js 18+ and npm
- Python 3.10+
- MySQL 8 (local or hosted)

---

## Quickstart

### Backend
```bash
cd "24 March 2025/backend"
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt

# Configure database access in app.py (db_config) or via environment
python app.py
```

Default server: `http://127.0.0.1:5000`

### Frontend
```bash
cd "24 March 2025/frontend"
npm install
npm run dev
```

Default dev server: `http://localhost:5173`

---

## Configuration

### Backend environment (optional)
If you prefer environment variables over hardcoding in `backend/app.py`:

```bash
# Example .env (not committed)
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=benchresource
```

Update `app.py` to read from environment before publishing. Replace any sample password strings.

### Frontend environment (optional)
Prefer a configurable API base URL (e.g., `VITE_API_BASE_URL`) and read it in `src/services/api.jsx`.

---

## Usage
1) Upload the Excel file on the upload screen (`FileUpload.jsx`).
2) Review the table (`EmployeeList.jsx`), filter by Grade/Location/Skills/Skill Bucket, and change page size.
3) Toggle Bench status per row. The UI updates immediately; data is persisted.
4) Download resumes (direct download for Google Drive links) or export refreshed Excel.

Sample Excel helpers live in `24 March 2025/backend/` for quick testing.

---

## API Endpoints (summary)
Base URL: `http://127.0.0.1:5000`

| Method | Endpoint                           | Description                              |
| ------ | ---------------------------------- | ---------------------------------------- |
| POST   | `/upload`                          | Upload an Excel file for ingestion       |
| GET    | `/employees`                       | Get all employees                        |
| GET    | `/employees/filter`                | Filter by `cadre`, `location`, `skills`  |
| GET    | `/dropdown-options`                | Distinct values for dropdown filters     |
| POST   | `/employees/toggle-status/<ps_no>` | Toggle bench status for a PS number      |
| GET    | `/employees/export-excel`          | Export Excel with updated statuses       |
| POST   | `/employees/download`              | Export Excel for selected PS numbers     |

---

## Database Model
Table: `benchresource.employee_data`

| Column                | Type                    |
| --------------------- | ----------------------- |
| `PS_No`               | VARCHAR(20) PRIMARY KEY |
| `Employee_Name`       | VARCHAR(255)            |
| `Skill_Matrix_System` | LONGTEXT                |
| `Grade`               | VARCHAR(255)            |
| `Base_Location`       | VARCHAR(255)            |
| `Profile`             | TEXT                    |
| `Status`              | VARCHAR(255)            |
| `Skill_Bucket`        | VARCHAR(255)            |

---

## Testing
- Backend smoke tests: run `python 24 March 2025/backend/test_backend.py` while the server is running.
- Frontend: `npm run lint` for static checks (extend with Jest/RTL for component tests).

---

## Troubleshooting
- MySQL connection errors: verify credentials and network access; ensure database `benchresource` exists or let startup create it.
- Excel ingestion errors: confirm file is a valid Excel workbook and common columns exist (`PS No`, `Employee Name`, etc.).
- CORS issues: allow the frontend origin in Flask‑CORS or run both locally on default ports.
- Direct resume links: for Google Drive, ensure sharing permissions allow download.

---

## Security Notes
- Do not commit real credentials; prefer environment variables/secret managers.
- Replace sample passwords in `backend/app.py` before publishing.
- Restrict CORS to the deployed frontend origin in production.

---

## Roadmap (suggested)
- Authentication & role‑based access (HR admin, viewer)
- Async/background ingestion for large files with progress updates
- Audit logs for status changes and exports
- Analytics (bench trends, skill heatmaps) and CSV/PDF exports
- Cloud storage integration for resumes and attachments

---

## Credits
- Frontend: React + Vite
- Backend: Flask + Pandas + OpenPyXL
- Database: MySQL
