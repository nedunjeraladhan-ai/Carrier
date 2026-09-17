# Carrier Assistant

A domain-specific chatbot built with **Flask + Gemini 3.1 Flash-Lite**.

## Features

- No login or registration
- Temporary, session-based chat history
- Separate chat session for each browser/device session
- Domain restriction configured in `config.py`
- Gemini API key loaded from `.env`
- Modern responsive UI for mobile, tablet, laptop and desktop
- Easy chatbot title/domain/theme customization
- Render + Gunicorn deployment
- Configurable PORT

## Project structure

```text
Carrier-Assistant/
├── app.py
├── config.py
├── .env
├── requirements.txt
├── README.md
└── templates/
    └── index.html
```

## 1. Install Python

Python 3.10+ is recommended.

## 2. Install dependencies

Open PowerShell in the project folder:

```powershell
pip install -r requirements.txt
```

## 3. Add your Gemini API key

Open `.env`:

```env
GEMINI_API_KEY=YOUR_GEMINI_API_KEY_HERE
FLASK_SECRET_KEY=YOUR_RANDOM_SECRET
```

Replace the placeholder with your Gemini API key.

Never commit `.env` to GitHub.

## 4. Run locally

```powershell
python app.py
```

Open:

```text
http://127.0.0.1:5000
```

## 5. Customize the chatbot

Open `config.py` and edit:

- `CHATBOT_TITLE`
- `DOMAIN`
- `SYSTEM_PROMPT`
- `BEHAVIOR`
- `WELCOME_MESSAGE`
- `PORT`
- `THEME`
- `UI_STYLE`

The model is intentionally instructed to answer only questions related to the configured domain.

## 6. Temporary session history

The application stores recent conversation turns in Flask's session cookie.

Important:
- There is no account/login system.
- Different browser/device sessions have independent histories.
- Do not put sensitive personal information into the chat.
- For production at larger scale, use server-side session storage (such as Redis) if you need stronger server-managed session handling.

## 7. Render deployment

Push the project to GitHub, then create a new **Web Service** on Render.

Build command:

```text
pip install -r requirements.txt
```

Start command:

```text
gunicorn app:app
```

Add these Render environment variables:

```text
GEMINI_API_KEY = your_real_gemini_api_key
FLASK_SECRET_KEY = a_long_random_secret
```

Do not hard-code your real API key in `app.py`, `config.py`, or `README.md`.

Render provides the `PORT` environment variable. To use it automatically, update the `PORT` setting in `config.py` to:

```python
PORT = int(os.getenv("PORT", 5000))
```

and add this import at the top:

```python
import os
```

The current package already supports Gunicorn.

## Model note

The application requests:

```text
gemini-3.1-flash-lite
```

If your Google AI account does not have access to that model, check the currently available Gemini models for your API key and replace the model name in `app.py`.

## Security note

The Flask development server is for local testing. Use Gunicorn on Render.

Also change `FLASK_SECRET_KEY` to a strong random value before deployment.

## Custom UI styles

The UI is intentionally kept in `templates/index.html` so each chatbot title can have a different visual identity.

You can create additional styles by changing the CSS variables and the header/logo design while keeping the Flask backend unchanged.

## Domain-specific behavior

Domain control is implemented in the generated prompt. This is an application-level instruction, not a perfect security boundary. For high-stakes or strict production applications, add server-side domain classification/validation before sending a question to the model.
