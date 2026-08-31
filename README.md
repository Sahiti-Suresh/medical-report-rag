#  Medical Report Assistant using RAG

A Retrieval-Augmented Generation (RAG) based medical report assistant that allows users to ask questions about the contents of a medical report and receive context-aware, source-grounded responses.

> Note: The medical report used in this project is completely synthetic and is intended only for software development and RAG testing. It is not medical advice.

##  Project Overview

The system combines document retrieval with a Large Language Model (LLM).

Instead of asking the LLM to answer questions from its own knowledge, the system first retrieves relevant information from the uploaded medical report and then provides that information as context to the LLM.

##  Architecture

```text
Medical Report PDF
        ↓
    PDF Loader
        ↓
   Text Chunking
        ↓
  MiniLM Embeddings
        ↓
   ChromaDB
        ↓
  Similarity Search
        ↓
 Retrieved Chunks
        ↓
   Gemini LLM
        ↓
 Final Answer + Sources
```

##  Technologies Used

* Python
* LangChain
* Google Gemini
* Hugging Face Sentence Transformers
* `all-MiniLM-L6-v2`
* ChromaDB
* PyPDF

## How It Works

### 1. Document Loading

The medical report is loaded from a PDF using a PDF document loader.

### 2. Text Chunking

The extracted text is divided into smaller chunks using a recursive text splitter.

Chunking makes it easier to retrieve only the relevant portions of the document.

### 3. Embeddings

Each chunk is converted into a numerical vector using the Hugging Face Sentence Transformer model:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The model produces 384-dimensional embeddings.

### 4. Vector Database

The generated embeddings are stored in ChromaDB.

### 5. Retrieval

When a user asks a question, the question is converted into an embedding and compared with the document embeddings.

The most relevant chunks are retrieved using similarity search.

### 6. Generation

The retrieved chunks are provided to the Gemini LLM along with the user's question.

Gemini generates an answer using the retrieved context.

### 7. Grounded Responses

The system is instructed to avoid generating information that is not present in the retrieved context.

For example:

**Question:**

```text
What is the patient's blood pressure?
```

**Answer:**

```text
The patient's blood pressure is 138/88 mmHg.
```

If the user asks:

```text
What is the patient's blood group?
```

and the report does not contain blood-group information, the system responds that the information is not available in the report.

##  Example

### Question

> What is the patient's blood pressure?

### Retrieved Information

```text
Blood Pressure: 138/88 mmHg
```

### Generated Answer

```text
The patient's blood pressure is 138/88 mmHg.
```

### Source

```text
synthetic_medical_report.pdf — Page 1
```

## 🔐 API Key

The Gemini API key is not included in this repository.

The API key should be supplied through an environment variable rather than hard-coded into the notebook.

## 🚀 Future Improvements

* Streamlit-based user interface
* Support for multiple medical reports
* Conversation history
* Improved source highlighting
* Metadata-based filtering
* Evaluation of retrieval accuracy
* Deployment as a web application
