# HostelOps — Smart Hostel Complaint Management System

A web-based hostel complaint management system that helps students submit and track complaints while allowing administrators to manage complaints, monitor resolution progress, and maintain hostel operations efficiently.

---

## 🚀 Features

### 👨‍🎓 Student Features
- Student login and authentication.
- Submit hostel-related complaints.
- View submitted complaints.
- Track complaint status.
- Monitor complaint resolution progress.

### ⚙️ Admin Features
- Secure administrator login.
- View and manage student complaints.
- Update complaint status.
- Monitor total and resolved complaints.
- Access complaint statistics through the admin dashboard.

### 📊 Public Homepage
- Displays live total complaint count.
- Displays live resolved complaint count.
- Retrieves statistics from the backend API and MongoDB database.

---

## 🏗️ System Architecture

```text
                    INTERNET
                       |
                 PORT 80 (HTTP)
                       |
                NGINX CONTAINER
                  Reverse Proxy
                    /       \
                   /         \
                  /           \
             FRONTEND        /api/*
                |               |
         FRONTEND SERVICE   BACKEND SERVICE
          Static HTML       Node.js + Express
                                |
                             MONGODB
                           mongo:27017

                     DOCKER NETWORKS
```

### Architecture Overview

- **Nginx:** Acts as a reverse proxy and provides a single public entry point.
- **Frontend:** Serves the HostelOps web application.
- **Backend:** Handles authentication, complaint management, API requests, and business logic.
- **MongoDB:** Stores application data, including users and complaints.
- **Docker Compose:** Manages the application services and their communication.

Only Nginx publishes port 80 in the Docker Compose configuration. The backend and MongoDB do not have direct public port mappings.

---

## 🛠️ Technology Stack

| Component | Technology |
|---|---|
| Frontend | HTML5, CSS3, JavaScript |
| Backend | Node.js, Express.js |
| Database | MongoDB |
| ODM | Mongoose |
| Authentication | JSON Web Token (JWT) |
| Security | Helmet.js, Express Rate Limit |
| Reverse Proxy | Nginx |
| Containerization | Docker |
| Orchestration | Docker Compose |
| Deployment | AWS EC2 / Render |
| Database Hosting | MongoDB Atlas (Render deployment) |

---

## 📁 Project Structure

```text
HostelOpsMain/
│
├── backend/
│   ├── controllers/
│   │   ├── authController.js
│   │   └── complaintController.js
│   │
│   ├── middleware/
│   │   └── auth.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Complaint.js
│   │
│   ├── routes/
│   │   ├── authRoutes.js
│   │   └── complaintRoutes.js
│   │
│   ├── server.js
│   └── Dockerfile
│
├── frontend/
│   ├── index.html
│   ├── nginx-spa.conf
│   └── Dockerfile
│
├── nginx/
│   └── nginx.conf
│
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## 🚀 Quick Deployment Using Docker

### Prerequisites

Install the following before deployment:

- Docker
- Docker Compose
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/ashikapm06/HostelopsMain.git
```

### Step 2: Navigate to the Project

```bash
cd HostelopsMain
```

### Step 3: Configure Environment Variables

Create a `.env` file in the project root:

```env
JWT_SECRET=replace_with_a_long_random_secret
```

Use a strong, private secret. Never commit your actual `.env` file to GitHub.

### Step 4: Build and Start the Application

```bash
docker compose up --build -d
```

### Step 5: Check Running Containers

```bash
docker compose ps
```

### Step 6: Access the Application

```text
http://localhost
```

For an EC2 deployment:

```text
http://YOUR_EC2_PUBLIC_IP
```

Make sure the EC2 security group allows inbound HTTP traffic on port 80.

---

## 🐳 Docker Commands

| Purpose | Command |
|---|---|
| Build and start | `docker compose up --build -d` |
| View running containers | `docker compose ps` |
| View backend logs | `docker compose logs -f backend` |
| View frontend logs | `docker compose logs -f frontend` |
| View Nginx logs | `docker compose logs -f nginx` |
| Stop containers | `docker compose down` |
| Restart services | `docker compose restart` |
| Rebuild services | `docker compose up --build -d` |
| Access backend shell | `docker exec -it hostelops-backend sh` |

To stop containers without deleting persistent database data:

```bash
docker compose down
```

