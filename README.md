# AI Company Knowledge Base & RAG Assistant
An AI-powered Retrieval-Augmented Generation (RAG) system that enables organizations to centralize internal documents and retrieve information through a conversational AI assistant.
The system automatically processes documents uploaded to Google Drive, extracts their content, generates structured metadata using AI, prevents duplicate indexing with SHA-256 hashing, stores document records in Airtable, and indexes unique documents into Pinecone.
Employees can then interact with an AI chatbot that retrieves relevant information from the knowledge base and generates contextual responses with source citations.

**Tech Stack:** `n8n` · `Google Drive` · `Google Gemini` · `Pinecone` · `Airtable` · `Simple Memory` · `SHA-256`

---

## 📌 Problem
Organizations often accumulate large collections of internal documents such as:
- Company policies
- Standard Operating Procedures (SOPs)
- Training materials
- Employee manuals
- Internal guides
- Operational documentation
As the document library grows, employees can spend significant time searching through folders and files to find specific information.
Duplicate documents can also reduce retrieval quality and create unnecessary storage and processing costs.
The goal of this project was to build an automated knowledge management system that organizes company documents, prevents duplicate indexing, and allows employees to retrieve information using natural language.

---

## 🎯 Objectives
The system was designed to:
- Automate document ingestion from Google Drive
- Extract document content automatically
- Prevent duplicate document indexing
- Generate AI-powered document metadata
- Generate document summaries and keywords
- Organize documents into searchable categories
- Store document records in Airtable
- Index unique documents into Pinecone
- Retrieve relevant information using semantic search
- Provide conversational access to company knowledge
- Maintain conversation history
- Return source citations with AI responses

---

## 💡 Solution
The system consists of two interconnected workflows.

### 1. Document Ingestion Workflow
Whenever a new document is uploaded to Google Drive, the workflow:
1. Detects the new document.
2. Downloads the file.
3. Extracts its text content.
4. Sends the content to Google Gemini for metadata extraction.
5. Parses the structured AI response.
6. Generates a SHA-256 hash for duplicate detection.
7. Searches Airtable for an existing document with the same hash.
8. Stops processing if the document already exists.
9. Uploads unique documents into Pinecone.
10. Creates an Airtable record containing the document metadata.
This creates an automated pipeline from **document upload → processing → classification → deduplication → vector indexing**.

### 2. AI Chat Workflow
Users interact with the knowledge base through an AI chat interface.
The chat workflow:
1. Receives the user's question.
2. Maintains conversation context using Simple Memory.
3. Searches Pinecone for relevant document information.
4. Provides retrieved context to Gemini.
5. Generates a contextual response.
6. Returns source citations so users can identify the documents used to generate the answer.

---

## 🏗️ Workflow Architecture
### Document Ingestion
```
Google Drive
     ↓
New Document
     ↓
Download File
     ↓
Extract Text
     ↓
Google Gemini
     ↓
AI Metadata Extraction
     ↓
Parse Structured JSON
     ↓
Generate SHA-256 Hash
     ↓
Search Airtable
     ↓
Duplicate?
   ↙       ↘
 YES       NO
  ↓         ↓
 END    Upload to Pinecone
              ↓
       Create Airtable Record
```

AI Chat / RAG
```
User
 ↓
Chat Interface
 ↓
AI Agent
 ↓
Simple Memory
 ↓
Pinecone Retrieval
 ↓
Relevant Document Context
 ↓
Google Gemini
 ↓
Contextual AI Response
 ↓
Source Citations
 ↓
User
```
Complete System
```
                    DOCUMENT INGESTION
                           │
                           ▼
                    Google Drive
                           │
                           ▼
                    Extract Content
                           │
                           ▼
                    Google Gemini
                 Metadata Extraction
                           │
                           ▼
                     SHA-256 Hash
                           │
                           ▼
                      Airtable
                  Duplicate Check
                     │         │
                  Exists     Unique
                     │         │
                     ▼         ▼
                    END     Pinecone
                              │
                              ▼
                          Airtable
                       Document Record
                     AI KNOWLEDGE CHAT
                           │
                           ▼
                          User
                           │
                           ▼
                       AI Agent
                           │
                    ┌──────┴──────┐
                    ▼             ▼
              Simple Memory   Pinecone
                                  │
                                  ▼
                         Relevant Documents
                                  │
                         ┌────────┴────────┐
                         ▼                 ▼
                      Context           Metadata
                         │                 │
                         └────────┬────────┘
                                  ▼
                            Google Gemini
                                  │
                                  ▼
                         AI Response
                         + Citations
                                  │
                                  ▼
                                User
```

