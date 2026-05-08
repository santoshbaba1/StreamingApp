# StreamingApp

Stream premium video content, host live watch parties, and manage your catalogue with a modern microservice architecture. The platform now ships with a production-ready admin portal, real-time chat, S3-backed adaptive streaming, and a redesigned cinematic frontend experience.
##
📺 StreamingApp — MERN Stack Video Streaming Platform
🚀 Project Overview

StreamingApp is a full-stack video streaming web application built using the MERN stack and deployed with modern DevOps practices.
-<img width="1536" height="1024" alt="ChatGPT Image May 8, 2026, 10_24_49 PM" src="https://github.com/user-attachments/assets/4855fe0b-7ce4-40f6-a6d7-7c7fed3cae1e" />



It enables:

👤 User registration & login (JWT authentication)
🎬 Admin video upload (video + thumbnail)
📺 Video browsing & streaming
🔐 Role-based access (Admin/User)
🗂 Local file storage (instead of Amazon S3)
🚀 Scalable deployment using Docker, Jenkins, and Kubernetes
🏗️ System Architecture
🧰 Tech Stack
🔹 Frontend
React.js
Material UI
Axios
🔹 Backend
Node.js
Express.js
🔹 Database
MongoDB
🔹 DevOps & Cloud
Docker
Jenkins (CI/CD)
AWS ECR (Container Registry)
AWS EKS (Kubernetes)
Nginx (Reverse Proxy)
📂 Project Structure
StreamingApp/
│
├── frontend/                  # React application
├── backend/
│   ├── authService/           # Authentication APIs
│   ├── adminService/          # Admin + Upload APIs
│
├── uploads/                   # Local storage
│   ├── videos/
│   └── thumbnails/
│
├── docker/                    # Docker configs
├── helm/                      # Helm charts
└── README.md
⚙️ Local Setup Instructions
1️⃣ Clone Repository
git clone https://github.com/UnpredictablePrashant/StreamingApp.git
cd StreamingApp
2️⃣ Backend Setup
cd backend/authService
npm install

cd ../adminService
npm install
3️⃣ Configure Environment Variables

Create .env in both services:

PORT=3001
MONGO_URI=mongodb://localhost:27017/streaming
JWT_SECRET=supersecret123
CLIENT_URL=http://localhost:3000
4️⃣ Start Backend Services
cd authService
npm start

cd ../adminService
npm start
5️⃣ Frontend Setup
cd frontend
npm install
npm start

👉 Access application:

http://localhost:3000
🎥 File Storage (Local Setup)

Instead of S3, files are stored locally:

uploads/
├── videos/
├── thumbnails/

Serve files via Express:

app.use('/uploads', express.static('uploads'));
🔐 Authentication Flow
User registers
Password hashed using bcrypt
JWT token generated on login
Token stored in localStorage
Protected routes validated via middleware
🐳 Docker Setup
Build Images
docker build -t streaming-frontend ./frontend
docker build -t streaming-auth ./backend/authService
docker build -t streaming-admin ./backend/adminService
Push to AWS ECR
aws ecr create-repository --repository-name streaming-app

Tag & push images:

docker tag streaming-frontend:latest <ECR_URI>
docker push <ECR_URI>
⚙️ CI/CD Pipeline (Jenkins)
Jenkins installed on EC2
Integrated with GitHub
Automated build & push to ECR
Sample Pipeline
pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'docker build -t streaming-app .'
      }
    }

    stage('Push') {
      steps {
        sh 'docker push <ECR_URI>'
      }
    }
  }
}
☸️ Kubernetes Deployment (EKS)
Create Cluster
eksctl create cluster --name streaming-cluster
Deploy Application
helm install streaming-app ./helm
Verify
kubectl get pods
kubectl get svc
🌐 Nginx Reverse Proxy

Nginx is used for:

Routing frontend and backend
Handling API paths (/api, /admin)
Serving static uploads
Eliminating CORS issues
📊 Monitoring & Logging
AWS CloudWatch (logs & metrics)
Application logs via Node.js
Nginx access logs
🔔 ChatOps (Bonus)
AWS SNS for alerts
Integration with Slack / Teams
🧪 Testing
API Testing
POST /api/register
POST /api/login
GET /api/verify
Upload Test (Admin)
POST /api/admin/upload
✅ Features Implemented
✔ User Authentication (JWT)
✔ Role-based Authorization
✔ Video Upload (Admin only)
✔ Local File Storage
✔ REST API Architecture
✔ CI/CD Pipeline
✔ Kubernetes Deployment
✔ Reverse Proxy (Nginx)
⚠️ Limitations
Local storage is not scalable
No CDN for video delivery
No adaptive streaming (HLS)
🚀 Future Enhancements
🎥 HLS streaming (adaptive bitrate)
☁️ S3 + CloudFront integration
🔐 Refresh token system
📱 Mobile optimization
📊 Advanced monitoring
👨‍💻 Author

