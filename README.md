# Build-a-Complete-Medical-Chatbot-with-LLMs-LangChain-Pinecone-Flask-AWS

# How to run?
### STEPS:

Clone the repository

```bash
git clonehttps://github.com/GouthumKharvi/Medical-Chatbot-GenAI-.git
```
### STEP 01- Create a conda environment after opening the repository

```bash
conda create -n medicalbot python=3.10 -y
```

```bash
conda activate medicalbot
```


### STEP 02- install the requirements
```bash
pip install -r requirements.txt
```


### Create a `.env` file in the root directory and add your Pinecone & openai credentials as follows:

```ini
PINECONE_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
GROQ_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```


```bash
# run the following command to store embeddings to pinecone
python store_index.py
```

```bash
# Finally run the following command
python app.py
```

Now,
```bash
open up localhost:
```


### Techstack Used:

- Python
- LangChain
- Flask
- GPT
- Pinecone



# AWS-CICD-Deployment-with-Github-Actions

## 1. Login to AWS console.

## 2. Create IAM user for deployment

	#with specific access

	1. EC2 access : It is virtual machine

	2. ECR: Elastic Container registry to save your docker image in aws


	#Description: About the deployment

	1. Build docker image of the source code

	2. Push your docker image to ECR

	3. Launch Your EC2 

	4. Pull Your image from ECR in EC2

	5. Lauch your docker image in EC2

	#Policy:

	1. AmazonEC2ContainerRegistryFullAccess

	2. AmazonEC2FullAccess

	
## 3. Create ECR repo to store/save docker image
    - Save the URI: 785282204071.dkr.ecr.us-east-1.amazonaws.com/medicalbot

	
## 4. Create EC2 machine (Ubuntu) 

## 5. Open EC2 and Install docker in EC2 Machine:
	
	
	#optinal

	sudo apt-get update -y

	sudo apt-get upgrade
	
	#required

	curl -fsSL https://get.docker.com -o get-docker.sh

	sudo sh get-docker.sh

	sudo usermod -aG docker ubuntu

	newgrp docker
	
# 6. Configure EC2 as self-hosted runner:
    setting>actions>runner>new self hosted runner> choose os> then run command one by one


# 7. Setup github secrets:

   - AWS_ACCESS_KEY_ID
   - AWS_SECRET_ACCESS_KEY
   - AWS_DEFAULT_REGION
   - ECR_REPO
   - PINECONE_API_KEY
   - GROQ_API_KEY










<div align="center">

# 🏥 Medical Chatbot — GenAI

### A RAG-powered medical Q&A chatbot built with LangChain · Groq (Llama 3.3 70B) · Pinecone · Flask · Docker · AWS EC2

