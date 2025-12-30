# Jenkins + Local + Node.js + TypeScript  

**Complete Step-by-Step Setup Guide (Beginner to Industry-Ready)**

This guide explains how to configure **Jenkins locally** to build, test, and manage a **Node.js + TypeScript** application using a Jenkins Pipeline.

---

## Prerequisites

Before starting, ensure you have the following:

- Jenkins installed and running on your local machine
- A Node.js + TypeScript project containing:
  - `package.json`
  - A build script (`npm run build`)
  - A test script (`npm test`)
- A Jenkins user account with permission to:
  - Manage plugins
  - Configure tools
  - Create pipeline projects

---

## 1. Node.js Environment Setup in Jenkins

Follow these steps to install and configure Node.js in Jenkins:

1. Open the **Jenkins Dashboard**
2. Click the **gear icon** → **Manage Jenkins**
3. Select **Plugins**
4. Navigate to **Available Plugins**
5. Search for **NodeJS**
   - If it is already installed, verify it under **Installed Plugins**
6. Install the **NodeJS Plugin**
7. Go back to **Manage Jenkins**
8. Click **Tools**
9. Scroll down to **NodeJS Installations**
10. Click **Add NodeJS**
11. Configure the Node.js installation:
    - **Name**: `node-lts`  
      > This name will be referenced in the Jenkins pipeline to select the Node.js environment
    - Enable **Install automatically**
    - Select the required **Node.js version** (LTS is recommended)
12. Leave all other options as default
13. Click **Save**
14. Return to the **Jenkins Home Page**

---

## 2. Jenkins Pipeline Project Setup

Create a new Jenkins Pipeline project:

1. From the Jenkins Dashboard, click **New Item**
2. Enter a project name of your choice
3. Select **Pipeline**
4. Click **OK**
5. You will be redirected to the project configuration page
6. (Optional) Add a project description
7. Scroll down to the **Pipeline** section
8. Set **Definition** to:
   - `Pipeline script`
9. Paste the pipeline script provided below
10. Click **Save**

After saving, Jenkins will redirect you to the newly created project page.

---

## 3. Jenkins Pipeline Script

> ⚠️ **Important:** Update the project directory path to match your local system or replace it with `$WORKSPACE`.

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

        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies'
                dir('/home/vikas/Documents/jenkins-practice') {
                    sh 'npm install'
                }
            }
        }

        stage('Build TypeScript') {
            steps {
                echo 'Compiling TypeScript'
                dir('/home/vikas/Documents/jenkins-practice') {
                    sh 'npm run build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests'
                dir('/home/vikas/Documents/jenkins-practice') {
                    sh 'npm test'
                }
            }
        }

        stage('Post-Build') {
            steps {
                echo 'Build completed successfully'
            }
        }

        // Optional: Start the application
        // stage('Start Application') {
        //     steps {
        //         echo 'Starting application'
        //         dir('/home/vikas/Documents/jenkins-practice') {
        //             sh 'npm run dev'
        //         }
        //     }
        // }
    }

    post {
        success {
            echo 'Pipeline finished successfully'
        }
        failure {
            echo 'Pipeline failed. Check the console output for details.'
        }
        always {
            echo 'Build finished. Artifacts (dist/) can be archived if required.'
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
