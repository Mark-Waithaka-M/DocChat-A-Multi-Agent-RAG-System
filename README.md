DocChat a Multi-Agent RAG System

eager to see how generative AI, multi-agent systems, and RAG pipelines can be combined to tackle real-world information retrieval challenges

Have you ever struggled to extract precise information from long, complex documents? Whether it's a research paper, legal contract, technical report, or environmental study, finding the exact details you need can feel overwhelming.

That's where DocChat comes in—a multi-agent RAG tool designed to help you ask questions about your documents and receive fact-checked, hallucination-free answers.

Sure, you could use ChatGPT, DeepSeek or Gemini to accomplish this task, but when dealing with long documents containing multiple tables, images, and dense text, these models struggle with retrieval and are prone to hallucinations.

They often misinterpret tables, overlook key data hidden in footnotes, or even fabricate citations. The problem? These models lack document-aware reasoning and don't verify their responses against structured sources.

That's why DocChat takes a different approach. Instead of relying on a single LLM, it combines multiple AI agents, each with a specific role:

A Hybrid Retriever that intelligently combines BM25 keyword search and vector embeddings to retrieve the most relevant passages
A Research Agent that analyzes the retrieved content and generates an initial response
A Verification Agent that cross-checks the response against the original document to detect hallucinations and flag unsupported claims
A Self-Correction Mechanism that re-runs the research step if any contradictions or unsupported statements are found
This multi-step, verification-driven approach ensures that DocChat provides precise, document-grounded answers, even for complex and long-form documents that general-purpose chatbots struggle with. Whether you need to extract specific data points, summarize sections, compare multiple reports, or analyze tables, DocChat is built to help you navigate your documents with confidence.

Key Lessons
Build and deploy a multi-agent RAG (retrieval-augmented generation) system that intelligently retrieves, analyzes, and verifies information from complex documents
Integrate hybrid retrieval techniques using BM25 and vector search to improve document understanding and relevance
Implement a verification pipeline that detects hallucinations and ensures factual accuracy in AI-generated responses
Create an intuitive user interface with Gradio, making your document analysis tool accessible to anyone

What does the DocChat app do?
With DocChat, you can:

Upload and analyze long documents (PDFs, Word files, text reports) with ease
Ask questions about the content and get precise, source-backed answers
Extract specific details from structured documents, even those with tables, figures, and dense text
Avoid AI hallucinations—every response goes through a verification step to ensure factual correctness
Receive an alert when your question is irrelevant to the uploaded documents—so you know when the AI cannot confidently answer based on the provided sources
Retrieve accurate answers even when multiple documents are uploaded—DocChat intelligently finds the right document to reference

key libraries that power DocChat:

docling – Handles document processing, chunking, and structured data extraction
langgraph – Enables the multi-agent workflow, allowing different AI agents to collaborate efficiently
chromadb – Provides fast, efficient vector search, ensuring accurate document retrieval
langchain (RAG) – Facilitates retrieval-augmented generation, integrating retrieval strategies with LLM-based reasoning
gradio - Builds the user-friendly web interface
ibm_watsonx_ai - Interacts with WatsonX models


Understand why multi-agent RAG is used
A Naïve RAG (Retrieval-Augmented Generation) pipeline is often insufficient for handling long, structured documents due to several limitations:

Limited query understanding: Naïve RAG processes queries at a single level, failing to break down complex questions into multiple reasoning steps. This results in shallow or incomplete answers when dealing with multi-faceted queries.

No hallucination detection or error handling: Traditional RAG pipelines lack a verification step. This means that if a response contains hallucinated or incorrect information, there's no mechanism to detect, correct, or refine the output.

Inability to handle out-of-scope queries: Without a proper scope-checking mechanism, Naïve RAG may attempt to generate answers even when no relevant information exists, leading to misleading or fabricated responses.

Inefficient multi-document retrieval: When multiple documents are uploaded, a Naïve RAG system might retrieve irrelevant or suboptimal passages, failing to select the most relevant content dynamically.

To overcome these challenges, DocChat implements a multi-agent RAG research system, which introduces intelligent agents to enhance retrieval, reasoning, and verification.

How multi-agent RAG solves these issues
Scope checking & routing
A Scope-Checking Agent first determines whether the user's question is relevant to the uploaded documents. If the query is out of scope, DocChat explicitly informs the user instead of generating hallucinated responses.
Dynamic multi-step query processing
For complex queries, an Agent Workflow ensures that the question is broken into smaller sub-steps, retrieving the necessary information before synthesizing a complete response.
For example, if a question requires comparing two sections of a document, an agent-based approach recognizes this need, retrieves both parts separately, and constructs a comparative analysis in the final answer.
Hybrid retrieval for multi-document contexts
When multiple documents are uploaded, the Hybrid Retriever (BM25 + Vector Search) ensures that the most relevant document(s) are selected dynamically, improving accuracy over traditional retrieval pipelines.

Fact verification & self-correction
After an initial response is generated, a Verification Agent cross-checks the output against the retrieved documents.
If any contradictions or unsupported claims are found, the Self-Correction Mechanism refines the answer before presenting it to the user.
Shared global state for context awareness
The Agent Workflow maintains a shared state, allowing each step (retrieval, reasoning, verification) to reference previous interactions and refine responses dynamically.
This enables context-aware follow-up questions, ensuring that users can refine their queries without losing track of previous answers.

Project overview
Below is a breakdown of DocChat's workflow

1 - User query processing & relevance analysis
The system starts when a user submits a question about their uploaded document(s)
Before retrieving any data, DocChat first analyzes query relevance to determine if the question is within the scope of the uploaded content
2 - Routing & query categorization
The query is routed through an intelligent agent that decides whether the system can answer it using the document(s):
In scope: Proceed with document retrieval and response generation.
Not in scope: Inform the user that the question cannot be answered based on the provided documents, preventing hallucinations.
3 - Multi-agent research & document retrieval
If the query is relevant, DocChat retrieves relevant document sections from a hybrid search system:
Docling converts the document into a structured Markdown format for better chunking
LangChain splits the document into logical chunks based on headers and stores them in ChromaDB (a vector store)
The retrieval module searches for the most contextually relevant document chunks using BM25 and vector search
4 - Answer generation & verification loop
Conduct research:

The research agent generates an initial answer based on retrieved content
A sub-process starts where queries are dynamically generated for more precise retrieval
Verification process:

The verification agent cross-checks the generated response against the retrieved content
If the response is fully supported, the system finalizes and returns the answer
If verification fails (e.g., hallucinations, unsupported claims), the system re-runs the research step until a verifiable response is found
5 - Response finalization
After verification is complete, DocChat returns the final response to the user
The workflow ensures that each answer is sourced directly from the provided document(s), preventing fabrication or unreliable outputs