[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.1.1-black?logo=flask)](https://flask.palletsprojects.com/)
[![LangChain](https://img.shields.io/badge/LangChain-0.3.26-green)](https://www.langchain.com/)
[![Pinecone](https://img.shields.io/badge/Pinecone-VectorDB-purple)](https://www.pinecone.io/)
[![Groq](https://img.shields.io/badge/Groq-Llama--3.3--70B-orange)](https://groq.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20ECR-FF9900?logo=amazon-aws)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-Apache%202.0-red)](LICENSE)

</div>

---

## 📖 Overview

This project is a **complete end-to-end medical chatbot** that answers medical questions using a **Retrieval-Augmented Generation (RAG)** architecture. The knowledge base is the *Gale Encyclopedia of Medicine (2nd Edition)* — a multi-volume medical reference covering thousands of conditions, treatments, and diagnostic tests.

Instead of relying on a general-purpose LLM's training data alone, this chatbot **retrieves relevant chunks** from the medical PDF, embeds them in a Pinecone vector index, and passes them as context to **Groq's Llama-3.3-70b-versatile** model to generate grounded, accurate answers.

The app runs as a **Flask web server** with a custom chat UI, containerized via **Docker**, and deployed to **AWS EC2** through a fully automated **GitHub Actions CI/CD pipeline**.

---

## 🏗️ Architecture

```
User Query
    │
    ▼
Flask Web App (app.py)
    │
    ├──► HuggingFace Embeddings
    │    (sentence-transformers/all-MiniLM-L6-v2, dim=384)
    │
    ├──► Pinecone Vector Store
    │    Index: medical-chatbot | Region: us-east-1 | Records: 5,859
    │    Similarity Search (top-k=3)
    │
    └──► LangChain RAG Chain
         ├── Retriever → fetches top-3 relevant chunks
         ├── Prompt → system prompt + retrieved context + user question
         └── ChatGroq (llama-3.3-70b-versatile) → generates answer
                │
                ▼
           Response rendered in Chat UI (chat.html)
```

---

## ✨ Features

- 🔍 **RAG Pipeline** — retrieves exact content from the medical PDF before generating answers, preventing hallucination
- 🧠 **Llama 3.3 70B via Groq** — fast inference with a state-of-the-art open LLM (free Groq API)
- 📚 **Gale Encyclopedia of Medicine** as the knowledge base (5,859 indexed chunks in Pinecone)
- 🗃️ **Pinecone Vector Store** — dense index (dimension 384, cosine metric, on-demand, AWS us-east-1)
- 🌐 **Flask Web App** — clean chat UI served at port 8080
- 🐳 **Dockerized** — fully containerized for consistent deployment
- 🚀 **AWS EC2 + ECR** — production deployment on cloud infrastructure
- ⚙️ **GitHub Actions CI/CD** — automated build, push to ECR, and deploy to EC2 on every push to `main`

---

## 📁 Project Structure

```
Medical-Chatbot-GenAI-/
│
├── .github/
│   └── workflows/
│       └── cicd.yaml              # GitHub Actions CI/CD pipeline
│
├── data/
│   └── Medical_book.pdf           # Gale Encyclopedia of Medicine (knowledge base)
│
├── research/
│   └── trials.ipynb               # Jupyter notebook — prototyping & testing the RAG pipeline
│
├── src/
│   ├── __init__.py
│   ├── helper.py                  # PDF loading, text splitting, HuggingFace embeddings
│   └── prompt.py                  # System prompt for the medical assistant
│
├── static/
│   └── style.css                  # Custom dark-themed chat UI styles
│
├── templates/
│   └── chat.html                  # Flask HTML template — chat interface
│
├── app.py                         # Main Flask application
├── store_index.py                 # One-time script: loads PDF → chunks → embeds → upserts to Pinecone
├── Dockerfile                     # Docker image definition
├── requirements.txt               # Python dependencies
├── setup.py                       # Package setup (medical-chatbot-genai)
├── template.sh                    # Shell script to scaffold project directories and files
├── .gitignore
└── .env                           # API keys (not committed — see below)
```

---

## 🔬 Research & Prototyping (`research/trials.ipynb`)

Before building the full application, the entire pipeline was prototyped and tested in a Jupyter notebook (`research/trials.ipynb`) running inside the `medicalbot` conda environment. The notebook covers:

1. **PDF Loading** — using `DirectoryLoader` with `PyPDFLoader` to load `Medical_book.pdf` from the `Data/` folder
2. **Text Splitting** — using `RecursiveCharacterTextSplitter` (`chunk_size=500`, `chunk_overlap=20`)
3. **Metadata Filtering** — stripping all metadata except `source` to keep Pinecone records lean
4. **Embedding** — `sentence-transformers/all-MiniLM-L6-v2` via `HuggingFaceEmbeddings` (384 dimensions)
5. **Pinecone Indexing** — creating the `medical-chatbot` serverless index and upserting all chunks
6. **RAG Chain Assembly** — building the retriever → prompt → ChatGroq chain with LangChain
7. **Live Q&A Testing** — tested with questions like:
   - *"What is Acromegaly and gigantism?"*
   - *"What is Atkins diet and key terms?"*
   - *"What is cancer and its causes?"*
   - *"What is Acute kidney failure?"*

All answers were verified to be grounded in the retrieved medical PDF content.

---

## 🧩 Core Components

### `src/helper.py`

Handles all data pipeline utilities:

- **`load_pdf_file(data)`** — scans a directory for `*.pdf` files using `DirectoryLoader` + `PyPDFLoader` and returns a list of `Document` objects
- **`filter_to_minimal_docs(docs)`** — strips each document's metadata to only `source`, reducing noise in Pinecone records
- **`text_split(minimal_docs)`** — splits documents into 500-character chunks with 20-character overlap using `RecursiveCharacterTextSplitter`
- **`download_hugging_face_embeddings()`** — loads `sentence-transformers/all-MiniLM-L6-v2` locally via `HuggingFaceEmbeddings`, producing 384-dimensional vectors

### `src/prompt.py`

Defines the system prompt that controls how the LLM responds:

```
You are a Medical assistant for question-answering tasks.
Use ONLY the following retrieved context to answer the question.
Be specific and mention exact medical terms, chemicals, hormones
and causes as stated in the context.
Do NOT say 'not specified' if the context contains the answer.
If the answer is truly not in the context, say 'Not found in document'.
Keep the answer concise within 3 sentences.
```

### `store_index.py`

A one-time setup script that:
1. Loads the PDF from `data/`
2. Filters metadata and splits into chunks
3. Generates embeddings using `all-MiniLM-L6-v2`
4. Creates a Pinecone serverless index (`medical-chatbot`, dimension=384, cosine, AWS us-east-1) if it doesn't already exist
5. Upserts all 5,859 chunk embeddings into Pinecone via `PineconeVectorStore.from_documents()`

### `app.py`

The main Flask application:
1. Loads environment variables (`PINECONE_API_KEY`, `GROQ_API_KEY`)
2. Initialises embeddings and connects to the existing Pinecone index (`PineconeVectorStore.from_existing_index`)
3. Creates a similarity retriever (`search_type="similarity"`, `k=3`)
4. Initialises `ChatGroq` with `llama-3.3-70b-versatile`
5. Assembles the full RAG chain: `create_retrieval_chain` + `create_stuff_documents_chain`
6. Serves the chat UI at `/` and handles POST requests at `/get`
7. Runs on `0.0.0.0:8080`

### `templates/chat.html` + `static/style.css`

A custom Bootstrap 4 chat interface styled with a dark gradient theme. The frontend uses jQuery AJAX to send user messages to the `/get` endpoint and render bot responses in real-time — no page refresh needed.

---

## ⚙️ Tech Stack

| Layer | Technology |
|---|---|
| **LLM** | Groq — `llama-3.3-70b-versatile` |
| **Embeddings** | `sentence-transformers/all-MiniLM-L6-v2` (HuggingFace, local) |
| **Vector DB** | Pinecone (Dense, 384-dim, cosine, AWS us-east-1) |
| **RAG Framework** | LangChain (`langchain`, `langchain-pinecone`, `langchain-groq`) |
| **Web Framework** | Flask 3.1.1 |
| **Frontend** | HTML, Bootstrap 4, jQuery (AJAX) |
| **Containerization** | Docker (`python:3.10-slim-buster`) |
| **Cloud** | AWS EC2 (Ubuntu), AWS ECR |
| **CI/CD** | GitHub Actions |
| **Language** | Python 3.10 |

---

## 🚀 How to Run Locally

### Step 1 — Clone the Repository

```bash
git clone https://github.com/GouthumKharvi/Medical-Chatbot-GenAI-.git
cd Medical-Chatbot-GenAI-
```

### Step 2 — Create and Activate Conda Environment

```bash
conda create -n medicalbot python=3.10 -y
conda activate medicalbot
```

### Step 3 — Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4 — Set Up Environment Variables

Create a `.env` file in the root directory:

```ini
PINECONE_API_KEY = "your_pinecone_api_key_here"
GROQ_API_KEY    = "your_groq_api_key_here"
```

### Step 5 — Store Embeddings in Pinecone *(one-time setup)*

```bash
python store_index.py
```

This loads `Medical_book.pdf`, splits it into chunks, generates embeddings, and upserts them into your Pinecone index.

### Step 6 — Run the App

```bash
python app.py
```

Open your browser at: **[http://localhost:8080](http://localhost:8080)**

---

## ☁️ AWS Deployment with GitHub Actions CI/CD

This project is deployed to AWS using a fully automated CI/CD pipeline defined in `.github/workflows/cicd.yaml`.

### Infrastructure Setup

#### 1. IAM User
Create an IAM user with the following policies:
- `AmazonEC2ContainerRegistryFullAccess`
- `AmazonEC2FullAccess`

#### 2. Amazon ECR Repository
Create an ECR repository to store the Docker image:
```
785282204071.dkr.ecr.us-east-1.amazonaws.com/medicalbot
```

#### 3. EC2 Instance (Ubuntu)
Launch an EC2 Ubuntu instance and install Docker:

```bash
# Update system
sudo apt-get update -y
sudo apt-get upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

#### 4. Self-Hosted GitHub Actions Runner
Configure the EC2 instance as a self-hosted runner:
> GitHub repo → Settings → Actions → Runners → New self-hosted runner → choose Ubuntu → run the commands

#### 5. GitHub Secrets
Add the following secrets to your GitHub repository:

| Secret | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | IAM user access key |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret key |
| `AWS_DEFAULT_REGION` | e.g. `us-east-1` |
| `ECR_REPO` | ECR repository name |
| `PINECONE_API_KEY` | Pinecone API key |
| `GROQ_API_KEY` | Groq API key |

### CI/CD Pipeline (`.github/workflows/cicd.yaml`)

The pipeline has two jobs triggered on every push to `main`:

**Continuous Integration** (runs on `ubuntu-latest`):
1. Checkout code
2. Configure AWS credentials
3. Login to Amazon ECR
4. Build Docker image, tag as `latest`, push to ECR

**Continuous Deployment** (runs on the EC2 self-hosted runner):
1. Checkout code
2. Configure AWS credentials
3. Login to Amazon ECR
4. Pull latest image from ECR and run it on port 8080 with all secrets injected as environment variables

---

## 🗃️ Pinecone Index Details

| Property | Value |
|---|---|
| **Index Name** | `medical-chatbot` |
| **Record Count** | 5,859 |
| **Dimension** | 384 |
| **Metric** | Cosine |
| **Type** | Dense |
| **Cloud** | AWS |
| **Region** | us-east-1 |
| **Capacity Mode** | On-demand |

---

## 📦 Requirements

```
langchain==0.3.26
flask==3.1.1
sentence-transformers==4.1.0
pypdf==5.6.1
python-dotenv==1.1.0
langchain-pinecone==0.2.8
langchain-openai==0.3.24
langchain-community==0.3.26
langchain-groq==0.2.4
```

---

## 🤝 Author

**Gouthum Kharvi**  
📧 gouthumkharvi1899@gmail.com  
🔗 [GitHub](https://github.com/GouthumKharvi)

---

## 📄 License

This project is licensed under the **Apache 2.0 License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">
  <sub>Built with ❤️ using LangChain · Groq · Pinecone · Flask · AWS</sub>
</div>