Santosh Kumar Sharma

📎 Submission

GitHub Repository:
👉 https://github.com/UnpredictablePrashant/StreamingApp.git

⭐ Conclusion

This project demonstrates:

Full-stack MERN development
Real-world DevOps pipeline
Cloud-native deployment
Production-ready architecture



## Architecture

| Service | Port | Description |
| --- | --- | --- |
| `authService` | 3001 | User authentication, registration, JWT issuance |
| `streamingService` | 3002 | Video catalogue, S3 playback endpoints, public APIs |
| `adminService` | 3003 | Dedicated admin microservice for asset management and uploads |
| `chatService` | 3004 | Websocket + REST chat for live watch parties |
| `frontend` | 3000 | React SPA with revamped UI and integrated chat |
| `mongo` | 27017 | Shared MongoDB instance |

All backend services share common database models and utilities through `backend/common`.

## Environment Configuration

Create an `.env` for each service (or export variables before running). All services accept the standard AWS credentials for S3 access.

### Auth Service (`backend/authService/.env`)
```ini
PORT=3001
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-south-1
AWS_S3_BUCKET=
```

### Streaming Service (`backend/streamingService/.env`)
```ini
PORT=3002
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-south-1
AWS_S3_BUCKET=
AWS_CDN_URL=
STREAMING_PUBLIC_URL=http://localhost:3002
```

### Admin Service (`backend/adminService/.env`)
```ini
PORT=3003
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=ap-south-1
AWS_S3_BUCKET=
```

### Chat Service (`backend/chatService/.env`)
```ini
PORT=3004
MONGO_URI=mongodb://localhost:27017/streamingapp
JWT_SECRET=changeme
CLIENT_URLS=http://localhost:3000
```

### Frontend build variables (`frontend/.env` or Docker build args)
```ini
REACT_APP_AUTH_API_URL=http://localhost:3001/api
REACT_APP_STREAMING_API_URL=http://localhost:3002/api
REACT_APP_STREAMING_PUBLIC_URL=http://localhost:3002
REACT_APP_ADMIN_API_URL=http://localhost:3003/api/admin
REACT_APP_CHAT_API_URL=http://localhost:3004/api/chat
REACT_APP_CHAT_SOCKET_URL=http://localhost:3004
```

## Running with Docker Compose

1. Populate the environment variables above (or rely on the defaults baked into `docker-compose.yml`).
2. Build and start the stack:
   ```bash
   docker-compose up --build
   ```
3. Navigate to `http://localhost:3000` for the web app.

The compose file provisions MongoDB plus all four Node.js microservices. S3 credentials are optional for local testing—you can still browse seeded metadata, but streaming requires valid S3 objects.

## Local Development

Install dependencies for each service:

```bash
# auth service
cd backend/authService && npm install

# streaming service
cd ../streamingService && npm install

# admin service
cd ../adminService && npm install

# chat service
cd ../chatService && npm install

# frontend
cd ../../frontend && npm install
```

Run the services (in separate terminals) after starting MongoDB:

```bash
cd backend/authService && npm run dev
cd backend/streamingService && npm run dev
cd backend/adminService && npm run dev
cd backend/chatService && npm run dev
cd frontend && npm start
```

## Feature Highlights

- **S3-backed adaptive streaming** with secure signed uploads for admins.
- **Dedicated admin microservice** for video ingestion, metadata management, and featured curation.
- **Real-time chat** overlay in the player (Socket.IO + persistent message history).
- **Modern React experience** featuring cinematic hero sections, dynamic carousels, and responsive design.
- **Role-aware access control** across frontend routes and backend microservices.

## Testing

Automated tests are not yet included. Recommended smoke checks:

1. Register and log in through the web UI.
2. Upload a small video + thumbnail via the admin dashboard (requires valid S3 credentials).
3. Confirm playback from the browse page and verify that chat messages broadcast between multiple browser tabs.

## License
Santosh Kumar Sharma(12394)
MIT © StreamFlix Team
