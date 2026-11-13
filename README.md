# 🧠 Smart RAG — Full-Stack Document Assistant & Retrieval Augmented Generation Platform

Smart RAG is a full-stack Retrieval Augmented Generation (RAG) application built with a **FastAPI** backend and a **React + TypeScript** chatbot frontend. Users can register, log in, upload multi-format documents or raw text, ask context-aware questions, and inspect retrieved source chunks in real time.

---

## 🌟 Key Highlights

- **Multi-Format Document Ingestion**: Upload and index `.txt`, `.pdf`, and `.docx` files or submit raw text directly into the vector store.
- **Semantic Vector Search**: Uses **Sentence Transformers** embeddings paired with **FAISS** vector indexing for fast, accurate context retrieval.
- **Chatbot-First RAG Interface**: Conversational React UI with real-time source/context inspection so users can verify which document chunks grounded each answer.
- **Redis Caching & Persistence**: Redis-backed user management, document metadata storage, and query caching to lower latency and API costs.
- **Role-Based JWT Authentication**: Secure user registration, login, token refresh, and admin user management endpoints.
- **Containerized Deployment**: Ready-to-run Docker and Docker Compose configurations for local development and production deployment.

---

## 🛠️ Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React, TypeScript, Vite, Tailwind CSS, TanStack Query, Axios, Lucide React |
| **Backend** | FastAPI, Python 3.12+, LangChain, Uvicorn |
| **AI & Vector Search** | FAISS, Sentence Transformers, OpenAI API (`gpt-4o-mini`) |
| **Database & Cache** | Redis |
| **DevOps & Auth** | JWT Authentication, Docker, Docker Compose, Nginx |

---

## 📂 Project Architecture

```txt
Smart-RAG/
├── backend/
│   ├── api/                 # FastAPI route handlers (auth, ingestion, query, health)
│   ├── app/                 # Core application configuration & lifecycle
│   ├── auth/                # JWT authentication & password hashing
│   ├── rag/                 # Document chunking, embeddings, FAISS store & LLM chain
│   ├── scripts/             # Utility scripts (admin user initialization)
│   ├── smart_rag/           # Domain models & schemas
│   ├── utils/               # Helper utilities & Redis client
│   ├── .env.example         # Environment variable template
│   ├── Dockerfile           # Backend container configuration
│   ├── main.py              # FastAPI entry point
│   └── requirements.txt     # Python dependencies
├── frontend/
│   ├── public/              # Static assets
│   ├── src/                 # React TypeScript components, hooks & API services
│   ├── .env.example         # Frontend environment template
│   ├── Dockerfile           # Frontend production container
│   ├── nginx.conf           # Nginx reverse proxy configuration
│   └── package.json         # Node dependencies
├── docker-compose.yml       # Production multi-container setup
├── docker-compose.dev.yml   # Development setup with hot-reload
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- **Python** 3.12+ & `uv`
- **Node.js** 24+ & `npm`
- **Redis** (local instance or cloud Redis)
- **OpenAI API Key**
- **Docker** *(optional)*

---

### 1. Clone the Repository
```bash
git clone [https://github.com/akshay-bytes11/Smart-RAG.git](https://github.com/akshay-bytes11/Smart-RAG.git)
cd Smart-RAG
```

---

### 2. Backend Setup
1. Navigate to the backend directory and install dependencies:
   ```powershell
   cd backend
   uv venv
   uv pip install -r requirements.txt
   ```
2. Create `backend/.env` from `backend/.env.example`:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   LLM_MODEL=gpt-4o-mini
   VECTOR_STORE_PATH=./vector_store
   REDIS_HOST=localhost
   REDIS_PORT=6379
   REDIS_PASSWORD=
   CORS_ORIGINS=http://localhost:5173,[http://127.0.0.1:5173](http://127.0.0.1:5173),http://localhost:3000
   SECRET_KEY=change-this-to-a-secure-secret-key-min-32-characters
   ADMIN_USERNAME=admin
   ADMIN_PASSWORD=admin123
   ADMIN_EMAIL=admin@example.com
   ```
3. *(Optional)* Initialize the default admin user after starting Redis:
   ```powershell
   uv run python scripts/init_admin.py
   ```
4. Start the FastAPI server:
   ```powershell
   uv run uvicorn main:app --reload
   ```
   - **Backend URL:** `http://localhost:8000`
   - **API Docs:** `http://localhost:8000/docs`

---

### 3. Frontend Setup
1. Open a new terminal and navigate to the frontend directory:
   ```powershell
   cd frontend
   npm install
   npm run dev
   ```
   - **Frontend URL:** `http://localhost:5173`
2. To override the default backend URL (`http://localhost:8000`), create `frontend/.env`:
   ```env
   VITE_API_BASE_URL=http://localhost:8000
   ```

---

### 4. Quick Start with Docker
Create `backend/.env` first, then run from the project root:
```powershell
docker compose up --build
```
- **Frontend:** `http://localhost:3000`
- **Backend:** `http://localhost:8000`
- **Redis:** `localhost:6379`

For backend Docker development with reload:
```powershell
docker compose -f docker-compose.dev.yml up --build
```

---

## 📡 API Endpoints

### Health
- `GET /api/health`

### Authentication
- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/refresh`
- `GET /api/auth/me`
- `POST /api/auth/logout`
- `GET /api/auth/users`
- `DELETE /api/auth/users/{user_id}`

### Ingestion
- `POST /ingest/upload-text`
- `POST /ingest/upload-file`
- `GET /ingest/sources`
- `DELETE /ingest/clear-all`

### Query
- `POST /api/query`
- `GET /api/cache/stats`

---

## 🌐 Deployment Notes

Frontend and backend are designed to deploy separately:
- **Frontend:** Vercel or Netlify (`VITE_API_BASE_URL=https://your-backend-domain.com`)
- **Backend:** Render, Railway, or Fly.io with persistent storage mounted to `VECTOR_STORE_PATH`
- **Redis:** Upstash Redis or managed Redis from your backend host

Backend production environment variables:
```env
OPENAI_API_KEY=your_openai_api_key_here
LLM_MODEL=gpt-4o-mini
REDIS_HOST=your_redis_host
REDIS_PORT=6379
REDIS_PASSWORD=your_redis_password
VECTOR_STORE_PATH=/app/vector_store
CORS_ORIGINS=[https://your-frontend-domain.com](https://your-frontend-domain.com)
SECRET_KEY=your_secret_key
```

---

## 🛡️ Git Safety
Do not commit real environment files containing API keys or secrets:
- `backend/.env`
- `frontend/.env`

Use `.env.example` files for shared configuration templates.

---

## 👨‍💻 Author
**Akshay Jain**
- GitHub: [@akshay-bytes11](https://github.com/akshay-bytes11)

---

## 📄 License
This project is licensed under the MIT License.
