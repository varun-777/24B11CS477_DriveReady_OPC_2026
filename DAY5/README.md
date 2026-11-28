# Day 5 –  Building a PDF-Based RAG System

## Overview

On Day 5 of the Agentic AI Workshop, I learned about **Retrieval-Augmented Generation (RAG)** and how to connect external documents with Large Language Models (LLMs).

I explored how documents can be converted into embeddings, stored in a vector index, and searched to retrieve relevant information based on a user's question.

As a practical implementation, I built a PDF-based Question Answering System. I uploaded a certificate PDF into the system and asked questions about the information available in it. The system retrieved relevant content from the certificate and used an LLM to generate answers.

## Concepts I Learned

### 1. Embedding Models

Learned how embedding models convert text into numerical vectors that represent their semantic meaning.

I experimented with two Sentence Transformer models:

- `all-MiniLM-L6-v2`
- `all-mpnet-base-v2`

I generated embeddings for the same sentence using both models and compared their output dimensions.

### 2. PDF Text Extraction and OCR

Learned how to extract text from PDF documents using PyMuPDF (`fitz`).

I also explored OCR (Optical Character Recognition) using Tesseract to extract text from scanned PDF pages where normal text extraction is not possible.

### 3. Text Chunking

Understood why large documents need to be divided into smaller chunks before generating embeddings.

In my implementation:
- Chunk size: 500 characters
- Chunk overlap: 100 characters

The overlap helps preserve context between consecutive chunks.

### 4. Vector Embeddings and FAISS

Used Sentence Transformers to convert the extracted text chunks into embeddings.

Learned how FAISS (Facebook AI Similarity Search) stores these vectors and helps retrieve relevant information efficiently.

I used `IndexFlatIP` with normalized embeddings to perform similarity-based retrieval.

### 5. Retrieval-Augmented Generation (RAG)

Understood how RAG combines information retrieval with LLM-based answer generation.

Instead of directly asking an LLM to answer a question using only its existing knowledge, RAG retrieves relevant information from an external document and provides it as context to the LLM.

### 6. LLM Integration Using Groq

Integrated the Groq API with the retrieval system.

The retrieved PDF content and user question are passed to the LLM, which generates an answer based on the provided context.

I also configured a system prompt to prevent the model from inventing information that is not available in the PDF.

---

## Practical Implementation – Certificate Question Answering

To understand the complete RAG workflow, I uploaded a certificate PDF into my implementation and tested it with questions related to the certificate.

### Step 1: Uploading the Certificate

Uploaded my certificate PDF using Google Colab's file upload functionality.

The system accepts the PDF and starts extracting its content.

### Step 2: Extracting Certificate Information

Used PyMuPDF to extract the text from the certificate.

If a page does not contain extractable text, the system converts it into an image and uses Tesseract OCR to recognize the text.

The extracted content is then stored for further processing.

### Step 3: Splitting the Content into Chunks

Divided the extracted certificate text into smaller chunks of 500 characters with an overlap of 100 characters.

This makes the document easier to process and search.

### Step 4: Generating and Storing Embeddings

Converted each text chunk into a numerical embedding using `all-MiniLM-L6-v2`.

Stored the generated embeddings in a FAISS index using `IndexFlatIP`.

Now, the system can search for relevant certificate information using vector similarity.

### Step 5: Asking Questions About the Certificate

After preparing the document and vector index, I integrated Groq to answer questions.

For example, I could ask:

- What is the name mentioned in the certificate?
- Which organization issued the certificate?
- What is the certificate awarded for?
- What is the date mentioned on the certificate?
- What is the name of the course or workshop?

These are examples of questions that can be asked depending on the information available in the uploaded certificate.

### Step 6: Retrieving Relevant Information

When I ask a question, the system:

1. Converts the question into an embedding using the same embedding model.
2. Searches the FAISS index for similar vectors.
3. Retrieves the top 3 relevant text chunks.
4. Combines the retrieved chunks and passes them to the LLM as context.

### Step 7: Generating the Answer

Groq receives the retrieved certificate information along with the user's question.

The LLM generates a clear and concise answer using the provided PDF context.

If the requested information is not available in the certificate, the system responds:

`The answer is not available in the PDF.`

The user can continue asking questions until they type `exit`.

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core implementation |
| Google Colab | Development environment |
| Sentence Transformers | Generating text embeddings |
| PyMuPDF | Extracting PDF text |
| Tesseract OCR | Extracting text from scanned pages |
| NumPy | Handling embedding vectors |
| FAISS | Vector storage and similarity search |
| Groq API | LLM integration and answer generation |

## Overall Workflow

**PDF Upload → Text Extraction → Chunking → Embedding Generation → FAISS Indexing → Question Embedding → Context Retrieval → LLM Response**

## Key Takeaways

Through this implementation, I understood how RAG connects external documents with LLMs.

I gained practical experience in PDF processing, OCR, text chunking, embedding generation, vector similarity search, context retrieval, and LLM integration.

The most important thing I learned is that the retrieval component finds relevant information, while the LLM uses that information to generate the final answer.

This implementation gave me a practical foundation for building document-aware AI applications and more advanced AI systems.
