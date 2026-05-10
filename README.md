# StreamingApp

Stream premium video content, host live watch parties, and manage your catalogue with a modern microservice architecture. The platform now ships with a production-ready admin portal, real-time chat, S3-backed adaptive streaming, and a redesigned cinematic frontend experience.

<img width="1536" height="1024" alt="ChatGPT Image May 8, 2026, 10_30_47 PM" src="https://github.com/user-attachments/assets/2e930753-90f8-4f54-9316-27049e4b4217" />

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

<img width="1908" height="297" alt="stream serv" src="https://github.com/user-attachments/assets/8b093ac2-10d9-46be-8cbc-ee1f1ebeeb1d" />
<img width="1906" height="311" alt="auth service" src="https://github.com/user-attachments/assets/b76d42cf-d38f-4b85-a194-58a78c6898d0" />
<img width="1907" height="295" alt="admin service" src="https://github.com/user-attachments/assets/1fc79b7c-4cd3-46da-919d-ca8df5940238" />
<img width="1358" height="720" alt="st we" src="https://github.com/user-attachments/assets/b0009ed9-e27e-420e-b69e-bb41d9668e59" />
<img width="1364" height="720" alt="st-aws key" src="https://github.com/user-attachments/assets/952be0b3-44db-44ad-bdeb-03ac54217b10" />
<img width="1314" height="665" alt="st login 1" src="https://github.com/user-attachments/assets/fe875ba0-0326-4f97-869e-4ac101a1464f" />
<img width="1315" height="671" alt="ec2-2" src="https://github.com/user-attachments/assets/a4f67bcc-a3d0-4a8c-99d8-46dbcc301655" />

<img width="1315" height="662" alt="video run" src="https://github.com/user-attachments/assets/f31c8989-8ec8-45c5-aa76-829593c5c3e8" />
<img width="1323" height="661" alt="chat service" src="https://github.com/user-attachments/assets/ff522e36-79f7-49e6-88fc-5890fe2801e8" />

<img width="1908" height="297" alt="stream serv" src="https://github.com/user-attachments/assets/a60c475d-ff5f-40fc-a3d6-6fb898a69088" />

<img width="1365" height="716" alt="st-upload-run user" src="https://github.com/user-attachments/assets/edfd198e-247e-40be-920e-d84d5efaf48a" />

<img width="1918" height="1017" alt="docker buid" src="https://github.com/user-attachments/assets/bfff5c93-89ac-4bab-bff2-552ad1a48cdd" />
<img width="1918" height="1021" alt="doc comp" src="https://github.com/user-attachments/assets/1fabf066-ed1e-4743-acec-82dbdc8af628" />
<img width="1317" height="670" alt="user" src="https://github.com/user-attachments/assets/5b82d54a-7644-4c39-bd15-0660dbec9336" />







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
