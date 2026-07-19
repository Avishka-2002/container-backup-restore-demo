# 📦 Container Backup & Restore Demo (`container-backup-restore-demo`)

An automated **Disaster Recovery (DR)** and container lifecycle verification pipeline built with **Docker** and **GitHub Actions**. This project demonstrates building a container image, exporting it to a `.tar` backup file, saving it as an integrated CI/CD artifact, and verifying recovery by reloading (`docker load`) and running the container.

---

## 🎯 Project Blueprint (The 6-Step Guide)

### 1. Setup Repo
Contains a lightweight web container setup with an `index.html` landing page and a clean `Dockerfile`.

### 2. Write Dockerfile
Uses `nginx:alpine` to serve our custom HTML:
```dockerfile
FROM nginx:alpine
COPY ./index.html /usr/share/nginx/html/index.html
```
> **Sinhala Note:** මේකෙන් simple web container build වෙනවා.

### 3. GitHub Actions Workflow
Defined at `.github/workflows/container-backup.yml`, triggering on push to `main`. It automates the entire lifecycle using modern `v4` actions for maximum compatibility:
- **Build:** `docker build -t demo-app:latest .`
- **Backup:** `docker save demo-app:latest -o demo-app.tar`
- **Upload Artifact:** Saves `demo-app.tar` directly to the GitHub Actions Artifacts section.
- **Restore:** `docker load -i demo-app.tar`
- **Test:** `docker run -d -p 8080:80 demo-app:latest` + live HTTP health check (`curl http://localhost:8080`).

### 4. Backup Artifact
The pipeline exports the complete Docker image (`demo-app.tar`) and stores it as a downloadable CI/CD artifact (`docker-image-backup`).
> **Sinhala Note:** මේකෙන් container backup file එක GitHub Actions artifacts section එකේ save වෙනවා.

### 5. Restore & Test
To verify the integrity of our backup archive, the pipeline wipes the image from local cache, reloads it using `docker load -i demo-app.tar`, and spins up the container to confirm it boots up without issues.
> **Sinhala Note:** Restore test එකෙන් backup file එක valid කියලා confirm වෙනවා.

### 6. Educational Value
This workflow clearly illustrates the foundational container lifecycle:
`Build` ➡️ `Backup (Save to tar)` ➡️ `Upload Artifact` ➡️ `Restore (Load tar)` ➡️ `Run & Verify`
> **Sinhala-English Mix:** “Developersට disaster recovery readiness demonstrate කරන්න easy way එකක්.”

---

## 🚀 Quick Push Instructions

Open PowerShell inside this directory and run:

```powershell
cd "C:\Users\EXT\OneDrive\Desktop\Week 12\container-backup-restore-demo"

# Initialize Git and Push to GitHub
git init
git add .
git commit -m "feat: setup automated container backup and restore pipeline"
git branch -M main
git remote add origin https://github.com/Avishka-2002/container-backup-restore-demo.git
git push -u origin main
```
