# InternPath — Production-Ready Internship Portal

A responsive full-stack internship portal capstone built with HTML, CSS, JavaScript, Node.js, Express and Zod. It includes searchable internship listings, category/work-mode filters, a validated application flow, persistent application storage, security headers, rate limiting, structured logs, health checks and automated API tests.

## Features

- Responsive mobile-first layout and accessible form controls.
- Internship listings with keyword search, category and work-mode filters.
- Application form with browser-side and server-side validation.
- Applications saved to a local JSON data store.
- Express security headers via Helmet and rate-limited application submissions.
- Health endpoint, structured server logs, consistent API errors.
- Automated tests for health, listings, filters, validation, submissions and 404 handling.
- No API keys or paid services required for local demonstration.

## Requirements

- Node.js 20 or newer (Node.js 22 LTS recommended)
- npm
- VS Code (recommended)

## Run locally

1. Extract the ZIP file.
2. Open the extracted `production-ready-internship-portal` folder in VS Code.
3. Open **Terminal → New Terminal**.
4. Run:

   ```bash
   npm install
   npm test
   npm run check
   npm start
   ```

5. Open `http://localhost:3000` in your browser.
6. Test the health endpoint at `http://localhost:3000/health`.

For automatic restart during development, use `npm run dev`.

## API

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health` | Health/status check |
| GET | `/api/internships` | List internships |
| GET | `/api/internships?q=design` | Search internships |
| GET | `/api/internships?category=Engineering&type=Remote` | Filter internships |
| POST | `/api/applications` | Submit an application |

### Application request

```json
{
  "fullName": "Aarav Sharma",
  "email": "aarav@example.com",
  "phone": "+91 9876543210",
  "internshipId": "web-01",
  "education": "B.Tech Computer Science",
  "coverNote": "I enjoy building accessible web applications."
}
```

The API validates all fields, verifies the internship ID and returns an application reference. Application records are stored in `data/applications.json`, created automatically on the first successful submission.

## Project structure

```text
production-ready-internship-portal/
├── data/                 # Application data (created at runtime)
├── public/
│   ├── index.html        # Portal interface
│   ├── styles.css        # Responsive design
│   └── app.js            # Search, filters and application flow
├── src/
│   ├── app.js            # Express app, API, validation and middleware
│   └── server.js         # Server startup and graceful shutdown
├── tests/
│   └── api.test.js       # Automated API tests
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

## Quality and security checks

Run `npm test` for the automated regression suite and `npm run check` for JavaScript syntax checks. Helmet adds common HTTP security headers. The application endpoint has an IP-based rate limit. Request size is capped, user input is validated on the server, and user-provided content is escaped before rendering.

### Important production note

This capstone uses a local JSON file to keep setup beginner-friendly. On hosting platforms with ephemeral filesystems, local submissions may not persist across redeploys or restarts. For a real production launch, replace the JSON store with a managed database (for example PostgreSQL), add authenticated administration, configure backups, and set an appropriate privacy/retention policy. Do not collect real applicant data until those controls are in place.

## Deploy on Render (free web service where available)

1. Push this folder to a **public GitHub repository**.
2. Sign in to Render and choose **New → Web Service**.
3. Connect the repository.
4. Set **Runtime** to Node, **Build Command** to `npm install`, and **Start Command** to `npm start`.
5. Add environment variable `NODE_ENV=production`.
6. Deploy. Render supplies the `PORT` environment variable automatically.
7. Open the generated public URL and test the home page and `/health`.

For persistent application storage on Render, attach a persistent disk and set up a managed database before using this with real applicants.

## GitHub upload

From the project folder, run these commands after creating an empty public repository on GitHub:

```bash
git init
git add .
git commit -m "Build production-ready internship portal"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/production-ready-internship-portal.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. Do not upload `node_modules`, `.env`, or real applicant data.

## Demo checklist

- [ ] Home page opens on desktop and mobile.
- [ ] Search finds roles by title, skill or company.
- [ ] Category and work-mode filters work together.
- [ ] Application dialog opens for the selected role.
- [ ] Invalid form data is rejected.
- [ ] Valid demo application returns a reference.
- [ ] `/health` returns `status: healthy`.
- [ ] `npm test` passes.
- [ ] `npm run check` passes.
- [ ] Public GitHub repository and deployed app links are ready to submit.

## Capstone engineering decisions

- Express serves the frontend and API from one deployable Node service.
- Zod validates untrusted request data at the API boundary.
- Helmet and rate limiting provide baseline web protections.
- A health endpoint supports deployment health checks.
- Structured logs record server lifecycle and successful application receipt without logging applicant contact details.
- The JSON persistence layer is intentionally simple for a beginner capstone and is clearly marked for replacement before real production use.
