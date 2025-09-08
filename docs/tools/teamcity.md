# TeamCity Documentation

### 1. Overview

**TeamCity** is a Continuous Integration (CI) server that automates building, testing, and deploying software. In our setup, TeamCity interacts with GitHub repositories to build PR branches, merges, and main branches based on triggers.

Key points from the environment:  
- Using TeamCity with GitHub (non-Enterprise)  
- VCS triggers: dev and main branch pull requests
- PR branch builds have special handling; non-built branches may appear in UI if triggers are misconfigured  

---

### 2. Setup

#### 2.1 Prerequisites
- Linux for TeamCity server and agent  
- Java (JDK 17+) installed  
- Access to GitHub repositories  
- Docker installed to agent if running builds in containers
- Python installed to agent for the testing of the application 
- Database server if using a dedicated db for teamcity

#### 2.2 Installation Steps
1. **Install Java**
   ```
   sudo dnf install java-21-openjdk -y
   ```
   Set JAVA_HOME path if not yet set
2. **Download TeamCity** from [JetBrains website](https://www.jetbrains.com/teamcity/download/).  
3. **Install** on server:
   ```bash
   tar -xzf TeamCity-<version>.tar.gz
   cd TeamCity
   ./bin/runAll.sh start
   ```
4. **Create a systemd service** (optional)
   create a service(.service) file at /etc/systemd/system
   ```
   [Unit]
   Description=TeamCity Server
   After=network.target

   [Service]
   Type=forking
   User=teamcity
   Group=teamcity
   Environment=TEAMCITY_DATA_PATH=/var/lib/teamcity
   WorkingDirectory=/opt/TeamCity/
   ExecStart=/opt/TeamCity/bin/teamcity-server.sh start
   ExecStop=/opt/TeamCity/bin/teamcity-server.sh stop
   Restart=on-failure
   TimeoutSec=300

   [Install]
   WantedBy=multi-user.target
   ```
   Reload the daemon and enable the service
   ```
   sudo systemctl daemon-reload
   sudo systemctl enable --now teamcity-server.service
   ```
5. Access Web UI via http://server-ip:8111
6. Configure Database (MariaDB)
   ```
   sudo dnf install -y mariadb-server
   sudo systemctl enable --now mariadb
   ```
   Secure the installation:
   ```
   sudo mysql_secure_installation
   ```
   Create TeamCity Database & User
   ```
   sudo mysql -u root -p

   CREATE DATABASE teamcity CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

   CREATE USER 'teamcity'@'%' IDENTIFIED BY 'Password';

   GRANT ALL PRIVILEGES ON teamcity.* TO 'teamcity'@'%';

   FLUSH PRIVILEGES;
   EXIT;
   ```
   Restart TeamCity
   ```
   sudo systemctl restart teamcity-server
   ```
   Refer to this [page](teamcity-agent.md) for the teamcity agent setup
