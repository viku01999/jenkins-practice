# Jenkins Installation on Ubuntu 25 with Oracle JDK 21

Before installing Jenkins, make sure your system meets the following requirements:

---

## 1. Operating System

- Ubuntu 25 (or any modern Debian/Ubuntu LTS version)
- A 64-bit OS is recommended

---

## 2. Java

- **Oracle JDK 21** (or OpenJDK 21, officially supported)
- Java must be installed and accessible
- Environment variables set:

```bash
export JAVA_HOME=/opt/jdk-21.0.8
export PATH=$JAVA_HOME/bin:$PATH
```

- Verify **Java** installation

```bash
java -version
which java
```

## 3. Jenkins

- **Installation Command**

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
```

- **Download Jenkins WAR** (WAR file and setup)

```bash
sudo mkdir -p /usr/share/java
sudo wget https://get.jenkins.io/war-stable/2.528.3/jenkins.war -O /usr/share/java/jenkins.war
```

- Run Jenkins manually

```bash
sudo -u jenkins /opt/jdk-21.0.8/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --httpPort=8998
```

- Optional: Run in background

```bash
sudo -u jenkins nohup /opt/jdk-21.0.8/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --httpPort=8998 > /var/log/jenkins.log 2>&1 &
```

```bash
# Generally this is not required (Optional)
sudo chown jenkins:jenkins /usr/share/java/jenkins.war
sudo chmod 755 /usr/share/java/jenkins.war
```

- Create Jenkins user

```bash
sudo adduser jenkins
```

- Access Jenkins
  - Open browser: ``` http://localhost:8998 ```

```bash
sudo cat /var/lib/jenkins/.jenkins/secrets/initialAdminPassword
```

## Delete and Clean Up Jenkins on Ubuntu 25 (Keep Java)

This guide explains how to **stop Jenkins, remove its service, configuration, and WAR file**, without removing your Java installation.

---

## 1. Stop Jenkins Service

If you installed Jenkins via `apt` or created a custom systemd service:

```bash
# Stop Jenkins service
sudo systemctl stop jenkins

# Disable it from starting on boot
sudo systemctl disable jenkins

# Verify status
systemctl status jenkins

# Remove Jenkins package completely
sudo apt-get purge jenkins -y

# Remove unused dependencies
sudo apt-get autoremove -y

# Update package list
sudo apt update

# Remove Jenkins WAR
sudo rm -f /usr/share/java/jenkins.war

# Remove Jenkins home directory
sudo rm -rf /var/lib/jenkins

# Remove Jenkins cache
sudo rm -rf /var/cache/jenkins/war

# Remove Jenkins logs
sudo rm -rf /var/log/jenkins

# Remove override configuration
sudo rm -rf /etc/systemd/system/jenkins.service.d

# Remove systemd service file
sudo rm -f /etc/systemd/system/jenkins.service

# Reload systemd
sudo systemctl daemon-reload

# Remove Jenkins User (Optional)
sudo deluser --remove-home jenkins

# Check if any Jenkins files remain
ls -l /usr/share/java
ls -l /var/lib/
ls -l /var/log/
systemctl status jenkins
```

## Flow chart

```mermaid
flowchart TD
    A[Start: Ubuntu 25 System] --> B[Install Oracle JDK 21]
    B --> C[Set JAVA_HOME=/opt/jdk-21.0.8 and add to PATH]
    C --> D[Download Jenkins WAR file]
    D --> E[Create Jenkins user: sudo adduser jenkins]
    E --> F[Set permissions for WAR]
    F --> G[Run Jenkins manually using WAR]
    G --> H{Optional: Keep Jenkins running after logout?}
    H -->|Yes| I[Run with nohup or screen: sudo -u jenkins nohup java -jar jenkins.war &]
    H -->|No| J[Jenkins runs in foreground terminal]
    I --> K[Access Jenkins at http://localhost:8998]
    J --> K
    K --> L[Retrieve initial admin password: /var/lib/jenkins/secrets/initialAdminPassword]
    L --> M[Complete setup in browser]
