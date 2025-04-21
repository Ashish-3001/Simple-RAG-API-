#  Simple RAG API — Django + FAISS

This project implements a minimal **Retrieval-Augmented Generation (RAG)** backend using Django, FAISS, and sentence-transformers. It supports ingesting documents, querying them semantically using cosine similarity, and managing the vector store via reset operations.

---

##  Setup Instructions

Once your virtual environment is created and activated, install the dependencies:

```bash
pip install -r requirements.txt
```

---

##  Apply Migrations & Run the Server

```bash
python manage.py migrate
python manage.py runserver
```

---

##  Your Local API Will Now Be Running At

```
http://127.0.0.1:8000/api/
```

---

##  Running Tests

To run all tests, including validation for ingestion, query, and reset:

```bash
python manage.py test core
```

This test suite:
- Validates all endpoints (ingest, query, reset)
- Handles edge cases and error responses
- Automatically re-ingests the original Appendix documents after reset

---

##  API Endpoints

###  `POST /api/ingest/`
- **Method:** `POST`
- **Purpose:** Ingest a new document into the vector store
- **Why POST?** It modifies server state by adding a new document
- **Body Format:**
```json
{
  "text": "Your document content here"
}
```
- **Headers:** `Content-Type: application/json`
- **Success Response:**
```json
{
  "message": "Document ingested.",
  "id": 8
}
```

---

###  `GET /api/query/?text=your+query`
- **Method:** `GET`
- **Purpose:** Retrieve top 3 most relevant documents
- **Why GET?** It's a read-only semantic similarity lookup
- **Example:**
```
/api/query/?text=machine+learning
```
- **Success Response:**
```json
{
  "results": [
    {
      "id": 2,
      "text": "Retrieval-Augmented Generation (RAG)...",
      "similarity": 0.9123
    },
    ...
  ]
}
```

---

###  `POST /api/reset/`
- **Method:** `POST`
- **Purpose:** Clear all documents and reset FAISS index
- **Why POST?** It's a destructive operation affecting backend state
- **Request Body:** None
- **Success Response:**
```json
{
  "message": "Vector store and document map reset."
}
```

---

###  `GET /api/`
- **Method:** `GET`
- **Purpose:** Root API view for navigation
- **Success Response:**
```json
{
  "ingest": "http://127.0.0.1:8000/api/ingest/",
  "query": "http://127.0.0.1:8000/api/query/?text=example",
  "reset": "http://127.0.0.1:8000/api/reset/",
}
```
