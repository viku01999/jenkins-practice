# 🚀 Jenkins + Local + Node.js + TypeScript  

### 🏗️ Complete Step-by-Step Setup Guide (Beginner → Industry-Ready)

This document explains how to configure **Jenkins locally** to **build, test, and manage** a **Node.js + TypeScript** application using a **Jenkins Declarative Pipeline**.

## 🔄 Project structure

- ✅ This is folder structure for this
![Project Structure](/media/local_project.png)

---

## 📌 What You’ll Learn

- ✅ Install & configure **Node.js** in Jenkins  
- ✅ Create a **Jenkins Pipeline project**
- ✅ Run **npm install, build, and tests**
- ✅ Monitor builds & understand pipeline status
- ✅ Follow **best practices** used in real-world CI/CD

---

## 🧰 Prerequisites

Before you begin, make sure you have:

- 🟢 **Jenkins** installed and running locally  
- 🟢 A **Node.js + TypeScript project** containing:
  - `package.json`
  - `npm run build`
  - `npm test`
- 🟢 A Jenkins user with permission to:
  - 🔧 Manage plugins  
  - ⚙️ Configure tools  
  - 🧪 Create pipeline jobs  

---

## 1️⃣ Node.js Environment Setup in Jenkins

Follow these steps to install Node.js support in Jenkins:

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

📌 **Note:**  
The name `node-lts` will be referenced in the Jenkins pipeline.

---

## 2️⃣ Create a Jenkins Pipeline Project

1. ➕ Click **New Item**
2. ✏️ Enter a project name (example: `node-typescript-pipeline`)
3. 📌 Select **Pipeline**
4. ✅ Click **OK**
5. 📝 (Optional) Add a description
6. ⬇️ Scroll to **Pipeline**
7. Set **Definition** → `Pipeline script`
8. 📋 Paste the pipeline code (see below)
9. 💾 Click **Save**

---

## 3️⃣ Jenkins Pipeline Script

> ⚠️ **Important:**  
> Replace the project path with `$WORKSPACE` for best practice.

```groovy
pipeline {
    agent any

    tools {
        nodejs 'node-lts'
    }

    environment {
        NODE_ENV = 'development'
    }

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

## 🚀 4. Running the Jenkins Build

Follow these steps to execute and monitor your Jenkins pipeline:

1. 🏠 Open your Jenkins project from the **Jenkins Dashboard**.

2. On the project page, you will find several options:

   - **▶️ Build Now**  
     Manually triggers a new pipeline build immediately.

   - **⚙️ Configure**  
     Opens the configuration page to edit pipeline scripts, environment variables, triggers, and other settings.

   - **📊 Pipeline Overview**  
     Shows a visual representation of all pipeline stages and their execution status.

   - **🕘 Build History**  
     Lists previous builds with numbers, timestamps, and statuses (success, failure, running).

   - **📈 Status**  
     Displays the current state of the last build.

   - **📝 Changes**  
     Shows source code changes since the last successful build (requires SCM integration).

   - **📂 Workspace**  
     Accesses files and directories used by Jenkins during the build.

3. Click **▶️ Build Now** to start a new build.

4. A new entry will appear in **🕘 Build History**.

5. Click the build number to open detailed view, where you can access:

   - **📄 Console Output** – View detailed logs and execution output for debugging.
   - **📊 Pipeline Overview** – Track stage progress and results.
   - **📝 Changes** – Review committed code included in the build.
   - **⏱️ Timing** – See the duration of each stage.

6. To monitor logs in real-time, click **📄 Console Output**.

7. To stop a running build, click **❌ Terminate**.

---

## 📊 5. Build Status & Monitoring

Jenkins provides visual indicators for build status:

- ✅ **Green build** – Pipeline completed successfully.
- ❌ **Red build** – Pipeline failed. Check **Console Output** for errors.
- ⏳ **Blue / Running build** – Pipeline is currently executing.

---

## 🧠 Notes & Best Practices

- 🚫 Avoid hardcoding absolute paths; use **Jenkins environment variables** instead.
- ✅ Best practices for pipelines:
  - **🔗 Git SCM Integration** – Automated builds from source control.
  - **📂 Jenkins Workspace (`$WORKSPACE`)** – Use for all file paths.
  - **📦 Artifact Archiving** – Store build outputs for production pipelines.
- 🔒 Maintain consistent Node.js versions using **.nvmrc** or Node version managers.
- 📌 Consider adding automated triggers for **CI/CD workflows**.

---

### 🎯 Summary

By following these steps, you can:

- ✅ Start, monitor, and debug Jenkins builds  
- ✅ Understand pipeline stage results  
- ✅ Implement industry-standard best practices for CI/CD  

---
