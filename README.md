# Resume ATS Checker

A Streamlit app that accepts a resume (PDF, DOCX, or TXT), extracts its text, and uses Google Gemini Flash to estimate ATS readiness and suggest improvements.

> **Important:** The ATS score is an AI-generated estimate, not a score from a real applicant tracking system and not a guarantee of selection. Review suggestions carefully and never add skills, achievements, or numbers that are not true.

## Features

- Resume upload: PDF, DOCX, and TXT (maximum 5 MB)
- Optional job description for role-specific feedback
- Estimated score from 0 to 100
- Strengths, prioritized improvements, formatting checks, and potential keyword gaps
- Truth-preserving summary and bullet rewrite suggestions
- Download analysis as JSON
- API key entered in the UI locally or supplied through Streamlit Secrets in deployment

## 1. Requirements

- Python 3.10 or newer
- A Google Gemini API key
- Internet access for Gemini requests

Get a Gemini API key through Google AI Studio: https://aistudio.google.com/apikey

## 2. Run locally

Open a terminal in this project folder:

```bash
python -m venv .venv
```

Activate the environment.

**Windows PowerShell:**
```powershell
.\.venv\Scripts\Activate.ps1
```

**Windows Command Prompt:**
```bat
.venv\Scripts\activate.bat
```

**macOS/Linux:**
```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Run the app:

```bash
streamlit run app.py
```

Paste your Gemini API key into the sidebar. Alternatively, set an environment variable before starting Streamlit.

**PowerShell:**
```powershell
$env:GEMINI_API_KEY="your-api-key"
streamlit run app.py
```

Do not share or commit your API key.

## 3. Push to GitHub

Create an empty repository on https://github.com/new. Do not add a README there if you already have this README locally.

Create a `.gitignore` file in your project folder with:

```gitignore
.venv/
__pycache__/
*.py[cod]
.streamlit/secrets.toml
.env
.DS_Store
```

Then run these commands in the project folder, replacing the URL with your own repository URL:

```bash
git init
git add app.py README.md requirements.txt .gitignore
git commit -m "Build resume ATS checker"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
git push -u origin main
```

If Git asks you to sign in, complete GitHub's browser authentication flow. Never upload `secrets.toml`, `.env`, or API keys.

## 4. Deploy on Streamlit Community Cloud

1. Push the project to GitHub.
2. Open https://share.streamlit.io/ and sign in with GitHub.
3. Select **Create app**, choose your repository and `main` branch, and set the main file path to `app.py`.
4. Open the app's **Advanced settings / Secrets** section and add:

   ```toml
   GEMINI_API_KEY = "your-real-api-key"
   ```

5. Deploy the app. The app reads the `GEMINI_API_KEY` environment variable; Streamlit Cloud Secrets exposes configured secrets to the app environment in supported deployments. If your environment does not expose it automatically, update the key-loading code to read `st.secrets["GEMINI_API_KEY"]` with a safe fallback to the environment variable.
6. After deployment, open the app URL and test with a non-sensitive sample resume.

## Supported files and limitations

- PDF: text is extracted from the PDF layer. Scanned/image-only PDFs need OCR first.
- DOCX: paragraphs and tables are extracted.
- TXT: UTF-8 text is read.
- Files larger than 5 MB are rejected. Extracted text is limited to 30,000 characters before sending to Gemini.
- Complex columns, graphics, and visual layout may not be represented accurately by text extraction.
- The resume text and optional job description are sent to Google's Gemini service for analysis. Do not upload information unless you are comfortable processing it through that service. Check the applicable Google terms and privacy settings for your account.
- AI results can be wrong or incomplete. Human review is recommended.

## Troubleshooting

- **Missing API key:** add it in the sidebar for local use, or configure `GEMINI_API_KEY` in Streamlit Cloud.
- **Invalid model / model not found:** check the model name available to your Google AI Studio project and update the model field in the sidebar.
- **Quota or API errors:** check the key, project, billing/quota, and current Gemini API availability.
- **PDF has no text:** use a text-based PDF or OCR the scanned document before uploading.
- **Dependency issues:** use Python 3.10+ and reinstall with `pip install -r requirements.txt`.
