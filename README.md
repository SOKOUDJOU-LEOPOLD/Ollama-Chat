# Ollama Chat

A full-stack chat application that provides a web interface for interacting with local LLMs through Ollama.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Development](#development)
- [Troubleshooting](#troubleshooting)

## Overview

Ollama Chat is a lightweight chat interface that connects a React frontend to a Flask backend, enabling conversations with locally-hosted large language models via [Ollama](https://ollama.ai/).

### Features

- Real-time chat interface
- Message history within session
- Loading state indicators
- CORS-enabled API for local development

## Architecture

```
┌─────────────────┐     HTTP/JSON     ┌─────────────────┐     Python SDK     ┌─────────────────┐
│   React Client  │ ───────────────── │   Flask Server  │ ────────────────── │     Ollama      │
│   (Port 5173)   │                   │   (Port 5001)   │                    │   (Port 11434)  │
└─────────────────┘                   └─────────────────┘                    └─────────────────┘
```

## Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v18 or higher)
- [Python](https://www.python.org/) (v3.10 or higher)
- [Ollama](https://ollama.ai/)

### Ollama Setup

1. Install Ollama from [ollama.ai](https://ollama.ai/)
2. Pull a model:
   ```bash
   ollama pull llama3.2
   ```
3. Verify the installation:
   ```bash
   ollama list
   ```

## Installation

### Clone the Repository

```bash
git clone <repository-url>
cd Ollama-Chat
```

### Server Setup

1. Create and activate a virtual environment:
   ```bash
   python -m venv .venv

   # Windows
   .venv\Scripts\activate

   # macOS/Linux
   source .venv/bin/activate
   ```

2. Install Python dependencies:
   ```bash
   pip install flask flask-cors ollama
   ```

### Client Setup

1. Navigate to the client directory:
   ```bash
   cd client
   ```

2. Install Node.js dependencies:
   ```bash
   npm install
   ```

## Usage

### Starting the Application

1. **Start the Flask server** (from the `server` directory):
   ```bash
   cd server
   python main.py
   ```
   The server will start on `http://localhost:5001`

2. **Start the React client** (from the `client` directory):
   ```bash
   cd client
   npm run dev
   ```
   The client will start on `http://localhost:5173`

3. Open your browser and navigate to `http://localhost:5173`

## API Reference

### POST /ai

Send a message to the AI model and receive a response.

**Request:**

```json
{
  "message": "Hello, how are you?"
}
```

**Response:**

```json
{
  "reply": "I'm doing well, thank you for asking! How can I help you today?"
}
```

**Error Responses:**

| Status Code | Description |
|-------------|-------------|
| 400 | No message provided in request body |
| 500 | Internal server error (e.g., model not found) |

## Project Structure

```
Ollama-Chat/
├── client/                 # React frontend application
│   ├── public/            # Static assets
│   ├── src/
│   │   ├── App.jsx        # Main chat component
│   │   ├── App.css        # Application styles
│   │   ├── main.jsx       # React entry point
│   │   └── index.css      # Global styles
│   ├── index.html         # HTML template
│   ├── package.json       # Node.js dependencies
│   ├── vite.config.js     # Vite configuration
│   └── eslint.config.js   # ESLint configuration
├── server/
│   └── main.py            # Flask API server
├── .venv/                 # Python virtual environment
└── README.md              # This file
```

## Configuration

### Changing the AI Model

To use a different Ollama model, update the `model` parameter in `server/main.py`:

```python
response: ChatResponse = chat(model='your-model-name', messages=[...])
```

Available models can be listed with:
```bash
ollama list
```

### Port Configuration

| Component | Default Port | Configuration Location |
|-----------|--------------|----------------------|
| Flask Server | 5001 | `server/main.py` - `app.run(port=5001)` |
| React Client | 5173 | `client/vite.config.js` |
| Ollama | 11434 | Ollama system settings |

### CORS Settings

The server allows requests from `http://localhost:5173` by default. To modify allowed origins, update `server/main.py`:

```python
CORS(app, origins=["http://localhost:5173", "http://your-domain.com"])
```

## Development

### Available Scripts

**Client:**

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build |
| `npm run lint` | Run ESLint |

**Server:**

| Command | Description |
|---------|-------------|
| `python main.py` | Start Flask development server |

### Tech Stack

**Frontend:**
- React 19
- Vite 7
- ESLint

**Backend:**
- Python 3
- Flask
- Flask-CORS
- Ollama Python SDK

## Troubleshooting

### Model Not Found Error

```
Error: model 'llama3' not found (status code: 404)
```

**Solution:** Pull the required model or update `server/main.py` to use an installed model.

```bash
# Check installed models
ollama list

# Pull a model
ollama pull llama3.2
```

### CORS Errors

If you see CORS-related errors in the browser console, ensure:
1. The Flask server is running
2. The client origin matches the CORS configuration in `server/main.py`

### Connection Refused

If the client cannot connect to the server:
1. Verify the Flask server is running on port 5001
2. Check that Ollama is running (`ollama serve`)
3. Ensure no firewall is blocking the connections
