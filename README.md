# AI Resume Analyzer

A full-stack web app that scores a resume and returns strengths, weaknesses and targeted suggestions, using a Next.js frontend, a FastAPI backend and Google Gemini.

## Features

- Email and password sign-up and login, plus Google sign-in (Firebase Authentication)
- Upload a PDF or DOCX resume (5 MB limit), with an optional target job title and industry
- Text extraction on the backend with PyPDF2 (PDF) and python-docx (DOCX)
- Gemini (`gemini-2.0-flash`) analysis returning a score from 1 to 100, an overall analysis, strengths, weaknesses and suggestions with examples
- Uploaded files stored in Firebase Storage and analysis results saved to Cloud Firestore
- Dashboard with analysis history and a progress page with charts (Recharts)
- Settings page

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 13 (App Router), React 18, TypeScript, Tailwind CSS, shadcn/ui (Radix UI), Recharts |
| Auth and data | Firebase Authentication, Cloud Firestore, Firebase Storage |
| Backend | Python, FastAPI, Uvicorn, Pydantic |
| AI and parsing | Google Gemini (`google-genai`), PyPDF2, python-docx |

## Project Structure

```text
app/                  Next.js pages: landing, login, signup, dashboard (analyze, progress, settings)
components/ui/        shadcn/ui components
lib/firebase.ts       Firebase app, auth helpers
lib/resumeService.js  Calls the API, uploads to Storage, reads and writes Firestore
backend/main.py       FastAPI service
```

## API

| Method | Endpoint | Description |
|---|---|---|
| POST | `/analyze-resume` | Multipart form: `file`, optional `job_title`, `industry`. Returns `score`, `analysis`, `strengths`, `weaknesses`, `suggestions` |
| GET | `/health` | Health check |

## Getting Started

**Backend**

```bash
cd backend
pip install fastapi uvicorn pydantic python-multipart google-genai PyPDF2 python-docx
export GEMINI_API_KEY=your_key_here   # required; the API will not start without it
python main.py        # or: uvicorn main:app --reload
```

The API runs at `http://127.0.0.1:8000`.

**Frontend** (from the repository root)

```bash
npm install
npm run dev
```

Before `npm run dev`, copy `.env.example` to `.env.local` and fill in your own Firebase web app settings:

| Variable | Used by |
|---|---|
| `NEXT_PUBLIC_FIREBASE_API_KEY`, `NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN`, `NEXT_PUBLIC_FIREBASE_PROJECT_ID`, `NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET`, `NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID`, `NEXT_PUBLIC_FIREBASE_APP_ID` | Frontend (`lib/firebase.ts`) |
| `NEXT_PUBLIC_API_URL` (optional) | Frontend, if the backend is not on `http://localhost:8000` |
| `GEMINI_API_KEY` | Backend (`backend/main.py`) |

Open `http://localhost:3000`. Never commit `.env` or `.env.local`; both are in `.gitignore`.

## Team

Built as a team project by:

- Abhiram Kamarajugadda ([abhiramkamarajugadda](https://github.com/abhiramkamarajugadda))
- Nagasasank Palepu ([nagasasankpalepu](https://github.com/nagasasankpalepu))
- Shaik Ahmed Ali ([shaikahmed-ali](https://github.com/shaikahmed-ali))

## Author

**Abhiram Kamarajugadda**
GitHub: [abhiramkamarajugadda](https://github.com/abhiramkamarajugadda) | Portfolio: [abhiramkamarajugadda.github.io](https://abhiramkamarajugadda.github.io)
