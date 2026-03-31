# PhysioVision Backend API

## Overview

FastAPI-based REST API backend for PhysioVision, featuring user authentication, therapy management, RAG-powered chatbot, and real-time WebSocket communication.

## Prerequisites

- Python 3.10+
- MongoDB (local or cloud instance)
- Node.js (for frontend integration testing)

## Installation

### 1. Set up Python Virtual Environment

```bash
cd Backend
python -m venv venv

# On Windows:
venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Environment Configuration

Create a `.env` file in the Backend directory:

```env
MONGODB_URI=mongodb://localhost:27017/PhysioVision
MISTRAL_API_KEY=your_mistral_api_key_here
```

### 4. Initialize Database

```bash
python setup_database.py
```

Optionally, insert sample data:

```bash
python insert_sample_data.py
```

## Running the Server

### Development Mode

```bash
python -m uvicorn app.main:app --host 0.0.0.0 --port 8002 --reload
```

### Production Mode

```bash
python -m uvicorn app.main:app --host 0.0.0.0 --port 8002
```

The API will be available at `http://localhost:8002`

API documentation (Swagger): `http://localhost:8002/docs`

## Key Modules

### `app/main.py`

Main FastAPI application with:

- User authentication and registration
- User profile and health data management
- Therapy plan CRUD operations
- RAG-based chatbot for Q&A

### `app/chatbot.py`

RAG (Retrieval-Augmented Generation) pipeline using:

- Sentence Transformers for embeddings
- Chroma for vector storage
- MistralAI for LLM responses

### `app/Database/`

Database models and operations for MongoDB integration:

- User schemas
- Therapy plans
- Chat history

## API Endpoints

### Authentication

- `POST /api/register` - Register new user
- `POST /api/login` - User login
- `POST /api/logout` - User logout

### User Management

- `GET /api/users/{user_id}` - Get user profile
- `PUT /api/users/{user_id}` - Update user profile
- `POST /api/users/{user_id}/health-data` - Add health data

### Therapy

- `GET /api/therapy-plans` - Get all therapy plans
- `POST /api/therapy-plans` - Create new therapy plan
- `GET /api/therapy-plans/{plan_id}` - Get specific plan
- `PUT /api/therapy-plans/{plan_id}` - Update therapy plan

### Chatbot

- `POST /api/chat` - Send message to RAG chatbot
- `GET /api/chat/history` - Get chat history

## WebSocket Connections

### Vision Analysis WebSocket

Connect to `ws://localhost:8002/ws/vision` for real-time exercise analysis data from the Vision Backend.

## Project Structure

```
Backend/
├── app/
│   ├── __init__.py
│   ├── main.py              # Main FastAPI application
│   ├── chatbot.py           # RAG pipeline implementation
│   └── Database/            # MongoDB models and operations
├── requirements.txt         # Python dependencies
├── setup_database.py        # Database initialization script
├── insert_sample_data.py    # Sample data insertion script
└── README.md               # This file
```

## Troubleshooting

### MongoDB Connection Error

Ensure MongoDB is running:

```bash
# Windows
net start MongoDB

# macOS
brew services start mongodb-community

# Linux - Check your distribution's documentation
```

### Port Already in Use

To change the port, modify the port in the startup command:

```bash
python -m uvicorn app.main:app --host 0.0.0.0 --port 8003
```

Then update `NEXT_PUBLIC_API_URL` in the frontend's `.env.local`:

```env
NEXT_PUBLIC_API_URL=http://localhost:8003
```

### Missing Dependencies

Ensure all requirements are installed:

```bash
pip install -r requirements.txt --upgrade
```

## Development Guidelines

- Follow PEP 8 style guide
- Use type hints for better IDE support
- Keep endpoints RESTful and well-documented
- Handle errors gracefully with appropriate HTTP status codes
- Implement input validation using Pydantic models

## License

Proprietary - PhysioVision Project
