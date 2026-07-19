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

---

## 💻 How to Run This Project Locally on Your PC (Windows PowerShell)

If you have **Docker Desktop** installed on your Windows PC, you can run the exact same lifecycle and test disaster recovery right on your machine:

### Step 1: Open PowerShell and navigate to the project
```powershell
cd "C:\Users\EXT\OneDrive\Desktop\Week 12\container-backup-restore-demo"
```

### Step 2: Build the Docker image
```powershell
docker build -t demo-app:latest .
```
> **Sinhala Note:** අපගේ `index.html` එක සහ Nginx භාවිතා කර `demo-app:latest` නම් container image එක build කරයි.

### Step 3: Backup — Export the image to a `.tar` archive
```powershell
docker save demo-app:latest -o demo-app.tar
```
> **Sinhala Note:** `demo-app.tar` ලෙස backup file එකක් project folder එකේ නිර්මාණය වේ.

### Step 4: Simulate Disaster — Remove the image from your local Docker cache
```powershell
docker rmi -f demo-app:latest
```
> **Sinhala Note:** Backup එකෙන් restore වෙන බව confirm කිරීමට, දැනට build කරපු local image එක delete කරමු.

### Step 5: Restore — Load the image back from the `.tar` archive
```powershell
docker load -i demo-app.tar
```
> **Sinhala Note:** `docker load` මඟින් අපගේ `demo-app.tar` backup file එක නැවත Docker වෙත restore කරයි.

### Step 6: Run & Verify the restored container
```powershell
# Start the container in background on port 8080
docker run -d -p 8080:80 --name local-demo-app demo-app:latest

# Open in your web browser or test via curl
curl http://localhost:8080
```
Then open your browser and go to **`http://localhost:8080`** — you will see the `"✅ Container Successfully Restored!"` web page!

### Step 7: Clean up local container (when finished testing)
```powershell
docker rm -f local-demo-app
```

