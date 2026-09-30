# 🚀 AI Document Intelligence System

[![FastAPI](https://img.shields.io/badge/FastAPI-0.104-green)](https://fastapi.tiangolo.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue)](https://www.postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-24.0-blue)](https://www.docker.com)

A production-grade **RAG (Retrieval-Augmented Generation)** API system for intelligent document querying. Built with FastAPI, PostgreSQL with pgvector, OpenAI, and deployed on AWS.

## 🌟 Features

### Core Features
- ✅ **PDF Document Processing** - Upload and automatically process PDF documents
- ✅ **Intelligent Chunking** - Semantic chunking with overlap for better context
- ✅ **Vector Embeddings** - Support for both local (Sentence Transformers) and cloud (OpenAI) embeddings
- ✅ **Vector Search** - PostgreSQL pgvector for efficient similarity search
- ✅ **RAG Pipeline** - Advanced prompting with context retrieval
- ✅ **Streaming Responses** - Real-time token streaming for better UX

### Production Features
- ✅ **Database Persistence** - PostgreSQL with pgvector extension
- ✅ **Connection Pooling** - Efficient database connection management
- ✅ **Structured Logging** - JSON logs for production monitoring
- ✅ **Error Handling** - Comprehensive error handling with retry logic
- ✅ **API Rate Limiting** - Protect against abuse (configurable)
- ✅ **CORS & Security Headers** - Production security middleware
- ✅ **Health Checks** - Readiness and liveness probes
- ✅ **Docker Support** - Multi-stage Docker builds
- ✅ **Docker Compose** - Complete local development setup
- ✅ **AWS Deployment Ready** - Deploy to ECS, EKS, or EC2

## 🏗️ Architecture
┌─────────────┐ ┌──────────────┐ ┌─────────────┐
│ Client │────▶│ FastAPI │────▶│ PostgreSQL │
│ │ │ (Uvicorn) │ │ + pgvector │
└─────────────┘ └──────────────┘ └─────────────┘
│ │
▼ ▼
┌──────────────┐ ┌─────────────┐
│ Redis │ │ OpenAI │
│ (Cache) │ │ (APIs) │
└──────────────┘ └─────────────┘

text

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/ai-document-intelligence.git
cd ai-document-intelligence
2. Configure Environment
bash
cp .env.example .env
# Edit .env with your OpenAI API key
3. Run with Docker Compose
bash
docker-compose up --build
The API will be available at http://localhost:8000

4. Test the API
bash
# Upload a document
curl -X POST http://localhost:8000/upload/ \
  -F "file=@/path/to/your/document.pdf"

# Query the document
curl -X POST http://localhost:8000/query/ \
  -H "Content-Type: application/json" \
  -d '{"query": "What is this document about?"}'

# Stream query response
curl -X POST http://localhost:8000/query/stream \
  -H "Content-Type: application/json" \
  -d '{"query": "Summarize the key points"}'
📚 API Documentation
Endpoints
Method	Endpoint	Description
POST	/upload/	Upload a PDF document
POST	/upload/batch	Upload multiple PDFs
POST	/query/	Query documents (RAG)
POST	/query/stream	Stream query response
GET	/documents/	List all documents
GET	/documents/{id}	Get document details
DELETE	/documents/{id}	Delete a document
GET	/health/	Health check
Query Request Schema
json
{
  "query": "What are the main findings?",
  "top_k": 5,
  "temperature": 0.7,
  "max_tokens": 500,
  "document_ids": [1, 2, 3]
}
Query Response Schema
json
{
  "query": "What are the main findings?",
  "answer": "Based on the documents, the main findings include...",
  "sources": [
    {
      "id": 42,
      "document_id": 1,
      "document_name": "research.pdf",
      "chunk_index": 3,
      "text": "Relevant text chunk...",
      "similarity_score": 0.89
    }
  ],
  "total_tokens": 450,
  "response_time_ms": 1234.56,
  "model_used": "gpt-3.5-turbo"
}
🛠️ Development
Local Setup (without Docker)
bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up PostgreSQL with pgvector
# See: https://github.com/pgvector/pgvector

# Run migrations
alembic upgrade head

# Start the server
uvicorn app.main:app --reload
Running Tests
bash
pytest tests/ -v --cov=app
🚢 Deployment to AWS Free Tier
Option 1: AWS ECS (Fargate)
Push Docker image to ECR

bash
aws ecr create-repository --repository-name rag-api
docker tag ai-document-intelligence:latest <account>.dkr.ecr.us-east-1.amazonaws.com/rag-api:latest
docker push <account>.dkr.ecr.us-east-1.amazonaws.com/rag-api:latest
Create RDS PostgreSQL (Free Tier)

Use db.t3.micro (free tier eligible)

Enable pgvector extension

Deploy ECS Service

Use Fargate (pay per use, free tier includes 750 hours)

Configure task definition with environment variables

Option 2: AWS EC2 (t2.micro)
bash
# Launch t2.micro EC2 instance (free tier)
ssh -i your-key.pem ec2-user@<public-ip>

# Install Docker
sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user

# Clone and run
git clone <your-repo>
cd ai-document-intelligence
docker-compose up -d
Option 3: Railway.app (Simpler)
Create account on Railway.app

Connect GitHub repository

Add environment variables

Deploy automatically

📊 Monitoring & Logging
Structured JSON Logs - All logs are in JSON format for easy parsing

Health Endpoints - /health, /health/ready, /health/live

Query Logging - All queries are logged with response times

Metrics Ready - Integrate with Prometheus/CloudWatch

🔒 Security Considerations
✅ Environment variables for secrets

✅ CORS properly configured

✅ Input validation with Pydantic

✅ Rate limiting to prevent abuse

✅ File validation (size, type)

✅ SQL injection protection via SQLAlchemy

🎯 Performance Optimization
Connection Pooling - Database connection reuse

Batch Processing - Batch embeddings for efficiency

Caching - Redis for frequent queries (optional)

Async Operations - FastAPI async endpoints

Background Processing - Async document processing

🧪 Testing
bash
# Run all tests
pytest

# Run with coverage
pytest --cov=app --cov-report=html

# Run specific test
pytest tests/test_api.py -v
📈 Roadmap
Add authentication (JWT)

Support for more document formats (DOCX, TXT, HTML)

Implement semantic cache

Add monitoring with Prometheus

WebSocket support for real-time updates

Multi-tenant support

Custom embedding models

🤝 Contributing
Contributions are welcome! Please read our contributing guidelines.


🙏 Acknowledgments
FastAPI for the amazing framework

OpenAI for GPT APIs

pgvector for PostgreSQL vector search

Sentence Transformers for local embeddings

📞 Support
For issues, please create a GitHub issue. For questions, contact: your.email@example.com

Built with ❤️ for production RAG systems

text

## **Final Steps - Push to GitHub & Deploy**

```bash
# Initialize git repository
git init
git add .
git commit -m "Initial commit: Production-grade RAG API"

# Create GitHub repository and push
git remote add origin https://github.com/yourusername/ai-document-intelligence.git
git branch -M main
git push -u origin main

# For AWS deployment, create a deployment script
cat > deploy-aws.sh << 'EOF'
#!/bin/bash

# Build Docker image
docker build -t rag-api:latest .

# Tag for ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <your-account>.dkr.ecr.us-east-1.amazonaws.com
docker tag rag-api:latest <your-account>.dkr.ecr.us-east-1.amazonaws.com/rag-api:latest
docker push <your-account>.dkr.ecr.us-east-1.amazonaws.com/rag-api:latest

# Update ECS service
aws ecs update-service --cluster rag-cluster --service rag-api-service --force-new-deployment
EOF

chmod +x deploy-aws.sh
Key Production Features Implemented:
✅ Database Persistence - PostgreSQL with pgvector

✅ Schema Validation - Pydantic models with validation

✅ Advanced Prompt Engineering - System prompts with context formatting

✅ Document Filtering - Filter by document IDs, status, date ranges

✅ Docker Configuration - Multi-stage Dockerfile + docker-compose

✅ Logging & Error Handling - Structured JSON logging with loguru

✅ Streaming Responses - Server-Sent Events for real-time answers

✅ Production Ready - Health checks, connection pooling, retry logic

This is a complete, production-grade system that will absolutely impress employers. The code is clean, well-structured, follows best practices, and is ready for AWS deployment on the free tier!