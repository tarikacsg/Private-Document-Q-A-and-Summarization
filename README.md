# Private-Document-Q-A-and-Summarization
Overview

This notebook demonstrates how to build a retrieval‑augmented question answering (QA) and summarization system over private documents using the LangChain framework, IBM watsonx.ai large language models (LLMs) and a Chroma vector store. The workflow covers loading and preprocessing a text document, indexing it for efficient retrieval, constructing an LLM pipeline for answering questions, and extending the system with prompting strategies and conversational memory.

Key Steps

1. Install Dependencies:
The notebook installs specific versions of ibm-watsonx-ai, langchain, langchain-ibm, huggingface, sentence-transformers, chromadb, and wget via pip to ensure reproducible behavior.

2. Import Libraries and Configure Warnings:
It suppresses Python warnings and imports components from LangChain (for loading, splitting, embedding, retrieval, prompting and memory), along with IBM’s Model classes and the WatsonxLLM wrapper to access hosted LLMs.

3. Load and Inspect the Document:
A sample document (companyPolicies.txt) is downloaded via wget, then loaded into memory with TextLoader. Its contents can be printed to see the source text.

4. Split the Text into Chunks:
Using CharacterTextSplitter, the document is divided into smaller chunks (e.g., 1000 characters) with optional overlap. Chunking ensures each piece fits within an LLM’s context window and improves retrieval accuracy.

5. Create Embeddings and Vector Store:
A HuggingFaceEmbeddings model converts each text chunk into high‑dimensional vectors. Those vectors are stored in a Chroma database via Chroma.from_documents, enabling efficient similarity search.

6. Configure and Instantiate IBM watsonx.ai Models:
The notebook defines model IDs (initially Google’s FLAN‑UL2, later Meta’s LLAMA‑3‑70B-Instruct), decoding parameters (e.g., greedy decoding, max/min tokens, temperature), and authentication credentials. These settings are wrapped into IBM’s Model class and used to create a WatsonxLLM instance.

7. Build a Retrieval QA Chain:
A RetrievalQA chain combines the LLM with the vector store retriever. By invoking this chain with a question (e.g., “What is mobile policy?”), the system retrieves relevant chunks and asks the model to generate an answer.

8. Experiment with Different Models:
The code shows how to swap out the underlying LLM for a more capable model (LLAMA‑3‑70B) to improve summarization quality. It reuses the same retrieval pipeline with the new model.

9. Prompt Engineering:
A custom PromptTemplate is defined to instruct the model to use the retrieved context and to avoid fabricating answers (e.g., telling the model to respond with “I don’t know” when appropriate). This template is passed to the retrieval chain to control the LLM’s behavior.

10. Conversational Memory:
To enable multi‑turn dialogue, the notebook sets up a ConversationBufferMemory and uses a ConversationalRetrievalChain. It maintains a history list of previous question–answer pairs, allowing follow‑up questions (e.g., pronoun references) to be resolved correctly.

11. Interactive Agent Wrapper:
Finally, it outlines how to wrap the retrieval, prompting, and memory components into a reusable agent function that can repeatedly handle user queries in a conversational setting.
