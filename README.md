# 🌍 Intelligent Multilingual Document QA

An intelligent document question-answering system that allows users to upload multiple PDF documents and ask questions in different languages.

The system combines **OCR, multilingual embeddings, hybrid retrieval, BM25, FAISS, cross-encoder reranking, graph-based context expansion, question classification, multi-document reasoning, and voice interaction** to provide evidence-based answers.

---

## ✨ Features

- 📄 **Multi-PDF Document Processing**
  - Upload and process multiple PDF documents.
  - Extract text and document metadata.
  - Supports scanned PDFs through OCR.

- 🌍 **Multilingual Question Support**
  - Questions can be asked in different languages.
  - Language detection and translation are used during processing.
  - Answers are generated in English.

- 🔎 **Hybrid Information Retrieval**
  - Dense semantic retrieval using multilingual sentence embeddings.
  - Sparse retrieval using BM25.
  - Combines semantic and lexical search results.

- ⚡ **FAISS Vector Search**
  - Uses FAISS for efficient similarity search.
  - Supports GPU acceleration when available.

- 🎯 **Cross-Encoder Reranking**
  - Retrieved documents are reranked using a cross-encoder model to improve relevance.

- 🕸️ **Graph-Based Context Expansion**
  - Named entities are extracted from documents using spaCy.
  - Entity relationships are represented as a lightweight graph.
  - Related chunks can be added during retrieval.

- 🧠 **Question Classification**
  - Questions are classified into:
    - Metadata
    - Factual
    - Boolean
    - Interpretive

- 📚 **Multi-Document Reasoning**
  - Retrieves information from multiple documents.
  - Summarizes relevant contexts before generating an answer.

- 🎙️ **Voice Interaction**
  - Voice questions are transcribed using OpenAI Whisper.
  - Answers can be converted to speech using gTTS.

- 📖 **Provenance & Evidence**
  - Displays retrieved document chunks and provenance information.
  - Helps users understand where the answer came from.

- 📊 **Evaluation Support**
  - Includes ROUGE and BERTScore evaluation utilities.
  - Latency logging is included for retrieval and debugging.

- 💻 **Interactive Gradio Interface**
  - PDF upload and indexing.
  - Text-based questions.
  - Voice-based questions.
  - Answer history.
  - Entity visualization.
  - Provenance information.
  - Latency/debug information.


## 🏗️ System Architecture

                    ┌───────────────────┐
                    │    PDF Upload     │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │ PDF Text Extraction│
                    │      + OCR         │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │  Text Chunking    │
                    └─────────┬─────────┘
                              │
              ┌───────────────┴────────────────┐
              │                                │
       ┌──────▼───────┐                ┌───────▼──────┐
       │ Multilingual │                │     BM25     │
       │  Embeddings  │                │    Search    │
       └──────┬───────┘                └───────┬──────┘
              │                                │
              │          ┌──────────────┐      │
              └─────────►│Hybrid Search │◄─────┘
                         └──────┬───────┘
                                │
                       ┌────────▼────────┐
                       │ Graph Expansion │
                       └────────┬────────┘
                                │
                       ┌────────▼────────┐
                       │ Cross-Encoder   │
                       │   Reranking     │
                       └────────┬────────┘
                                │
                       ┌────────▼────────┐
                       │ Question Type   │
                       │ Classification  │
                       └────────┬────────┘
                                │
                       ┌────────▼────────┐
                       │  FLAN-T5 Based  │
                       │ Answer Generation│
                       └────────┬────────┘
                                │
              ┌─────────────────┼──────────────────┐
              │                 │                  │
       ┌──────▼─────┐    ┌──────▼─────┐    ┌──────▼─────┐
       │   Answer   │    │ Provenance │    │ Voice / TTS │
       └────────────┘    └────────────┘    └────────────┘
