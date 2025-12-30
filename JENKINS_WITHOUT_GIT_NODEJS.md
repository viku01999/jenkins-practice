# 🚀 Jenkins + Local + Node.js + TypeScript  

### 🏗️ Complete Step-by-Step Setup Guide (Beginner → Industry-Ready)

This document explains how to configure **Jenkins locally** to **build, test, and manage** a **Node.js + TypeScript** application using a **Jenkins Declarative Pipeline**.

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

## 4. Running the Jenkins Build

1. Open your Jenkins project from the Jenkins dashboard.

2. On the project page, you will see several available options, including:

   - **Build Now**  
     Manually triggers a new build of the pipeline immediately.

   - **Configure**  
     Opens the project configuration page where you can edit the pipeline script, environment variables, triggers, and other settings.

   - **Pipeline Overview**  
     Displays a visual representation of the pipeline stages and their execution status.

   - **Build History**  
     Shows a list of all previous builds with their build numbers, timestamps, and status (success, failure, or running).

   - **Status**  
     Displays the current state of the last build.

   - **Changes**  
     Shows source code changes since the last successful build (available when SCM is configured).

   - **Workspace**  
     Displays the files and directories used by Jenkins during the build.

3. Click **Build Now** to start a new pipeline execution.

4. A new build entry will appear in the **Build History** section.

5. Click the build number to open the build details page, where you can access:

   - **Console Output**  
     View detailed logs and command execution output for debugging.

   - **Pipeline Overview**  
     Track the progress and result of each pipeline stage.

   - **Changes**  
     Review the list of code changes included in that build.

   - **Timing**  
     See how long each stage took to execute.

6. To monitor logs in real time, click **Console Output**.

7. To stop a running build, click the **❌ (Terminate)** button for that build.

---

## 5. Build Status & Monitoring

Jenkins uses color indicators to show build status:

- ✅ **Green build**  
  The pipeline completed successfully.

- ❌ **Red build**  
  The pipeline failed. Check the **Console Output** for errors.

- ⏳ **Blue / Running build**  
  The pipeline is currently in progress.

---

## Notes & Best Practices

- Avoid hardcoding absolute paths; prefer Jenkins environment variables.
- Use the following best practices:
  - **Git SCM integration** for source control and automated builds
  - **Jenkins workspace** (`$WORKSPACE`) instead of manual directory paths
  - **Artifact archiving** for storing build outputs in production pipelines
- Use **.nvmrc** or Node.js version locking to maintain consistent Node.js versions across environments.
