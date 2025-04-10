# SonarQube Installation on Ubuntu (EC2)

This guide provides step-by-step instructions to install and configure **SonarQube** on an Ubuntu-based EC2 instance manually.

---

## 📋 Prerequisites

- **OS**: Ubuntu (e.g., 20.04 LTS)
- **Instance Type**: t3.medium (recommended)
- **Storage**: 30GB
- **Java**: OpenJDK 17
- **Sudo access**: Required for installation and configuration
- **Security Group**: Allow inbound traffic on port `9000` (for accessing SonarQube UI)

---

## ☕ Install Java

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java --version

```
---

# 🐘 Install PostgreSQL

```sh
sudo apt install postgresql postgresql-contrib -y
```
### Set up PostgreSQL for SonarQube

```sh

sudo -i -u postgres
psql
CREATE DATABASE sonarqube;
CREATE USER sonar WITH ENCRYPTED PASSWORD 'P@ssw0rd';
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
\c sonarqube
GRANT ALL PRIVILEGES ON SCHEMA public TO sonar;
\q
exit

```
---
### 📦 Install SonarQube

**1. Download & Extract**
```sh
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.7.0.96327.zip
sudo apt install unzip -y
unzip sonarqube-10.7.0.96327.zip
sudo mv sonarqube-10.7.0.96327 /opt/sonarqube
```
**2. Create a SonarQube User**
```sh
sudo useradd -m -d /opt/sonarqube -r -s /bin/bash sonar
sudo chown -R sonar:sonar /opt/sonarqube
```
---

**⚙️ Configure SonarQube**
```sh
sudo vim /opt/sonarqube/conf/sonar.properties
```

**Update the following lines:**
```sh
sonar.jdbc.username=sonar
sonar.jdbc.password=P@ssw0rd
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.web.host=0.0.0.0
sonar.web.port=9000
```

**Add Sonar User to Sudoers and Set Password**
```sh
sudo usermod -aG sudo sonar
sudo passwd sonar

```
---

**▶️ Run SonarQube**
```sh
su - sonar
ulimit -n 65536
cd /opt/sonarqube/bin/linux-x86-64/
./sonar.sh start
./sonar.sh status
```
---

### 🔎 Install Sonar Scanner

```sh
sudo wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-6.2.1.4610-linux-x64.zip
sudo unzip sonar-scanner-cli-6.2.1.4610-linux-x64.zip
sudo mv sonar-scanner-6.2.1.4610-linux-x64 /opt/sonar-scanner
sudo chown -R sonar:sonar /opt/sonar-scanner
```

**Create .scannerwork Directory**
```sh
sudo mkdir -p /opt/sonar-scanner/.scannerwork
sudo chown -R sonar:sonar /opt/sonar-scanner/.scannerwork
sudo chmod 755 /opt/sonar-scanner/.scannerwork
```

**Add Scanner to PATH**
```sh
echo 'export PATH=$PATH:/opt/sonar-scanner/bin' >> ~/.bashrc
source ~/.bashrc
sonar-scanner --version
```
---

### 🌐 Access SonarQube Web UI
```sh
http://<your-server-ip>:9000
```
- Username: admin

- Password: admin

⚠️ Change the default password after login for security.

---

**SonarQube is now up and running on your server. You can begin analyzing your source code using the Sonar Scanner.**