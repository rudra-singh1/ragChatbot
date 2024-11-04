# Biomedical RAG Application

A Retrieval Augmented Generation (RAG) implementation for medical domain queries using BioMistral-7B and other open-source components. This application provides accurate medical information retrieval and response generation while keeping all processing local and private.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)

## Features

- **Fully Local Processing**: All computations run on-premise without external API calls
- **Domain-Specific Models**: Uses medical-specialized language and embedding models
- **Self-hosted Vector Database**: Scalable vector storage using Qdrant
- **Interactive Web Interface**: Clean UI for easy interaction with the system
- **Document Source Tracking**: Provides source context for all generated responses

## Architecture

- **LLM**: BioMistral-7B (medical domain-specific model)
- **Embeddings**: PubMedBERT-based embeddings (medical domain-specific)
- **Vector Database**: Qdrant (self-hosted)
- **Framework**: LangChain + LlamaCpp
- **API**: FastAPI
- **Frontend**: HTML/JavaScript with Bootstrap

## Prerequisites

- Python 3.8+
- Docker
- 16GB+ RAM
- CPU with AVX2 support (for LlamaCpp)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/rudra-singh1/ragChatbot
cd biomedical-rag
```

2. Create and activate a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Download the BioMistral model:
- Get the quantized GGUF model from [TheBloke's HuggingFace repository](https://huggingface.co/TheBloke/biomistral-7b-GGUF)
- Place it in the project root directory

5. Start Qdrant:
```bash
docker pull qdrant/qdrant
docker run -p 6333:6333 qdrant/qdrant
```

## Configuration

1. Place your medical documents in the `data/` directory (supports PDF, TXT)
2. Update the model path in `app.py` if needed:
```python
LOCAL_LLM_PATH = "biomistral-7b.Q4_K_M.gguf"
```

## Usage

1. First, ingest your documents to create vectors:
```bash
python ingest.py
```

2. Start the application:
```bash
uvicorn app:app --reload
```

3. Access the web interface at `http://localhost:8000`

## API Endpoints

- `GET /`: Main web interface
- `POST /get_response`: Query endpoint
  - Input: `{"query": "your medical question here"}`
  - Returns: `{"answer": "response", "context": "source context", "source": "document name"}`

## Technical Details

### Vector Database Configuration
- Collection Name: `vector_db`
- Vector Dimension: 768 (PubMedBERT embedding size)
- Distance Metric: Cosine Similarity

### LLM Configuration
- Temperature: 0.1
- Max Tokens: 2048
- Model: BioMistral-7B (4-bit quantized)

### Document Processing
- Chunk Size: 700 tokens
- Chunk Overlap: 70 tokens
- Top-k retrieval: 2 documents

## Performance Considerations

- Average response time: 30-40 seconds on CPU
- RAM usage: ~8GB during operation
- Storage: Depends on document volume (vectors typically 20% of raw text size)

## Limitations

- CPU-only implementation (can be extended to GPU)
- No chat memory/history (stateless queries)
- Response time dependent on CPU capabilities
- Limited to medical domain queries

## Future Improvements

- [ ] Add chat memory for contextual conversations
- [ ] Implement streaming responses
- [ ] Add GPU support
- [ ] Improve document preview functionality
- [ ] Add more medical document formats support
- [ ] Implement authentication
