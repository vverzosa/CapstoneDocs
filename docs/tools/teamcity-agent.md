# TeamCity Documentation

### 1. Overview

**TeamCity Agent** – A lightweight process that runs builds assigned by the TeamCity server, executing build steps, running tests, and reporting results back to the server.

---

### 2. Setup

#### 2.1 Prerequisites
- Linux OS  
- Java (JDK 17+) installed  
- Docker installed for running builds in containers
- Python installed for the testing of the application 

#### 2.2 Installation Steps
1. **Install Java**
   ```
   sudo dnf install java-21-openjdk -y
   ```
   Set JAVA_HOME path if not yet set
2. **Install Docker**
   ```
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   ```
3. **Install** on agent:
   Download agent package from server
   ```bash
   wget http://<server-ip>:8111/update/buildAgent.zip
   unzip buildAgent.zip -d ~/buildAgent
   ```
4. **Configure** agent
   Edit the buildAgent/conf/buildAgent.properties file
   ```
   serverUrl=http://<server-ip>:8111/
   name=i9a-builder
   ```
5. **Start** agent
   ```
   cd ~/buildAgent/bin
   ./agent.sh start
   ```