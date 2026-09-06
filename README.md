<div align="center">

<pre>
  ██╗   ██╗ ██████╗ ██╗  ██╗██╗  ██╗   ██╗
  ██║   ██║██╔═══██╗╚██╗██╔╝██║  ╚██╗ ██╔╝
 ██║   ██║██║   ██║ ╚███╔╝ ██║   ╚████╔╝
╚██╗ ██╔╝██║   ██║ ██╔██╗ ██║    ╚██╔╝
 ╚████╔╝ ╚██████╔╝██╔╝ ██╗███████╗██║
  ╚═══╝   ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝
</pre>

**AI-powered platform to practice and improve your presentation skills**

[![Tech Stack](https://skillicons.dev/icons?i=java,spring,react,ts,vite,tailwind,python,fastapi,opencv,postgres,gradle,docker)](https://skillicons.dev)

</div>

---

## 🎤 About the project

**VoxLy** helps you become a better speaker by turning every practice session into structured, actionable feedback. Record or upload a presentation video and the platform automatically **transcribes** your speech, **analyzes your body language**, and generates **AI-driven feedback** with a per-session score, so you can track your progress over time instead of guessing what to improve.

Practicing presentations alone is hard: you can't watch yourself speak and judge your posture, gestures, and pace at the same time. VoxLy closes that gap by combining speech-to-text, computer-vision gesture detection, and an LLM coach into a single, simple workflow: upload a video, get a report.

## ✨ Features

- **Video upload & processing**: record or upload presentation videos (MP4, up to 100MB).
- **AI transcription**: automatic speech-to-text powered by OpenAI Whisper, with multi-language support (English/Spanish).
- **Body-language analysis**: computer-vision pose and hand tracking (MediaPipe) detects up to 4 people per video and flags issues like crossed arms, hands in pockets, fidgeting, face/hair touching, rigid arms, slouching, and hands behind the back.
- **Automated scoring**: a rule-based engine deducts points per detected gesture based on severity, duration, and frequency, producing a clear per-person score.
- **Annotated video output**: renders the original video back with pose/gesture overlays for visual, timestamped feedback.
- **AI-generated feedback**: GPT-based evaluation summarizes strengths, weaknesses, and suggestions for each session.
- **Session management & progress tracking**: create, organize, and revisit practice sessions, and follow your improvement across time.
- **Authentication & personalization**: secure JWT-based login, account settings, and password reset.

## 🏗️ Architecture

```
+---------------------------+
|         frontend         |   React 19 . TypeScript . Vite . Tailwind CSS
|   (dashboard, sessions,  |
|   recorder, progress)    |
+-------------+-------------+
              | REST
+-------------v-------------+        +----------------------------+
|          backend          |  REST  |         ai service         |
|   Java 21 . Spring Boot   +------->|   Python . FastAPI         |
|  auth . sessions . files  |<-------+   MediaPipe . OpenCV       |
|  evaluation . feedback    |        |   OpenAI Whisper/GPT       |
+-------+-----------+-------+        +--------------+-------------+
        |           |                               |
+-------v----+ +-----v------+              pose / gesture / speech
| PostgreSQL | | Cloudflare |                     analysis
|   (Neon)   | | R2 (files) |
+------------+ +------------+
```

The **backend** is the core API: it handles authentication, sessions, and orchestrates calls to the **AI service**, which performs speech transcription and computer-vision gesture analysis and returns structured results (transcript, gesture events, score, annotated video). Video files are stored in **Cloudflare R2**, structured data in **PostgreSQL**, and the **frontend** talks to the backend over REST.

## 📦 Project structure

```
voxly/
├── backend/    # Java Spring Boot API (auth, sessions, evaluation, feedback, progress)
├── frontend/   # React + TypeScript SPA (dashboard, sessions, recorder, progress)
├── ai/         # Python/FastAPI service - transcription & body-language analysis
└── scripts/    # Dev & setup helpers
```

### 🖥️ `frontend` - Web interface
The SPA users interact with: dashboard, session list and detail, new-session recorder, progress charts, and account/personalization.
- **Stack:** React 19, TypeScript, Vite, Tailwind CSS, React Router.

### ⚙️ `backend` - API & business logic
The main API. Handles identity (JWT auth, OAuth2), session lifecycle, file uploads, evaluation/feedback orchestration, and progress aggregation.
- **Stack:** Java 21, Spring Boot, Spring Security, Hibernate/JPA, Flyway, PostgreSQL, AWS S3 SDK (Cloudflare R2), Quartz.

### 🧠 `ai` - Presentation analysis service
Processes uploaded videos: extracts audio for Whisper transcription, runs MediaPipe pose/hand tracking for gesture detection, scores the presentation, and can render an annotated output video.
- **Stack:** Python, FastAPI, OpenCV, MediaPipe, OpenAI API (Whisper + GPT).

## 🛠️ Tech stack

| Layer | Technologies |
|---|---|
| Frontend | React 19 · TypeScript · Vite · Tailwind CSS |
| Backend | Java 21 · Spring Boot · Spring Security · Hibernate/JPA · Flyway |
| AI service | Python · FastAPI · OpenCV · MediaPipe · OpenAI (Whisper + GPT) |
| Data & storage | PostgreSQL (Neon) · Cloudflare R2 |
| Infra | Docker · Gradle |

## 🚀 Getting started

### Prerequisites
- **Java 21+**
- **Node.js 18+** and pnpm/npm
- **Python 3.10+** and FFmpeg
- **Docker** (for local infrastructure, optional)

### Setup

```bash
git clone https://github.com/Dexaroz/VoxLy
cd VoxLy

cp .env.example .env
# fill in .env with your credentials (DB, OpenAI, Cloudflare R2, JWT secret...)
```

Run each service (see `make help` / `Makefile` targets):

```bash
make backend   # Spring Boot API
make ai        # FastAPI analysis service
make frontend  # Vite dev server
```

See `ENV_SETUP.md` for detailed environment configuration.

---

<div align="center">

*VoxLy: practice, get feedback, present better.*

</div>
