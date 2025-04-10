# SonarQube Installation on Ubuntu (EC2)

This guide provides step-by-step instructions to install and configure **SonarQube** on an Ubuntu-based EC2 instance.

---

## 📋 Prerequisites

- **OS**: Ubuntu (e.g., 20.04 LTS)
- **Instance Type**: t3.medium (recommended)
- **Storage**: 30GB
- **Java**: OpenJDK 17
- **Permissions**: Sudo privileges required

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


