# Backend (app/backend)

This directory contains the backend service for the RAG chat app, built with Python and Quart, designed to work with Azure OpenAI and Azure AI Search.

## Features
- Asynchronous web API using [Quart](https://quart.palletsprojects.com/)
- Integration with Azure OpenAI, Azure Cognitive Search, Azure Blob Storage, and Cosmos DB
- Multiple RAG (Retrieval-Augmented Generation) approaches
- Support for chat history, user uploads, and vision (GPT-4V)
- Telemetry and monitoring with Azure Monitor and OpenTelemetry
- Configurable authentication and access control
- Containerized with Docker

## Directory Structure
- `app.py` - Main Quart app and API routes
- `main.py` - Entrypoint for Gunicorn/Uvicorn
- `config.py` - Configuration constants
- `core/` - Core helpers (authentication, session, images)
- `approaches/` - RAG approaches and prompt management
- `prepdocslib/` - Document ingestion, parsing, and embedding
- `chat_history/` - Cosmos DB chat history integration
- `decorators.py` - Authentication decorators
- `error.py` - Error handling utilities
- `custom_uvicorn_worker.py` - Custom Uvicorn worker for logging
- `Dockerfile` - Container build file
- `requirements.txt` - Python dependencies

## Setup
1. Install Python 3.11+
2. Install dependencies:
   ```powershell
   pip install -r requirements.txt
   ```
3. Set required environment variables (see below)
4. Run locally:
   ```powershell
   python main.py
   ```
   Or with Gunicorn (recommended for production):
   ```powershell
   python -m gunicorn -b 0.0.0.0:8000 main:app
   ```

## Environment Variables
- `AZURE_OPENAI_CHATGPT_MODEL`, `AZURE_SEARCH_SERVICE`, `AZURE_SEARCH_INDEX`, etc. (see `app.py` for full list)
- Optional: `APPLICATIONINSIGHTS_CONNECTION_STRING`, `APP_LOG_LEVEL`, feature toggles (e.g., `USE_GPT4V`, `USE_USER_UPLOAD`)

## API Endpoints
- `/chat` and `/chat/stream` - Main chat endpoints
- `/ask` - Single-turn Q&A
- `/config` - Feature/configuration info
- `/list_uploaded`, `/delete_uploaded` - User file management
- See `app.py` for all routes

## Running in Docker
Build and run the backend in a container:
```powershell
docker build -t rag-backend .
docker run -p 8000:8000 --env-file .env rag-backend
```

## Telemetry & Logging
- Logging is configured via `APP_LOG_LEVEL` (default: INFO)
- Azure Monitor and OpenTelemetry are enabled if `APPLICATIONINSIGHTS_CONNECTION_STRING` is set

## Python Requirements

The backend depends on the following Python packages (see `requirements.txt` for full details):

```
aiofiles==24.1.0
aiohappyeyeballs==2.4.4
aiohttp==3.10.11
aiosignal==1.3.1
annotated-types==0.7.0
anyio==4.4.0
asgiref==3.8.1
attrs==24.2.0
azure-ai-documentintelligence==1.0.0b4
azure-cognitiveservices-speech==1.40.0
azure-common==1.1.28
azure-core==1.30.2
azure-core-tracing-opentelemetry==1.0.0b11
azure-cosmos==4.9.0
azure-identity==1.17.1
azure-monitor-opentelemetry==1.6.1
azure-monitor-opentelemetry-exporter==1.0.0b32
azure-search-documents==11.6.0b12
azure-storage-blob==12.22.0
azure-storage-file-datalake==12.16.0
beautifulsoup4==4.12.3
blinker==1.8.2
certifi==2024.7.4
cffi==1.17.0
charset-normalizer==3.3.2
click==8.1.7
cryptography==44.0.1
deprecated==1.2.14
distro==1.9.0
fixedint==0.1.6
flask==3.0.3
frozenlist==1.4.1
h11==0.14.0
h2==4.1.0
hpack==4.0.0
httpcore==1.0.5
httpx==0.27.0
hypercorn==0.17.3
hyperframe==6.0.1
idna==3.10
importlib-metadata==8.0.0
isodate==0.6.1
itsdangerous==2.2.0
jinja2==3.1.6
jiter==0.8.2
markdown-it-py==3.0.0
markupsafe==2.1.5
mdurl==0.1.2
microsoft-kiota-abstractions==1.9.3
microsoft-kiota-authentication-azure==1.9.3
microsoft-kiota-http==1.9.3
microsoft-kiota-serialization-form==1.9.3
microsoft-kiota-serialization-json==1.9.3
microsoft-kiota-serialization-multipart==1.9.3
microsoft-kiota-serialization-text==1.9.3
msal==1.30.0
msal-extensions==1.3.1
msgraph-core==1.3.3
msgraph-sdk==1.26.0
msrest==0.7.1
multidict==6.0.5
oauthlib==3.2.2
openai==1.63.0
opentelemetry-api==1.31.1
opentelemetry-instrumentation==0.52b1
opentelemetry-instrumentation-aiohttp-client==0.52b1
opentelemetry-instrumentation-asgi==0.52b1
opentelemetry-instrumentation-dbapi==0.52b1
opentelemetry-instrumentation-django==0.52b1
opentelemetry-instrumentation-fastapi==0.52b1
opentelemetry-instrumentation-flask==0.52b1
opentelemetry-instrumentation-httpx==0.52b1
opentelemetry-instrumentation-openai==0.39.0
opentelemetry-instrumentation-psycopg2==0.52b1
opentelemetry-instrumentation-requests==0.52b1
opentelemetry-instrumentation-urllib==0.52b1
opentelemetry-instrumentation-urllib3==0.52b1
opentelemetry-instrumentation-wsgi==0.52b1
opentelemetry-resource-detector-azure==0.1.5
opentelemetry-sdk==1.31.1
opentelemetry-semantic-conventions==0.52b1
opentelemetry-semantic-conventions-ai==0.4.3
opentelemetry-util-http==0.52b1
packaging==24.1
pillow==10.4.0
portalocker==2.10.1
priority==2.0.0
prompty==0.1.50
propcache==0.2.0
psutil==5.9.8
pycparser==2.22
pydantic==2.8.2
pydantic-core==2.20.1
pygments==2.18.0
pyjwt==2.10.1
pymupdf==1.25.1
pypdf==4.3.1
python-dotenv==1.0.1
pyyaml==6.0.2
quart==0.20.0
quart-cors==0.7.0
regex==2024.11.6
requests==2.32.3
requests-oauthlib==2.0.0
rich==13.9.4
six==1.16.0
sniffio==1.3.1
soupsieve==2.6
std-uritemplate==2.0.3
tenacity==9.0.0
tiktoken==0.8.0
tqdm==4.66.5
types-beautifulsoup4==4.12.0.20240511
types-html5lib==1.1.11.20241018
types-pillow==10.2.0.20240822
typing-extensions==4.12.2
urllib3==2.2.2
uvicorn==0.30.6
werkzeug==3.0.6
wrapt==1.16.0
wsproto==1.2.0
yarl==1.17.2
zipp==3.21.0
```

> **Note:** This is a partial list. For the full and most up-to-date list, see [`requirements.txt`](requirements.txt).

## References
- [Project README](../../README.md)
- [Backend customization](../../docs/customization.md)
- [Local development](../../docs/localdev.md)
- [Productionizing](../../docs/productionizing.md)

---
For more details, see the main project documentation and the source code in this directory.
