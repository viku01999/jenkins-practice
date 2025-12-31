# 🌐 Jenkins + GitHub + Node.js + TypeScript  

## 🏗️ Pushing Local Project to GitHub & Running Jenkins Pipeline

This guide explains how to **push your local Node.js + TypeScript project to GitHub** and **trigger your Jenkins pipeline automatically**.

---

## 🔄 Project Structure

- ✅ This is folder structure for the project
![Project Structure](/media/github.png)

---

## 📌 What You’ll Learn

- ✅ Install & configure **Node.js** in Jenkins  
- ✅ Create a **Jenkins Pipeline project**
- ✅ Run **npm install, build, and tests**
- ✅ Push local project to **GitHub**
- ✅ Trigger Jenkins pipeline automatically via **GitHub webhook**
- ✅ Monitor builds & understand pipeline status
- ✅ Follow **industry best practices** for CI/CD

---

## 🧰 Prerequisites

Before you begin, make sure you have:

- 🟢 **Jenkins** installed and running locally  
- 🟢 A **Node.js + TypeScript project** containing:
  - `package.json`
  - `npm run build`
  - `npm test`
- 🟢 **Git** installed locally  
- 🟢 A **GitHub account**  
- 🟢 A Jenkins user with permissions to:
  - 🔧 Manage plugins  
  - ⚙️ Configure tools  
  - 🧪 Create pipeline jobs  

---

## 1️⃣ Node.js Environment Setup in Jenkins

1. 🏠 Open **Jenkins Dashboard**  
2. ⚙️ Click **Manage Jenkins**  
3. 🔌 Select **Plugins**  
4. 🔍 Go to **Available Plugins**  
5. Search for **NodeJS**  
6. 📥 Install **NodeJS Plugin**  
7. 🔄 Return to **Manage Jenkins**  
8. 🛠️ Click **Tools**  
9. ⬇️ Scroll to **NodeJS Installations**  
10. ➕ Click **Add NodeJS**  
11. Configure:
    - **Name**: `node-lts`
    - ✅ Enable **Install automatically**
    - 📦 Choose **Node.js LTS version**
12. 💾 Click **Save**

---

## 2️⃣ Initialize Git in Local Project

1. Open terminal in your project root:

```bash
cd /path/to/your/project

# Initialize Git (if not already done):
git init

# Add all files to Git:
git add .

# Commit changes:
git commit -m "Initial commit"

```

## 3️⃣ Create a GitHub Repository

1. Go to **GitHub → Repositories → New**  
2. Enter repository name (e.g., `node-typescript-pipeline`)  
3. Optionally add a description  
4. Choose **Public** or **Private**  
5. Do **not** initialize with a README  
6. Click **Create repository**  

---

## 4️⃣ Push Local Project to GitHub

1. Copy the repository URL (HTTPS or SSH)  

2. Add remote origin:

```bash
git remote add origin https://github.com/username/node-typescript-pipeline.git

# Push local commits to GitHub:
git branch -M main

git push -u origin main

```

📌 Note: Replace username and repository URL with your own

## 5️⃣ Create a Jenkins Pipeline Project

1. ➕ Click **New Item**  
2. ✏️ Enter project name (example: `node-github`)  
3. 📌 Select **Pipeline**  
4. ✅ Click **OK**
5. 📝 Add optional description  
6. ⬇️ Scroll to **Pipeline**  
7. Set **Definition → Pipeline script from SCM**  
8. Configure:
   - **SCM:** Git  
   - **Repository URL:** `https://github.com/username/node-typescript-pipeline.git`  
   - **Branch:** `main`  
   - **Credentials:** Add if private  
9. 💾 Click **Save**  

---

## 6️⃣ Jenkins Pipeline Script

> ⚠️ **Important:** Ensure `Jenkinsfile` is at the root of your repository.

