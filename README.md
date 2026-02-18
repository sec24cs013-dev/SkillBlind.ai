# SkillBlind.AI

> **Skills First. Bias Blind.**
> A recruitment platform that strips identity from the equation. Our AI evaluates raw talent — no names, no photos, no unconscious bias.

![SkillBlind.AI](https://img.shields.io/badge/Next.js-14-black?style=flat-square&logo=next.js)
![Firebase](https://img.shields.io/badge/Firebase-10-orange?style=flat-square&logo=firebase)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-green?style=flat-square&logo=openai)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-3-blue?style=flat-square&logo=tailwindcss)

---

## What is SkillBlind.AI?

SkillBlind.AI is a bias-free hiring platform where candidates are evaluated purely on their skills and technical competency. The platform uses an LLM to:

- **Parse resumes** and extract skills, experience, certifications
- **Strip all PII** — names, emails, locations, gender pronouns
- **Score candidates** from 1–100 on technical fit
- **Present anonymous profiles** to recruiters in a clean, skill-first grid

---

## Features

| Feature | Description |
|---|---|
| 🔒 Zero PII | All personal identifiers stripped by the AI before saving |
| ⚡ AI Scoring | GPT-4o-mini assigns a 1–100 Job Fit Score |
| 🌿 Blind Review | Recruiters see skills, score, and evaluation — never names or photos |
| 🏅 Certifications | Extracted and displayed as badges |
| 🎯 Score Filtering | Recruiters can filter by 80+, 85+, 90+ fit scores |
| ✅ Shortlisting | One-click shortlist with instant toast notification |

---

## Tech Stack

- **Framework:** Next.js 14 (App Router)
- **Styling:** Tailwind CSS + inline CSS variables
- **Auth & Database:** Firebase Authentication + Firestore
- **AI:** OpenAI GPT-4o-mini via official SDK
- **Notifications:** react-hot-toast
- **Fonts:** Playfair Display, Lora, DM Mono

---

## Project Structure

```
skillblind-ai/
├── src/
│   ├── app/
│   │   ├── layout.tsx              # Root layout (Navbar, AuthProvider, Toaster)
│   │   ├── page.tsx                # Landing / home page
│   │   ├── globals.css             # Global styles, mesh animations, CSS vars
│   │   ├── login/
│   │   │   └── page.tsx            # Login page
│   │   ├── signup/
│   │   │   └── page.tsx            # Signup page (candidate / recruiter role select)
│   │   ├── candidate/
│   │   │   └── page.tsx            # Candidate dashboard (resume paste + AI analysis)
│   │   ├── recruiter/
│   │   │   └── page.tsx            # Recruiter dashboard (skill-first card grid)
│   │   └── api/
│   │       └── analyze-resume/
│   │           └── route.ts        # POST /api/analyze-resume (OpenAI integration)
│   ├── components/
│   │   ├── Navbar.tsx              # Sticky navbar with auth-aware links
│   │   ├── MeshBackground.tsx      # Animated gradient mesh background
│   │   ├── CandidateCard.tsx       # Skill card for recruiter dashboard
│   │   ├── ScoreCircle.tsx         # Circular SVG progress badge
│   │   └── Pill.tsx                # Skill tag pill component
│   └── lib/
│       ├── firebase.ts             # Firebase app + auth + db init
│       └── AuthContext.tsx         # Auth state provider + useAuth hook
├── .env.local.example              # Environment variable template
├── .gitignore
├── next.config.js
├── tailwind.config.js
├── tsconfig.json
├── package.json
└── README.md
```

---

## Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/skillblind-ai.git
cd skillblind-ai
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/) and create a new project
2. Enable **Authentication** → Email/Password sign-in
3. Create a **Firestore Database** (start in test mode for development)
4. Go to **Project Settings** → copy your web app config

### 4. Set up OpenAI

1. Go to [platform.openai.com](https://platform.openai.com/)
2. Create an API key with access to `gpt-4o-mini`

### 5. Configure environment variables

```bash
cp .env.local.example .env.local
```

Edit `.env.local` with your credentials:

```env
# Firebase
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id

# OpenAI
OPENAI_API_KEY=sk-...
```

### 6. Run the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

---

## Firestore Data Model

### `candidates/{uid}`
```json
{
  "candidateId": "uid",
  "skills": ["React", "TypeScript", "Node.js"],
  "experience": "2-sentence professional summary (PII-free)",
  "certifications": ["AWS Solutions Architect"],
  "jobFitScore": 87,
  "aiEvaluation": "Paragraph on technical strengths",
  "createdAt": "Timestamp"
}
```

### `recruiters/{uid}`
```json
{
  "recruiterId": "uid",
  "companyName": "Acme Corp",
  "isVerifiedInclusive": true,
  "createdAt": "Timestamp"
}
```

---

## The AI Prompt

The `/api/analyze-resume` route sends this system prompt to GPT-4o-mini:

```
You are a bias-free recruitment AI.
Analyze the following resume text.

1. Extract professional skills, years of experience, and certifications.
2. Provide a 'Job Fit Score' from 1-100 based on general tech competency.
3. Write a brief 'AI Evaluation' (2-3 sentences) of their technical strengths.
4. Write an 'experience' field: a 2-sentence professional summary.

STRICT RULE: You must remove ALL Personal Identifiable Information (PII).
- No names, no emails, no phone numbers
- No specific locations (city, country, address)
- No gender-specific pronouns (use "they/their")
- No company names that could identify the person
- No graduation years or specific dates
```

---

## User Flow

```
Landing Page
    ├── Sign Up as Candidate
    │       └── Paste resume → AI analysis → Anonymous profile saved → Firestore
    └── Sign Up as Recruiter
            └── Browse candidate grid → Filter by score → Shortlist candidates
```

---

## Design System

The UI uses a warm earthy palette with an animated gradient mesh background:

| Token | Value | Usage |
|---|---|---|
| `--terra` | `#be5f3a` | Primary accent, buttons |
| `--terra-deep` | `#9c4828` | Button gradient end |
| `--sand` | `#cba882` | Secondary text |
| `--sand-light` | `#e2cdb0` | Borders, inputs |
| `--clay` | `#9a7458` | Muted text |
| `--ink` | `#28160a` | Headings |
| `--ink-mid` | `#5e3d28` | Body text |
| `--ink-light` | `#8a6550` | Labels, captions |
| `--moss` | `#5f7a4a` | Success states, high scores |
| `--cream` | `#fdf7ee` | Card backgrounds |

**Fonts:** Playfair Display (display) · Lora (body) · DM Mono (labels/code)

---

## Deployment

### Deploy to Vercel (recommended)

```bash
npm install -g vercel
vercel
```

Add your environment variables in the Vercel dashboard under **Settings → Environment Variables**.

### Firestore Security Rules (production)

Before going live, update your Firestore rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /candidates/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == uid;
    }
    match /recruiters/{uid} {
      allow read, write: if request.auth.uid == uid;
    }
  }
}
```

---

## Contributing

Pull requests are welcome! For major changes, please open an issue first.

---

## License

MIT © SkillBlind.AI
