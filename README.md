# PraxaApp

PraxaApp is a Streamlit-based Retrieval-Augmented Generation (RAG) chat app for theater Q&A.
It combines:
- local PDF context documents,
- Chroma vector search,
- LangChain retrieval and prompting,
- an OpenRouter-hosted chat model.

## Features

- Chat UI built with Streamlit.
- Retrieval over local theater PDF documents.
- Answer responses with source/page references.
- Persistent local vector store using Chroma.

## Tech Stack

- Python
- Streamlit
- LangChain
- ChromaDB
- HuggingFace embeddings (`sentence-transformers/all-MiniLM-L6-v2`)
- OpenRouter via LangChain `ChatOpenAI`

## Project Structure

- `praxa_client.py`: Streamlit app entrypoint.
- `praxa_rag.py`: RAG chain wiring (retriever + prompt + model).
- `context.py`: PDF loading, chunking, embeddings, and vector store utilities.
- `model.py`: OpenRouter chat model wrapper.
- `context_data/`: Source PDF files used for retrieval.
- `chromadb/`: Persistent vector store files.
- `requirements.txt`: Python dependencies.

## Prerequisites

- Python 3.10+
- `pip`

## Installation

From the `PraxaApp` folder:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## API Key Setup

Set your OpenRouter API key as an environment variable before running the app:

```bash
export OPENROUTER_API_KEY="your_openrouter_api_key"
```

## Build/Refresh the Vector Store

If your `chromadb/` store is missing or you want to rebuild it from PDFs in `context_data/`:

```bash
python context.py
```

## Run the App

```bash
streamlit run praxa_client.py
```

Then open the local URL shown by Streamlit (usually `http://localhost:8501`).

## How It Works

1. `context.get_vector_store()` loads a persistent Chroma DB.
2. A retriever fetches relevant document chunks for each user question.
3. The prompt combines the question and retrieved context.
4. The chat model generates an answer.
5. The app displays the answer and source references.

## Notes

- Retrieval quality depends on the PDFs in `context_data/`.
- If you update or add PDFs, rebuild the vector store.
- Keep secrets out of source code and prefer environment variables for keys.
