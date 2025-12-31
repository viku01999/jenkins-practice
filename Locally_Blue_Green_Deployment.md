# 🔹 Blue-Green Deployment Locally with Node.js + Jenkins

This guide shows how to simulate **Blue-Green Deployment** locally using your existing Jenkins + Node.js + TypeScript project. The goal is to have **two environments (Blue & Green)** running on different ports, allowing safe deployment and testing.

---

## 🔹 Prerequisites

- Node.js + TypeScript project with:
  - `package.json`
  - `npm run build`
  - `npm test`
  - `npm start` (server script)
- Jenkins installed and configured
- Git repository with your project
- Ports available for testing (e.g., `3000` for Blue, `3001` for Green)

---

## 1️⃣ Update your `package.json` scripts

Add environment-specific scripts for Blue and Green:

```json
"scripts": {
  "dev": "nodemon src/index.ts",
  "build": "tsc",
  "start": "node dist/index.js",
  "start:blue": "PORT=3000 node dist/index.js",
  "start:green": "PORT=3001 node dist/index.js",
  "test": "jest"
}
```

```groovy
pipeline {
    agent any

    tools {
        nodejs 'node-lts'
    }

    stages {

        stage('📦 Install Dependencies') {
            steps {
                dir("$WORKSPACE") {
                    sh 'npm install'
                }
            }
        }

        stage('🏗️ Build TypeScript') {
            steps {
                dir("$WORKSPACE") {
                    sh 'npm run build'
                }
            }
        }

        stage('🧪 Run Tests') {
            steps {
                dir("$WORKSPACE") {
                    sh 'npm test'
                }
            }
        }

        stage('🚀 Deploy Blue') {
            when {
                expression { env.BUILD_TARGET == "blue" }
            }
            steps {
                dir("$WORKSPACE") {
                    sh 'npm run start:blue &'
                }
            }
        }

        stage('🚀 Deploy Green') {
            when {
                expression { env.BUILD_TARGET == "green" }
            }
            steps {
                dir("$WORKSPACE") {
                    sh 'npm run start:green &'
                }
            }
        }

        stage('✅ Post Build') {
            steps {
                echo 'Build and deploy completed'
            }
        }
    }

    post {
        always {
            echo '📁 Build finished'
        }
    }
}
```
