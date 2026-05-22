Music Architect AI 🎵🎨
An enterprise-grade, cloud-native multimodal AI platform that orchestrates real-time, high-fidelity music generation and synchronized visual artwork synthesis.

By leveraging a Retrieval-Augmented Generation (RAG) pipeline grounded in cultural musicology and a robust AWS serverless orchestration layer, Music Architect AI bypasses the traditional limitations and computational overhead of fine-tuning, delivering deterministic, culturally accurate audio-visual assets in under 12 seconds.

🚀 Key Features
Intelligent Prompt Augmentation (RAG): Uses ChromaDB and bge-small-en-v1.5 embeddings to intercept user prompts and inject precise, real-world music theory metrics (BPM ranges, specific musical keys/modes, hybrid instrumentation briefs) sourced from curated global datasets.

Native Cloud Orchestration: Replaced third-party handlers with AWS Step Functions, enabling an asynchronous, stateful, and highly resilient multi-stage generation lifecycle.

Dedicated GPU Inference: Hosts advanced foundation models (Qwen 2.5-7B, ACE-Step 1.5, and SDXL Turbo) on "warm" Amazon SageMaker Endpoints backed by EC2 G5 (NVIDIA A10G) hardware, eliminating serverless cold-start latency.

Secure & Performant Media Delivery: High-speed streaming and parallel asset uploads directly to AWS S3, served to the client via secure, expiring S3 Presigned URLs.

🏗️ System Architecture
[User Browser: Next.js 15] 
       │ (HTTPS REST API)
       ▼
[FastAPI Gateway (EC2/App Runner)]
       │ (Boto3 SDK / IAM Authenticated)
       ▼
[AWS Step Functions (State Machine Orchestrator)]
       ├── 1. Augmentation Phase ──> [ChromaDB Vector Store]
       ├── 2. Dispatch Phase ──────> [Amazon SageMaker Endpoint (EC2 G5)]
       │                                  ├── Audio Generation (ACE-Step 1.5)
       │                                  └── Image Generation (SDXL Turbo)
       └── 3. Delivery Phase ──────> [AWS S3 Bucket] ──> Secure URL to Client
🛠️ Tech Stack
Frontend: Next.js 15, React Server Components (RSC), TailwindCSS

Backend: FastAPI (Python), Boto3 SDK

Cloud & Infrastructure: AWS Step Functions, Amazon SageMaker, AWS S3, Amazon ECR, Docker, AWS VPC

AI & Data Engineering: ChromaDB, FastEmbed (BAAI/bge-small-en-v1.5), Qwen 2.5-7B, ACE-Step 1.5, SDXL Turbo

📋 Repository Structure
Plaintext


├── .github/workflows/     # CI/CD pipelines
├── backend/               # FastAPI application gateway
│   ├── app/
│   │   ├── api/           # Endpoints for prompt submission and status polling
│   │   ├── core/          # AWS Boto3 configurations and Step Function triggers
│   │   └── main.py        # Application entry point
│   └── Dockerfile
├── frontend/              # Next.js 15 UI client
├── rag_pipeline/          # Knowledge base engineering
│   ├── data/              # Curated JSON master files (sourced from HF, Kaggle, GitHub)
│   ├── index_documents.py # Script for document cleaning, splitting, and ChromaDB loading
│   └── embedder.py        # BGE embedding layer configuration
└── README.md
⚡ Quick Start
Prerequisites
Python 3.10+

Node.js 18+

Docker installed locally

Configured AWS CLI with permissions for Step Functions, SageMaker, and S3

1. Vector Database Setup (RAG)
Navigate to the RAG directory, populate your source data files, and initialize the persistent ChromaDB collection:

Bash


cd rag_pipeline
pip install -r requirements.txt
python index_documents.py
2. Backend Installation
Set up your local environment variables in backend/.env:

Extrait de code


AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=us-east-1
STEP_FUNCTIONS_ARN=arn:aws:states:...
CHROMADB_HOST=localhost
Run the FastAPI server:

Bash


cd ../backend
pip install -r requirements.txt
uvicorn app.main:app --reload
3. Frontend Installation
Run the development server:

Bash


cd ../frontend
npm install
npm run dev
Open http://localhost:3000 to interact with the platform.

📈 Technical Evolution Note
This project originally explored Supervised Fine-Tuning (SFT) on base versions of Qwen and ACE-Step 1.0. However, rigorous benchmarking against the generation capacity and native multilingual performance of ACE-Step 1.5 and optimized Qwen 2.5 configurations revealed that weight shifting introduced architectural drift. The strategy was consciously pivoted toward an Inference-Time Context Augmentation (RAG) pipeline orchestrated entirely within a secure AWS network perimeter, optimizing budget efficiency and real-time database adaptivity.