```groovy
pipeline {
    agent any

    tools {
        nodejs 'node-lts'
    }

    // environment {
    //     NODE_ENV = 'development'
    // }

    stages {

        stage('📦 Install Dependencies') {
            steps {
                echo 'Installing npm dependencies'
                dir("$WORKSPACE") {
                    sh 'npm install'
                }
            }
        }

        stage('🏗️ Build TypeScript') {
            steps {
                echo 'Building application'
                dir("$WORKSPACE") {
                    sh 'npm run build'
                }
            }
        }

        stage('🧪 Run Tests') {
            steps {
                echo 'Running tests'
                dir("$WORKSPACE") {
                    sh 'npm test'
                }
            }
        }

        stage('🚀 Deploy') {
            steps {
                echo 'Deploying application...'
                dir("$WORKSPACE") {
                    sh 'npm run dev'
                }
            }
        }

        stage('✅ Post Build') {
            steps {
                echo 'Build completed successfully'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline finished successfully'
        }
        failure {
            echo '❌ Pipeline failed – check console output'
        }
        always {
            echo '📁 Build finished – artifacts can be archived'
        }
    }
}
```

## 7️⃣ Trigger Jenkins Pipeline Automatically

## Option A: Poll SCM (Simple) or build manually

1. In **Pipeline → Build Triggers**, check **Poll SCM**  
2. Add schedule, e.g.: `H/5 * * * *`

> Jenkins will check GitHub every 5 minutes for changes.

## Option B: GitHub Webhook (Recommended)

1. In **GitHub → Repository → Settings → Webhooks**  
2. Click **Add webhook**  
3. Payload URL: `http://<jenkins-url>/github-webhook/`  
4. Content type: `application/json`  
5. Events: **Push events**  
6. Click **Add webhook**  

> Jenkins will automatically trigger the pipeline on every push.

---

## 8️⃣ Running & Monitoring Jenkins Build

1. 🏠 Open your Jenkins project from the **Jenkins Dashboard**  
2. Click **▶️ Build Now** to start a new build  
3. Monitor stages via **Pipeline Overview** and **Console Output**  
4. Use **Build History** to track past builds  
5. To stop a build, click **❌ Terminate**

### Build Status Indicators

- ✅ Green – Successful  
- ❌ Red – Failed  
- ⏳ Blue – Running  

---

## 🧠 Notes & Best Practices

- 🚫 Avoid hardcoding paths; use `$WORKSPACE`  
- 🔗 Integrate Git SCM for automated builds  
- 📦 Archive artifacts for production pipelines  
- 🔒 Use `.nvmrc` or Node version managers for consistent Node.js versions  
- Monitor builds with email, Slack, or Teams notifications  

---

## 🎯 Summary

By following this guide, you can:

- ✅ Set up a local Jenkins pipeline  
- ✅ Push your project to GitHub  
- ✅ Automatically trigger builds on GitHub pushes  
- ✅ Monitor and debug pipeline stages efficiently  
- ✅ Apply industry-standard CI/CD best practices

## 🔹 Check Poll SCM logs in Jenkins

1. **Open your Jenkins job**  
    - In left side git polling log (Click)

2. **Click on “git polling log”**  
   - In the left-hand menu of the job page, you should see **“git polling log”**.  
   - If you don’t see it, make sure **Poll SCM** is enabled in **Build Triggers**.

3. **Read the log**  
   Each poll will show something like:

## 📝 Note: How Jenkins Runs Builds with Poll SCM

## 🔹 Poll detects changes

- If **Poll SCM** finds new commits in Git, Jenkins will **trigger a new build**.
- If there are **no new commits**, Jenkins will **not trigger a build**.

## 🔹 What happens in a triggered build

Your pipeline runs **all stages** defined in `Jenkinsfile`:

- `npm install` → installs dependencies  
- `npm run build` → compiles TypeScript  
- `npm test` → runs tests  
- `npm start` → starts your Node.js server  

> Even if code changes are small, **all stages execute again**, including `npm start`.

---

### 🔹 Important point about `npm start` (Deploy stage)

- `npm start` runs your **Node.js server**.  
- **Problem:** If your previous build already started a server on the same port (e.g., 3000), the new build will **fail at this step** because the port is already in use.  
- Jenkins does **not automatically kill previous servers**; each build runs in the same workspace unless you isolate them with **Docker, containers, or different ports**.

```groovy
stage('🚀 Deploy') {
    steps {
        echo 'Deploying application...'
        dir("$WORKSPACE") {
            sh 'npm start'
        }
    }
}
```