⸻

🛠️ Technologies Used

Technology	Purpose
n8n	Workflow automation and orchestration
Google Drive	Document storage and ingestion trigger
Google Gemini	Metadata generation and AI response generation
Pinecone	Vector database and semantic retrieval
Airtable	Document records, metadata, and duplicate tracking
Simple Memory	Conversation history and contextual interactions
SHA-256	Document fingerprinting and duplicate detection

⸻

🧠 Core Features

📂 Automated Document Ingestion

New documents uploaded to Google Drive automatically enter the processing pipeline without requiring manual indexing.

🤖 AI Metadata Extraction

Google Gemini analyzes document content and generates structured metadata, summaries, keywords, and classification information.

🔐 SHA-256 Duplicate Detection

Instead of relying on filenames, the system generates a SHA-256 hash from document content to identify duplicate files.

This allows the system to detect duplicates even when the same document has been renamed.

🗃️ Airtable Document Registry

Airtable stores document records and associated metadata, providing administrators with visibility into the knowledge base.

🧠 Pinecone Vector Search

Unique documents are indexed into Pinecone, allowing the AI assistant to retrieve semantically relevant information rather than relying only on exact keyword matches.

💬 Conversational AI Assistant

Users can ask questions naturally instead of manually searching through company documents.

🧾 Source Citations

AI responses include source information, allowing users to identify where the retrieved information came from.

🏷️ Metadata Filtering

Structured metadata can be used to improve retrieval accuracy and restrict searches to relevant document categories.

🧠 Conversation Memory

Simple Memory maintains conversational context, allowing users to ask follow-up questions without repeating the entire context.

⸻

🧩 Challenges Encountered

- Preventing duplicate document indexing even when filenames change
- Preserving document integrity throughout the processing pipeline
- Parsing structured AI responses into reusable workflow variables
- Generating consistent metadata across different document types
- Organizing documents for accurate retrieval
- Improving retrieval accuracy using structured metadata
- Maintaining useful conversational context during multi-turn interactions

⸻

🔧 Improvements Made

- Implemented SHA-256 duplicate detection
- Added AI-generated document metadata
- Added automatic document summaries
- Added keyword extraction
- Implemented metadata filtering
- Added source citations to AI responses
- Improved retrieval accuracy through structured document classification
- Added conversational memory
- Separated document ingestion from the AI chat workflow

⸻

📊 Results

The completed system can:

- Automatically detect new documents
- Extract document content
- Generate structured metadata
- Detect duplicate documents
- Prevent duplicate vector indexing
- Store document records in Airtable
- Index unique documents into Pinecone
- Retrieve relevant information using semantic search
- Answer questions using company documents
- Maintain conversational context
- Provide source citations

The result is an automated company knowledge base that transforms scattered internal documents into a searchable conversational knowledge system.

⸻

📸 Screenshots

Screenshots demonstrating the document ingestion workflow, metadata extraction, Airtable document registry, Pinecone index, and AI chat interface will be added here.

⸻

🎥 Demo

A full walkthrough demonstrating document ingestion, duplicate detection, metadata generation, Pinecone indexing, and conversational retrieval will be linked here.

Demo: Coming soon

⸻

🔮 Future Improvements

- Interactive knowledge-base analytics dashboard
- Document version control
- Department-based access control
- Retrieval confidence scoring
- Voice-enabled AI assistant
- Slack integration
- Microsoft Teams integration
- WhatsApp integration
- Automated document expiry and review reminders

⸻

📚 Key Learnings

- Retrieval-Augmented Generation (RAG)
- Vector database architecture
- Semantic search
- Pinecone integration
- AI metadata extraction
- Structured AI output parsing
- SHA-256 hashing
- Duplicate detection
- Metadata filtering
- Conversational memory
- Source citation design
- Multi-workflow automation
- Knowledge-base architecture

⸻

👤 Author

Okanlawon Abdulhammed

AI & Automation Builder

[LinkedIn⁠](https://www.linkedin.com/in/okanlawon-a-o/)
