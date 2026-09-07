# MediBot 🩺

MediBot is a **RAG-based medical chatbot** that answers user queries using information retrieved from medical documents.

The application uses **LangChain**, **Hugging Face embeddings**, **Pinecone Vector Database**, **OpenAI GPT-4o**, and **Flask** to build an intelligent question-answering system grounded in medical reference material.

---

## 🚀 Features

* Medical question-answering chatbot
* Retrieval-Augmented Generation (RAG)
* PDF-based knowledge source
* Semantic search using embeddings
* Pinecone vector database for document retrieval
* GPT-4o for response generation
* Flask-based web interface
* Context-aware answers based on retrieved medical information

---

## 🧠 How It Works

MediBot follows a Retrieval-Augmented Generation pipeline:

```text
Medical PDF
    ↓
PDF Loader
    ↓
Text Splitting
    ↓
Text Embeddings
    ↓
Pinecone Vector Database
    ↓
User Question
    ↓
Query Embedding
    ↓
Similarity Search
    ↓
Relevant Document Chunks
    ↓
Context + User Question
    ↓
GPT-4o
    ↓
Generated Answer
    ↓
Flask Web Interface
```

The medical PDF is divided into smaller chunks and converted into numerical vector representations called embeddings.

These embeddings are stored in Pinecone.

When a user asks a question, the query is also converted into an embedding. Pinecone performs similarity search and retrieves the most relevant document chunks.

The retrieved context is then passed to GPT-4o along with the user's question to generate the final response.

---

## 🛠️ Technologies Used

### Backend

* Python
* Flask

### AI / LLM

* OpenAI GPT-4o
* LangChain

### Embeddings

* Hugging Face Sentence Transformers
* `sentence-transformers/all-MiniLM-L6-v2`

### Vector Database

* Pinecone

### Document Processing

* PyPDFLoader
* RecursiveCharacterTextSplitter

---

## 📂 Project Structure

```text
MediBot/
│
├── data/
│   └── Medical_book.pdf
│
├── src/
│   ├── __init__.py
│   ├── helper.py
│   └── prompt.py
│
├── static/
│
├── templates/
│   └── chat.html
│
├── app.py
├── store_index.py
├── requirements.txt
├── setup.py
├── .env
└── README.md
```

### Important Files

**`app.py`**

Runs the Flask application and handles user questions.

It connects to the existing Pinecone vector index, retrieves relevant document chunks and passes them to the LLM for answer generation.

**`store_index.py`**

Processes the medical PDF, generates embeddings and stores them in Pinecone.

**`src/helper.py`**

Contains helper functions for:

* Loading PDF documents
* Filtering metadata
* Splitting documents into chunks
* Loading embedding models

**`src/prompt.py`**

Contains the system prompt used to guide the medical assistant.

---

## ⚙️ Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Shatadru281/MediBot.git
```

Navigate to the project directory:

```bash
cd MediBot
```

---

### 2. Create a Virtual Environment

For Windows:

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

For Linux / macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 🔐 Environment Variables

Create a `.env` file inside the project folder.

Add the following:

```env
PINECONE_API_KEY=your_pinecone_api_key
OPENAI_API_KEY=your_openai_api_key
```

Replace the values with your own API keys.

Do not upload the `.env` file to GitHub.

Add it to `.gitignore`:

```text
.env
venv/
__pycache__/
```

---

## 📚 Create the Vector Database

Before running the chatbot for the first time, process the medical PDF and store its embeddings in Pinecone.

Run:

```bash
python store_index.py
```

This script:

1. Loads the medical PDF.
2. Splits the document into smaller chunks.
3. Generates embeddings using MiniLM.
4. Creates/connects to the Pinecone index.
5. Stores the document vectors in Pinecone.

---

## ▶️ Run the Application

After creating the vector index, start the Flask application:

```bash
python app.py
```

You should see something similar to:

```text
Running on http://127.0.0.1:5000
```

Open the following address in your browser:

```text
http://127.0.0.1:5000
```

The MediBot chat interface should now be available.

---

## 🔍 Embedding Model

The project uses:

```text
sentence-transformers/all-MiniLM-L6-v2
```

This model converts text into **384-dimensional vectors**.

These vectors represent the semantic meaning of the text and allow similar medical concepts to be retrieved even if the exact words are different.

---

## 📦 Document Chunking

The medical document is divided into smaller sections before generating embeddings.

Current configuration:

```text
Chunk Size: 500
Chunk Overlap: 20
```

Chunk overlap helps preserve context when information is located near the boundary between two chunks.

---

## 🔎 Retrieval

The application uses Pinecone to perform semantic similarity search.

The retriever selects the most relevant document chunks:

```text
Top K = 3
```

This means that the three most relevant chunks are provided to the LLM as context.

---

## 💡 Why RAG?

Using an LLM alone can sometimes result in hallucinations or answers that are not grounded in a specific knowledge source.

Retrieval-Augmented Generation improves the system by retrieving relevant information from trusted documents before generating the response.

Instead of relying only on the model's internal knowledge:

```text
Question → LLM → Answer
```

MediBot uses:

```text
Question
   ↓
Knowledge Retrieval
   ↓
Relevant Medical Context
   ↓
LLM
   ↓
Answer
```

This helps generate more contextually relevant responses.

---

## 🧪 Example

### User

```text
What are the symptoms of diabetes?
```

### MediBot

```text
Common symptoms may include increased thirst, frequent urination,
fatigue, unexplained weight loss and blurred vision.

Please consult a qualified healthcare professional for medical advice.
```

The exact response depends on the information available in the indexed medical document.

---

## 🔮 Future Improvements

Possible improvements include:

* Support for multiple medical documents
* Displaying source references with answers
* Chat history
* User authentication
* Better retrieval using reranking
* Hybrid semantic and keyword search
* Conversation memory
* Medical source citations
* Confidence-based response filtering
* Integration with trusted medical APIs
* Improved UI
* Deployment using Docker or cloud platforms

---

## ⚠️ Medical Disclaimer

MediBot is intended for **educational and informational purposes only**.

It should not be used as a substitute for professional medical diagnosis, treatment or advice.

Always consult a qualified healthcare professional for medical concerns or emergencies.

---

## 👨‍💻 Author

**Shatadru**

GitHub:
`https://github.com/Shatadru281`

---

## 📄 License

This project is intended for educational and academic purposes.
