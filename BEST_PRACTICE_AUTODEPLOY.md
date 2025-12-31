# GitHub Webhook (Best Practice for CI/CD)

This approach represents **real-world CI/CD** where Jenkins builds are triggered **instantly** when code is pushed to GitHub.

> ⚠️ **Important Note:**  
> GitHub webhooks require Jenkins to be accessible from the **public internet**.  
> If Jenkins is running on `localhost`, use **ngrok** or a cloud-hosted Jenkins instance.

---

## 🔗 Step 3: Configure Jenkins for GitHub Webhooks

### 3.1 Install Required Jenkins Plugin

1. Open **Jenkins Dashboard**
2. Navigate to:
   - **Manage Jenkins → Plugins**
3. Open the **Installed** tab
4. Ensure the following plugin is installed:
   - ✅ **GitHub Integration Plugin**

> If not installed, install it from the **Available** tab and restart Jenkins.

---

### 3.2 Configure the Jenkins Pipeline Job

1. Open your **Jenkins Pipeline job**
2. Click **Configure**
3. Scroll to **Build Triggers**
4. Enable:
   - ☑️ **GitHub hook trigger for GITScm polling**
5. Click **Save**

This allows Jenkins to listen for webhook events from GitHub.

---

### 3.3 Add Webhook in GitHub Repository

1. Open your **GitHub repository**
2. Go to:
   - **Settings → Webhooks**
3. Click **Add webhook**
4. Fill in the webhook details:

   - **Payload URL**

```http://<jenkins-host>:8080/github-webhook/```

- **Content type**

```application/json```

- **Events**
- Select **Just the push event**

5.Click **Add webhook**

✅ GitHub will now send a request to Jenkins on every push.

---

## 🔍 Step 4: Verify Webhook Delivery (Very Important)

1. In GitHub, go to:
   - **Settings → Webhooks**
2. Click on the newly created webhook
3. Scroll to **Recent Deliveries**
4. Check the delivery status:

   - ✅ **Green check** → Jenkins successfully received the webhook
   - ❌ **Red cross** → Jenkins could not be reached

---

### ❌ If Webhook Delivery Fails (Red Status)

Common reasons:

- Jenkins is running on **localhost**
- Incorrect **Payload URL**
- Firewall or network restrictions
- Jenkins is not publicly accessible

### ✔️ Solutions

- Use **ngrok** to expose local Jenkins
- Host Jenkins on a cloud VM (AWS, GCP, Azure)
- Use **Poll SCM** for local development setups

---

## 🎯 Summary

- GitHub Webhooks trigger Jenkins **instantly**
- Jenkins must be **publicly reachable**
- Correct webhook endpoint:
