# ATS Resume Checker

An AI-assisted resume analysis application that evaluates a resume against a job description and provides an ATS score, keyword insights, and improvement feedback.

## Live deployment

- **Backend API:** https://ats-resume-checker-oghk.onrender.com
- **API documentation (Swagger):** https://ats-resume-checker-oghk.onrender.com/docs
- **Backend health endpoint:** https://ats-resume-checker-oghk.onrender.com/api/v1/health
- **Frontend (Streamlit):** https://ats-resume-checker6529.streamlit.app/

> The backend is hosted on Render. The free Render instance may spin down after inactivity, so the first request can take longer while it starts.

## Features

- Upload resumes in PDF, DOC, and DOCX formats.
- Extract resume details such as skills, education, experience, projects, and certifications.
- Compare resume content with a supplied job description.
- Generate an ATS score and category-level feedback.
- Identify relevant and missing keywords.
- Provide AI-assisted parsing and recommendations using Groq.
- Sign in and manage a user account with Supabase authentication.
- Save and review previous analyses.
- Generate downloadable PDF reports (where enabled in the app).

## Technology stack

- **Frontend:** Streamlit
- **Backend:** FastAPI and Uvicorn
- **Resume parsing / NLP:** Python and spaCy
- **AI parsing:** Groq API
- **Authentication and database:** Supabase
- **HTTP/API communication:** Requests and HTTPX

## Repository structure

```text
ATS-RESUME-CHECKER/
├── backend/
│   ├── api/              # API routes and authentication
│   ├── core/             # Application configuration
│   ├── database/         # Supabase database operations
│   ├── services/         # Resume parsing, scoring and report services
│   └── main.py           # FastAPI application entry point
├── frontend/
│   ├── services/         # Supabase and backend API clients
│   ├── views/            # Streamlit application views
│   └── streamlit_app.py  # Streamlit application entry point
└── README.md
```

## Run locally

### 1. Clone the repository

```bash
git clone https://github.com/karanpruthi1903-pixel/ATS-RESUME-CHECKER.git
cd ATS-RESUME-CHECKER
```

### 2. Create and activate a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

macOS / Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install backend dependencies

```bash
pip install -r backend/requirements.txt
python -m spacy download en_core_web_sm
```

Install the frontend dependencies used by the Streamlit app if they are not already included in your environment.

### 4. Configure environment variables

Create a root-level `.env` file. Do not commit this file.

Configure the values required by your deployment:

```env
SUPABASE_URL=https://YOUR_PROJECT_REF.supabase.co
SUPABASE_KEY=YOUR_BACKEND_SUPABASE_SECRET_OR_SERVICE_ROLE_KEY
SUPABASE_ANON_KEY=YOUR_SUPABASE_PUBLIC_PUBLISHABLE_OR_ANON_KEY
SUPABASE_JWT_SECRET=YOUR_LEGACY_JWT_SECRET_IF_REQUIRED
GROQ_API_KEY=YOUR_GROQ_API_KEY
```

The Streamlit frontend also reads its backend URL from `frontend/.streamlit/secrets.toml`:

```toml
[backend]
url = "http://127.0.0.1:8000"
```

Configure the frontend's Supabase settings in its Streamlit secrets or environment configuration as required by `frontend/services/supabase_client.py`.

**Security:** Never commit `.env`, `.streamlit/secrets.toml`, Supabase secret/service-role keys, JWT secrets, or Groq API keys. Use environment variables or the hosting provider's secret manager.

### 5. Start the backend

From the repository root:

```bash
uvicorn backend.main:app --host 127.0.0.1 --port 8000
```

Backend API: http://127.0.0.1:8000  
Interactive API docs: http://127.0.0.1:8000/docs

### 6. Start the Streamlit frontend

Open a second terminal, move into the frontend directory, and run:

```bash
cd frontend
streamlit run streamlit_app.py
```

Streamlit normally opens at http://localhost:8501.

## Deployment

### Backend on Render

The deployed backend uses:

- **Build command:** `pip install -r backend/requirements.txt && python -m spacy download en_core_web_sm`
- **Start command:** `uvicorn backend.main:app --host 0.0.0.0 --port $PORT`

Set the required environment variables in the Render service's Environment settings. Do not place secret values in source code or GitHub.

### Frontend

Deploy the Streamlit frontend separately using Streamlit Community Cloud or another supported hosting provider. Configure its Supabase settings and set the backend URL to:

```text
https://ats-resume-checker-oghk.onrender.com
```

The public Streamlit frontend is available at https://ats-resume-checker6529.streamlit.app/. The Streamlit Community Cloud deployment is connected to the ATS-RESUME-CHECKER repository, branch main, with frontend/streamlit_app.py as its entry point.

## API endpoints

The deployed API's interactive documentation lists the available routes:

https://ats-resume-checker-oghk.onrender.com/docs

## Notes

- Resume uploads are limited by the backend's configured file-size and file-type restrictions.
- The Render free instance may sleep after inactivity.
- Keep all credentials in local environment files or hosting-provider secrets, never in Git.