The named Docker volumes preserve MongoDB data and uploaded files.

---

## 🔒 Security

HostelOps incorporates multiple application and infrastructure security measures.

- **JWT Authentication:** Protects restricted API routes through token-based authentication.
- **Password Security:** User passwords are handled by the backend authentication system.
- **Helmet.js:** Adds HTTP security headers.
- **Rate Limiting:** Express rate limiting helps control excessive API requests.
- **CORS:** Configured to control cross-origin requests.
- **Environment Variables:** Used to manage sensitive configuration such as database connection strings and JWT secrets.
- **Docker Network Isolation:** MongoDB and backend services do not have direct public port mappings.
- **Nginx Reverse Proxy:** Provides a single public entry point for application traffic.

**Note:** The current Docker Compose configuration publishes HTTP on port 80. HTTPS requires additional TLS configuration.

---

## ☁️ AWS EC2 Deployment

HostelOps can be deployed on an AWS EC2 instance.

### Step 1: Launch an EC2 Instance

- Choose Ubuntu Server 22.04 LTS.
- Select an instance type suitable for your workload, such as `t2.micro` where available.
- Configure the required storage.

### Step 2: Configure Security Groups

Allow the following inbound traffic:

| Type | Port | Source |
|---|---:|---|
| SSH | 22 | Your IP address |
| HTTP | 80 | 0.0.0.0/0 |

Do not expose MongoDB port 27017 or backend port 5000 publicly.

### Step 3: Connect to EC2

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

### Step 4: Install Docker

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-v2 git
```

Enable Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

### Step 5: Clone the Repository

```bash
git clone https://github.com/ashikapm06/HostelopsMain.git
cd HostelopsMain
```

### Step 6: Configure Environment Variables

```bash
nano .env
```

Add a strong JWT secret:

```env
JWT_SECRET=replace_with_a_long_random_secret
```

Save and exit.

### Step 7: Deploy

```bash
sudo docker compose up --build -d
```

### Step 8: Access the Application

```text
http://YOUR_EC2_PUBLIC_IP
```

An Elastic IP can be associated with the EC2 instance to provide a stable public IPv4 address.

---

## 🌐 Render Deployment

HostelOps is also deployed using Render.

The deployment consists of:

- **Frontend:** Static site hosted on Render.
- **Backend:** Node.js API hosted as a Render web service.
- **Database:** MongoDB Atlas cloud database.

### Live Application

- **Frontend:** https://hostelops-frontend-3bld.onrender.com
- **Backend Health Check:** https://hostelops-backend-3qaz.onrender.com/api/health
- **Public Complaint Statistics:** https://hostelops-backend-3qaz.onrender.com/api/complaints/public-stats

The public statistics endpoint returns aggregate complaint counts without exposing personal information.

---

## 📡 API Overview

The backend provides REST API endpoints for authentication and complaint management.

| Endpoint | Method | Purpose |
|---|---|---|
| `/api/health` | GET | Check backend health |
| `/api/complaints/public-stats` | GET | Get public complaint statistics |
| `/api/complaints/stats` | GET | Get protected admin statistics |
| `/api/complaints` | GET | Retrieve complaints |
| `/api/complaints` | POST | Submit a complaint |
| `/api/auth/login` | POST | Authenticate a user |

Protected endpoints require the appropriate authentication token and authorization.

---

## 📈 Homepage Statistics

The public homepage displays statistics retrieved directly from the backend.

- Total complaints filed.
- Total complaints resolved.

These values are fetched from the MongoDB database through the public statistics API, rather than relying on hardcoded homepage numbers.

---

## 🗄️ Database

HostelOps uses MongoDB to store application information.

Main data collections include:

- **Users:** Student and administrator account information.
- **Complaints:** Complaint descriptions, categories, status, timestamps, and related information.

For the Docker Compose deployment, MongoDB uses a persistent Docker volume.

For the Render deployment, MongoDB Atlas provides the cloud database.

---

## 🔮 Future Enhancements

- Email and notification integration.
- Complaint priority classification.
- Complaint analytics and reporting.
- Student feedback and satisfaction ratings.
- HTTPS configuration and enhanced deployment security.
- Automated database backups.

---

## 📄 License

This project is developed as an academic and software engineering project. Contact the repository owner for permission before redistributing or using the project commercially.
