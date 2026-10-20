# ForeignHire AI — AI-Powered Recruitment & Candidate Matching Platform

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Django](https://img.shields.io/badge/Django-5.1.5-green.svg)
![Gemini](https://img.shields.io/badge/AI-Google%20Gemini-purple.svg)
![CrewAI](https://img.shields.io/badge/Agents-CrewAI-orange.svg)

A Django web application that automates the first stages of hiring: candidates upload a resume against a job posting, the system extracts and structures the resume with Google Gemini, scores the candidate–job fit, and presents the results in a recruiter dashboard with analytics, candidate detail pages, shortlisting, and downloadable PDF scorecards.

> This repository is the public GitHub version of the HireFlow / ForeignHire AI recruitment project.

## Overview

Recruiters post jobs (title, department, description, requirements, location). Candidates apply by uploading a PDF resume for a specific job. On upload, the application:

1. Extracts resume text with **pdfplumber** (with **PyPDF2** as fallback).
2. Parses the resume with **Gemini** into structured JSON — skills, total experience, education, key achievements, and red flags.
3. Scores the match with a Gemini-powered **JobMatcher** — fit score (0–100), matched skills, missing skills, strengths, weaknesses, and a hiring recommendation.
4. Stores everything on the `Candidate` record so recruiters can review, filter, and shortlist from the dashboard.

An experimental **CrewAI** crew (`api/agents/crew_agent.py`) also defines four specialized agents — Skills Extraction, Job Matching, Gap Analysis, and Interview Question Design — as the multi-agent extension of the same pipeline.

## Key Features

- **Job postings** — create and list jobs with department, requirements, and location (`Job` model); includes a one-click sample-job creator for demos
- **Resume upload & parsing** — PDF text extraction (pdfplumber → PyPDF2 fallback) followed by Gemini-based structured parsing
- **AI fit scoring** — per-candidate fit score, matched/missing skills, strengths, weaknesses, and recommendation, with a rule-based fallback if the AI call fails
- **Recruiter dashboard** — overview of jobs and candidates
- **Analytics page** — aggregate view of candidate/job data
- **Candidate detail pages** — full parsed profile and AI analysis per candidate
- **Shortlisting** — mark candidates as shortlisted
- **PDF scorecards** — generate a downloadable candidate scorecard PDF with ReportLab
- **CrewAI agents (experimental)** — four-agent crew definition for skills extraction, matching, gap analysis, and interview-question generation

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Django 5.1.5 |
| AI | Google Gemini (`google-generativeai`), CrewAI + LangChain (experimental crew) |
| Resume parsing | pdfplumber, PyPDF2 |
| PDF generation | ReportLab |
| Frontend | Django templates (HTML/CSS) |
| Database | SQLite (development) |
| Static files | WhiteNoise |

## Project Structure

```
foreignhire-ai/
├── backend/            # Django project settings, URLs, WSGI/ASGI
├── api/                # Main application
│   ├── agents/
│   │   ├── resume_parser.py   # PDF extraction + Gemini resume parsing
│   │   ├── job_matcher.py     # Gemini fit-score analysis
│   │   └── crew_agent.py      # CrewAI multi-agent crew (experimental)
│   ├── templates/      # home, dashboard, analytics, candidate detail,
│   │                   # upload_resume, scorecard pages
│   ├── models.py       # Job and Candidate models
│   ├── views.py        # Upload, dashboard, analytics, shortlist, scorecard
│   └── urls.py
├── media/              # Uploaded resumes and banner assets
├── db.sqlite3          # Development database
└── manage.py
```

## Getting Started

### Prerequisites

- Python 3.10+
- A Google Gemini API key

### Installation

```bash
git clone https://github.com/datascientist970/foreignhire-ai.git
cd foreignhire-ai

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install django google-generativeai pdfplumber PyPDF2 reportlab whitenoise
# Optional, for the CrewAI experiment:
pip install crewai langchain

python manage.py migrate
python manage.py runserver
```

Open http://127.0.0.1:8000/ in your browser.

### Configuration

Set your Gemini API key where the agent modules read it (`api/agents/resume_parser.py` and `api/agents/job_matcher.py` configure `google.generativeai`). Do not commit a real API key to version control — prefer an environment variable in your own deployment.

## Usage

1. Open the home page and create a job (or use **Create Sample Job** for a quick demo).
2. Go to **Upload Resume**, select the job, enter candidate details, and upload a PDF resume.
3. The system parses the resume and calculates the AI fit score automatically.
4. Review results in the **Dashboard**, open a candidate's detail page, download the **PDF scorecard**, or **shortlist** the candidate.
5. Use the **Analytics** page for an aggregate overview.

## Author

**Muhammad Dawood** — AI & Data Science Engineer
- GitHub: https://github.com/datascientist970
- Location: Mardan, Pakistan

## License

This project is shared for learning and portfolio purposes. Add a license file before reusing it commercially.