🧰 Technologies Used
Component	Technology
Programming Language	Python
PDF Processing	PyMuPDF
OCR	Tesseract
Embeddings	intfloat/multilingual-e5-large
Vector Search	FAISS
Sparse Retrieval	BM25
Reranking	cross-encoder/ms-marco-MiniLM-L-6-v2
Language Model	FLAN-T5
NLP / NER	spaCy
Speech Recognition	OpenAI Whisper
Text-to-Speech	gTTS
Translation	Google Translator
Caching	Redis
Graph Processing	NetworkX
Evaluation	ROUGE, BERTScore
User Interface	Gradio
Optional Vector DB	Pinecone
🤖 Models
Embedding Model
intfloat/multilingual-e5-large

Used for multilingual semantic representation of document chunks and queries.

Reranker
cross-encoder/ms-marco-MiniLM-L-6-v2

Used to rerank retrieved document chunks according to their relevance to the query.

Question Answering / Reasoning
google/flan-t5-large

Used for summarization and answer generation based on retrieved contexts.

Speech Recognition
Whisper small

Used to convert voice questions into text.

🔍 Retrieval Pipeline

The system uses a hybrid retrieval strategy.

1. Dense Retrieval

Documents are converted into multilingual embeddings using:

intfloat/multilingual-e5-large

FAISS performs similarity search over the embeddings.

2. Sparse Retrieval

BM25 retrieves documents based on lexical matching.

3. Hybrid Retrieval

Dense and sparse scores are combined:

Combined Score =
    0.6 × Semantic Score +
    0.4 × BM25 Score
4. Graph Expansion

Named entities are extracted using spaCy.

Related document chunks containing important entities can be added to the candidate set.

5. Cross-Encoder Reranking

The retrieved candidates are finally reranked using:

cross-encoder/ms-marco-MiniLM-L-6-v2

This produces the final ranked context used for answering the question.

📄 PDF Processing

The system first attempts normal PDF text extraction.

For scanned or image-based pages, it falls back to OCR:

PDF
 │
 ├── Text available ──► PyMuPDF
 │
 └── No text ─────────► Tesseract OCR

Document metadata such as:

Title
Author
Subject
Keywords

is also extracted and indexed.

🧠 Question Classification

Questions are categorized into four types:

Metadata

Example:

What is the title of the document?
Factual

Example:

What projects are mentioned in the document?
Boolean

Example:

Is the candidate experienced in machine learning?
Interpretive

Example:

Does the document suggest that the candidate has strong technical experience?

Different answer-generation strategies are used depending on the question type.

🎙️ Voice Pipeline

The system also supports voice-based questions.

User Voice
    │
    ▼
Whisper
    │
    ▼
Transcription
    │
    ▼
Document QA Pipeline
    │
    ▼
Generated Answer
    │
    ▼
gTTS
    │
    ▼
Audio Response
💻 Running the Project

The project is currently provided as a Google Colab notebook.

1. Open the notebook

Open:

intelligent_docqa.ipynb

in Google Colab.

2. Run the installation cell

The notebook installs the required Python and system dependencies.

3. Upload PDF documents

Use the Gradio interface to upload one or more PDF files.

4. Build the index

Click:

Build Index

The documents will be:

Extracted
   ↓
Chunked
   ↓
Embedded
   ↓
Indexed with FAISS
   +
Indexed with BM25
5. Ask questions

Questions can be entered using text or voice.

The system retrieves relevant evidence and generates an English answer.

📊 Evaluation

The notebook includes evaluation utilities using:

ROUGE
BERTScore

Example:

evaluate_answer(
    predicted_answer,
    reference_answer
)

The system also records latency information for retrieval and debugging.

🗂️ Project Structure
intelligent-multilingual-docqa/
│
├── intelligent_docqa.ipynb
└── README.md
🚀 Future Improvements

Possible future improvements include:

 Modularize the notebook into Python packages
 Add persistent FAISS index storage
 Improve graph-based retrieval
 Add citation-aware answer generation
 Add conversational memory
 Add better multilingual answer generation
 Add a proper evaluation dataset
 Compare retrieval strategies quantitatively
 Add Docker support
 Deploy the Gradio application
 Add Pinecone as an optional production vector database
 Improve OCR for complex document layouts
👩‍💻 Author

Anam Waheed
       
