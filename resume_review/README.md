# AI Resume Reviewer

An intelligent resume screening tool that leverages **Groq LLM** to automatically parse, analyze, and rank resumes against a given job description. Built for recruiters and hiring managers to streamline the initial screening process.

## Features

- **Job Description Parsing** — Extracts structured information (role, required skills, preferred skills, experience, education, responsibilities) from any job description using LLM.
- **Resume Parsing** — Reads PDF and DOCX resumes and extracts candidate details (name, contact, skills, work experience, education, projects, certifications) using LLM.
- **Intelligent Matching** — Compares each parsed resume against the job requirements and generates a match score (0–100) with detailed reasoning.
- **Automated Ranking** — Sorts all candidates by match score and highlights the **top 2** candidates.
- **Multi-format Support** — Handles both `.pdf` and `.docx` resume files.

## Tech Stack

| Technology        | Purpose                                |
| ----------------- | -------------------------------------- |
| **Python 3.10+**  | Core language                          |
| **Groq API**      | LLM inference for parsing and matching |
| **Pydantic**      | Data validation and schema definition  |
| **PyPDF**         | PDF text extraction                    |
| **python-docx**   | DOCX text extraction                   |
| **python-dotenv** | Environment variable management        |

## Project Structure

```
resume_review/
├── main.py              # Main application script
├── README.md            # This file
├── resumes/             # Folder containing candidate resumes
│   ├── CHARLES MCTURLAND.pdf
│   ├── ROHIT KUMHAR.pdf
│   ├── SALVADOR SANZ.docx
│   └── TASIANA UKUEA.docx
└── .env                 # Environment variables (not tracked)
```

## Prerequisites

- Python 3.10 or higher
- A [Groq API key](https://console.groq.com/) (free tier available)

## Setup Instructions

### 1. Clone the repository

```bash
git clone <repository-url>
cd resume_review
```

### 2. Create a virtual environment (recommended)

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install groq pydantic pypdf python-docx python-dotenv
```

### 4. Configure environment variables

Create a `.env` file in the `resume_review/` directory:

```env
GROQ_API=your_groq_api_key_here
PROD_MODEL_NAME=llama-3.3-70b-versatile
```

> **Note:** You can use any Groq-compatible model. The `llama-3.3-70b-versatile` model is recommended for best results.

### 5. Add resumes

Place candidate resumes (`.pdf` or `.docx`) inside the `resumes/` folder.

### 6. Customize the job description (optional)

Open `main.py` and modify the `job_description` variable to match the role you're hiring for.

## Usage

Run the script from the `resume_review/` directory:

```bash
python main.py
```

### What happens

1. The script loads the job description and extracts structured requirements using Groq LLM.
2. It iterates through all resumes in the `resumes/` folder.
3. Each resume is parsed into a structured format (name, skills, experience, education, etc.).
4. The parsed resume is compared against the job requirements to produce a match score.
5. All candidates are ranked by score, and the **top 2** candidates are displayed.

### Sample Output

```
Processing: CHARLES MCTURLAND.pdf
Score: 85.0

Processing: ROHIT KUMHAR.pdf
Score: 72.5

Processing: SALVADOR SANZ.docx
Score: 91.0

Processing: TASIANA UKUEA.docx
Score: 64.0

TOP 2 CANDIDATES
SALVADOR SANZ - 91.0 %
{'matching_skills': ['Python', 'AWS', 'CI/CD', ...], 'missing_skills': ['Rust'], 'experience_met': True, 'verdict': 'Strong match for the SDE-I role.'}
CHARLES MCTURLAND - 85.0 %
{'matching_skills': ['Java', 'Microservices', ...], 'missing_skills': [], 'experience_met': True, 'verdict': 'Good fit with relevant experience.'}
```

## How It Works

### 1. Job Description Analysis

The script sends the job description to Groq LLM with a structured schema (Pydantic model) and receives a JSON response containing:

- `role` — Job title
- `required_skills` — Must-have skills
- `preferred_skills` — Nice-to-have skills
- `minimum_experience` — Years of experience required
- `education_requirements` — Degree/field requirements
- `responsibilities` — Key job responsibilities

### 2. Resume Parsing

Each resume file is read (PDF via `pypdf`, DOCX via `python-docx`) and sent to Groq LLM with a resume schema to extract:

- Name, email, phone
- Total experience in years
- Skills (gathered from all sections)
- Work experience (company, role, duration, description, skills used)
- Education
- Projects
- Certifications

### 3. Candidate Matching

The parsed job and resume are both sent to Groq LLM, which returns a `MatchResult` containing:

- `score` — Overall match percentage (0–100)
- `details` — Dictionary with matching skills, missing skills, experience check, and a final verdict

### 4. Ranking

All candidates are sorted by score in descending order. The top 2 candidates are printed to the console.

## Customization

### Changing the Job Description

Edit the `job_description` variable in `main.py`:

```python
job_description = """
Your new job description here...
"""
```

### Changing the LLM Model

Update the `PROD_MODEL_NAME` in your `.env` file:

```env
PROD_MODEL_NAME=mixtral-8x7b-32768
```

### Adjusting the Number of Top Candidates

Modify the slice in `main.py`:

```python
top_2 = all_results[:5]   # Show top 5 instead of top 2
```

## Limitations

- **API Dependency** — Requires an active internet connection and a valid Groq API key.
- **Rate Limits** — Groq free tier has rate limits; the script includes a 5-second delay between API calls to avoid throttling.
- **LLM Hallucination** — The LLM may occasionally invent or miss information. Always verify critical results.
- **Resume Formatting** — Complex layouts (tables, columns, images) may not be extracted perfectly.

## Troubleshooting

| Issue                         | Solution                                                                  |
| ----------------------------- | ------------------------------------------------------------------------- |
| `API key kaha hai bhai` error | Ensure `GROQ_API` is set in your `.env` file                              |
| `Model not found` error       | Ensure `PROD_MODEL_NAME` is set correctly in `.env`                       |
| No resumes processed          | Check that resumes are in the `resumes/` folder and are `.pdf` or `.docx` |
| Poor extraction quality       | Try a different Groq model (e.g., `llama-3.3-70b-versatile`)              |

## License

This project is for educational and demonstration purposes.

---

_Built with ❤️ using Groq LLM and Python._
