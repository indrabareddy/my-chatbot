# 📄 AI PDF RAG Chatbot

A simple AI-powered **Retrieval-Augmented Generation (RAG) chatbot** built using Python.

This application allows users to upload a PDF document and ask questions about its content. The application extracts text from the PDF, splits the text into smaller chunks, converts the chunks into embeddings, stores them in a FAISS vector database, retrieves relevant information, and uses an OpenAI LLM to generate the final answer.

## 🚀 Features

* Upload a PDF document
* Extract text from PDF
* Split PDF text into smaller chunks
* Generate embeddings using OpenAI
* Store embeddings using FAISS
* Retrieve relevant information using similarity search
* Ask questions about the uploaded PDF
* Generate answers using an OpenAI LLM
* Simple Streamlit user interface

## 🛠️ Technologies Used

* **Python**
* **Streamlit** – Web interface
* **pdfplumber** – Extract text from PDF
* **LangChain** – Build the RAG pipeline
* **OpenAI Embeddings** – Convert text into vector representations
* **FAISS** – Vector database for similarity search
* **OpenAI GPT-4o-mini** – Generate answers
* **Git & GitHub** – Version control

## 📁 Project Structure

```text
my-chatbot/
│
├── ragchatbot.py
├── requirements.txt
└── README.md
```

## 🔄 How the RAG Application Works

The application follows these main steps:

### 1. Upload PDF

The user uploads a PDF document through the Streamlit sidebar.

```python
file = st.file_uploader(
    "Upload a PDF file and start asking questions",
    type="pdf"
)
```

### 2. Extract Text

`pdfplumber` is used to extract text from every page of the uploaded PDF.

```python
with pdfplumber.open(file) as pdf:
    text = ""
    for page in pdf.pages:
        text += page.extract_text() + "\n"
```

### 3. Split Text into Chunks

The extracted text is divided into smaller chunks using LangChain's `RecursiveCharacterTextSplitter`.

```python
text_splitter = RecursiveCharacterTextSplitter(
    separators=["\n\n", "\n", ". ", " ", ""],
    chunk_size=1000,
    chunk_overlap=200
)
```

The overlap helps preserve context between neighboring chunks.

### 4. Generate Embeddings

Each text chunk is converted into a numerical vector using OpenAI embeddings.

```python
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small"
)
```

Embeddings allow the application to compare the meaning of the user's question with the document content.

### 5. Store Vectors in FAISS

The generated embeddings are stored in a FAISS vector store.

```python
vector_store = FAISS.from_texts(chunks, embeddings)
```

FAISS is used to efficiently search for relevant document chunks.

### 6. Retrieve Relevant Information

A retriever searches the vector database and returns relevant chunks.

```python
retriever = vector_store.as_retriever(
    search_type="mmr",
    search_kwargs={"k": 4}
)
```

The application retrieves the top relevant pieces of information from the PDF.

### 7. Create the Prompt

The retrieved information is provided to the LLM as context.

The prompt instructs the AI to answer using only the information retrieved from the PDF.

### 8. Generate the Answer

The retrieved context and user's question are passed to the OpenAI LLM.

```python
llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0.3,
    max_tokens=1000
)
```

The model generates the final response.

## 🔗 RAG Pipeline

```text
             PDF Upload
                  ↓
          Extract PDF Text
                  ↓
            Text Chunking
                  ↓
        Generate Embeddings
                  ↓
          FAISS Vector Store
                  ↓
            User Question
                  ↓
        Similarity Retrieval
                  ↓
       Relevant PDF Context
                  ↓
             LLM / GPT
                  ↓
          Generated Answer
```

## 💡 Example

### Upload

Upload a PDF document containing information about a particular topic.

### Question

```text
What is the main topic discussed in the document?
```

### Answer

The chatbot retrieves the relevant sections from the PDF and generates an answer based on that information.

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/indrabareddy/my-chatbot.git
```

### 2. Open the project

```bash
cd my-chatbot
```

### 3. Create a virtual environment

```bash
python -m venv venv
```

### 4. Activate the virtual environment

For Windows:

```bash
venv\Scripts\activate
```

### 5. Install dependencies

```bash
pip install -r requirements.txt
```

## 🔑 API Key Setup

This project uses the OpenAI API.

For security, do **not** hard-code the API key directly in the Python file.

Create a `.env` file:

```text
OPENAI_API_KEY=your_api_key_here
```

Add `.env` to `.gitignore`:

```text
.env
```

Then load the API key in Python using an environment variable.

## ▶️ Run the Application

Run the Streamlit application:

```bash
streamlit run ragchatbot.py
```

Streamlit will start the application in your browser.

## 📌 Important Concepts Learned

Through this project, I learned:

* Generative AI
* Large Language Models (LLMs)
* Retrieval-Augmented Generation (RAG)
* Text extraction
* Text chunking
* Embeddings
* Vector databases
* Similarity search
* Prompt engineering
* LangChain
* Streamlit
* FAISS

## 🎯 Project Objective

The main objective of this project is to understand how a basic RAG-based AI application works.

Instead of directly asking an LLM a question, the application first retrieves relevant information from the uploaded PDF and then provides that information to the LLM as context.

## 🔮 Future Enhancements

* Support multiple PDF files
* Add chat history
* Add conversation memory
* Display source documents
* Add PDF page references
* Improve the user interface
* Store vector embeddings permanently
* Deploy the application online

## 👨‍💻 Author

**Indra Bareddy**

GitHub: https://github.com/indrabareddy
