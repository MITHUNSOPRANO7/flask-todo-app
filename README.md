# Flask Todo App with CI/CD Pipeline

A production-ready Todo application built with Flask and MySQL, featuring automated deployment using Docker, Jenkins, and AWS EC2.

## 🔗 Live Demo

**Application URL:** http://16.171.142.62:5000

## 🛠️ Tech Stack

### Frontend
- HTML5, CSS3
- Responsive Design

### Backend
- Python 3.10
- Flask (Web Framework)
- MySQL 8.0 (Database)
- PyMySQL (Database Connector)

### DevOps & Infrastructure
- **Containerization:** Docker, Docker Compose
- **CI/CD:** Jenkins
- **Version Control:** Git, GitHub
- **Cloud Platform:** AWS EC2 (t3.micro)
- **Operating System:** Ubuntu 24.04 LTS

## 📋 Features

- ✅ Create, Read, Update, Delete (CRUD) todos
- ✅ Persistent MySQL database
- ✅ Dockerized multi-container architecture
- ✅ Automated CI/CD pipeline
- ✅ Cloud deployment on AWS EC2
- ✅ Responsive web interface

## 🏗️ Architecture
```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   GitHub    │────────▶│   Jenkins    │────────▶│   AWS EC2   │
│ Repository  │  Push   │   Pipeline   │  Deploy │             │
└─────────────┘         └──────────────┘         └─────────────┘
                                                         │
                                                         ▼
                                                  ┌─────────────┐
                                                  │   Docker    │
                                                  │  Containers │
                                                  └─────────────┘
                                                         │
                                        ┌────────────────┴────────────────┐
                                        ▼                                 ▼
                                 ┌─────────────┐                  ┌─────────────┐
                                 │ Flask App   │◀────────────────▶│   MySQL DB  │
                                 │ Container   │   Port 3306      │  Container  │
                                 │  Port 5000  │                  │             │
                                 └─────────────┘                  └─────────────┘
```

## 🚀 CI/CD Workflow

1. **Developer** makes code changes locally
2. **Git Push** commits to GitHub repository
3. **Jenkins** pulls latest code from GitHub
4. **Jenkins** copies files to EC2 via SSH/SCP
5. **EC2** rebuilds Docker images
6. **Docker Compose** restarts containers
7. **Application** automatically updates and goes live

## 📁 Project Structure
```
flask-todo-app/
├── app.py                 # Flask application
├── Dockerfile            # Flask container definition
├── docker-compose.yml    # Multi-container orchestration
├── requirements.txt      # Python dependencies
├── init.sql             # MySQL database schema
├── Jenkinsfile          # CI/CD pipeline configuration
├── templates/
│   └── index.html       # Frontend HTML/CSS
├── .gitignore
└── README.md
```

## 🔧 Local Development Setup

### Prerequisites
- Docker & Docker Compose installed
- Git installed
- Python 3.10+ (optional, for local testing)

### Steps

1. **Clone the repository:**
```bash
   git clone https://github.com/MITHUNSOPRANO7/flask-todo-app.git
   cd flask-todo-app
```

2. **Start the application:**
```bash
   docker-compose up -d
```

3. **Access the app:**
   - Open browser: `http://localhost:5000`

4. **Stop the application:**
```bash
   docker-compose down
```

## ☁️ AWS EC2 Deployment

### EC2 Instance Configuration
- **Instance Type:** t3.micro (Free Tier)
- **OS:** Ubuntu 24.04 LTS
- **Region:** ap-south-1 (Mumbai)
- **Security Groups:**
  - Port 22 (SSH)
  - Port 5000 (Flask App)
  - Port 8080 (Jenkins - Optional)

### Deployment Steps

1. **SSH into EC2:**
```bash
   ssh -i your-key.pem ubuntu@YOUR_EC2_IP
```

2. **Install Docker & Docker Compose:**
```bash
   sudo apt update
   sudo apt install docker.io -y
   sudo systemctl start docker
   sudo usermod -aG docker ubuntu
   
   sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
```

3. **Clone repository:**
```bash
   git clone https://github.com/MITHUNSOPRANO7/flask-todo-app.git
   cd flask-todo-app
```

4. **Deploy application:**
```bash
   docker-compose up -d --build
```

5. **Access the app:**
   - Open browser: `http://YOUR_EC2_IP:5000`

## 🔄 Jenkins CI/CD Setup

### Prerequisites
- Jenkins running in Docker
- GitHub repository access
- EC2 SSH key configured

### Jenkins Pipeline Configuration

1. **Install required plugins:**
   - SSH Agent
   - Git
   - Pipeline

2. **Add credentials:**
   - GitHub PAT (Personal Access Token)
   - EC2 SSH private key

3. **Create pipeline:**
   - New Item → Pipeline
   - Pipeline script from SCM
   - Repository: `https://github.com/MITHUNSOPRANO7/flask-todo-app.git`
   - Script Path: `Jenkinsfile`

4. **Trigger build:**
   - Manual: Click "Build Now"
   - Automatic: Configure GitHub webhook (optional)

## 🗄️ Database Configuration

**MySQL Credentials:**
- Host: `db` (Docker network)
- User: `root`
- Password: `rootpassword`
- Database: `todo_db`
- Port: `3306`

**Database Schema:**
```sql
CREATE TABLE todos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    task VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🐳 Docker Configuration

### Services

**1. MySQL Database (`db`):**
- Image: `mysql:8.0`
- Container: `mysql_db`
- Volume: Persistent data storage
- Healthcheck: Ensures DB is ready before Flask starts

**2. Flask Application (`web`):**
- Built from Dockerfile
- Container: `flask_app`
- Port: `5000:5000`
- Depends on: MySQL database
- Auto-restart: On failure

## 📊 Performance & Monitoring

- **Container Status:** `docker ps`
- **Application Logs:** `docker-compose logs -f`
- **Database Logs:** `docker-compose logs db`
- **Flask Logs:** `docker-compose logs web`

## 🔐 Security Considerations

- SSH key-based authentication for EC2
- MySQL credentials via environment variables
- Security groups restrict EC2 access
- Docker network isolation

## 🚧 Troubleshooting

### MySQL Connection Error
```bash
docker-compose restart db
docker-compose restart web
```

### Port Already in Use
```bash
docker-compose down
sudo lsof -i :5000
kill -9 <PID>
```

### Rebuild Containers
```bash
docker-compose down -v
docker-compose up -d --build
```

## 📈 Future Enhancements

- [ ] Add user authentication
- [ ] Implement task categories
- [ ] Add due dates and priorities
- [ ] Setup HTTPS with SSL certificate
- [ ] Configure custom domain name
- [ ] Add monitoring (Prometheus/Grafana)
- [ ] Implement automated backups
- [ ] Add unit and integration tests

## 👤 Author

**Mithun MM**
- GitHub: [@MITHUNSOPRANO7](https://github.com/MITHUNSOPRANO7)

## 📄 License

This project is open source and available for educational purposes.

## 🙏 Acknowledgments

- Flask documentation
- Docker documentation
- Jenkins documentation
- AWS documentation

---

**⭐ If you found this project helpful, please give it a star!**
