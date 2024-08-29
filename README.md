README
Overview
This project implements a Retrieval-Augmented Generation (RAG) system using NVIDIA's AI models to perform contextual query answering. The code loads a local PDF document, generates embeddings using NVIDIA's embedding models, creates an index, and allows querying the index with enhanced context using HyDE transformation. The system leverages LlamaParse for document parsing, NVIDIA models for embedding and re-ranking, and integrates with the Llama-3.1 8B model for final response generation.

Key Features
Document Parsing: Utilizes LlamaParse to convert local PDF files into text format.
Embedding Generation: Uses NVIDIAEmbeddings to generate vector embeddings for document chunks, enabling efficient similarity searches.
Index Creation and Persistence: Constructs a VectorStoreIndex to store and retrieve document embeddings and persists the index for future use.
Query Decomposition and Transformation: Uses HyDEQueryTransform to enhance the relevance of queries by creating hypothetical contexts, improving the quality of retrieval.
Contextualized Retrieval and Re-ranking: Retrieves relevant documents based on the HyDE-transformed query and uses NVIDIARerank to rank the retrieved documents based on relevance.
Final Response Generation: The most relevant documents are used as context for generating a final response using NVIDIA's Llama-3.1 8B language model.
How It Works
Document Parsing: The local PDF document (PhilDataset.pdf) is parsed using LlamaParse, converting it into a format suitable for text processing.

Text Splitting and Embedding Generation: The parsed text is split into smaller chunks (tokens). Each chunk is then embedded using the NVIDIAEmbeddings model, converting the text into a vector representation.

Index Creation: The embeddings and corresponding text chunks are used to create a VectorStoreIndex. This index is stored and can be loaded later for querying.

HyDE Transformation for Query Enhancement:

When a query is submitted, the HyDEQueryTransform creates a hypothetical document that might answer the query. This transformed query includes the original query plus additional generated context.
The purpose of this transformation is to enhance the contextual relevance of the query. By broadening the query scope, HyDE helps retrieve more relevant information from the vector index.
Contextualized Retrieval: The enhanced query is then used to search the VectorStoreIndex for the most relevant documents. This ensures that the retrieved documents are highly context-specific and likely to contain useful information.

Re-ranking: The top-k retrieved documents are re-ranked using the NVIDIARerank model, which evaluates each document's relevance to the original question.

Response Generation: The most relevant document from the re-ranking process is used as context for generating a final response using the ChatNVIDIA client (Llama-3.1 8B model). The model takes the context and user query to generate a coherent, contextually aware answer.

Example Usage
To execute a query and receive a contextually relevant response, use the following function call:

python
question = "Which course did Philemon study in campus?"
response = query_model_with_context(question)
print("Final Response: ", response)

Dependencies
llama_index
langchain_nvidia_ai_endpoints
langchain_core
llama_parse
Install the required libraries using pip:

bash

pip install llama_index langchain_nvidia_ai_endpoints langchain_core llama_parse
Conclusion
This project demonstrates how advanced query transformations and contextualization using NVIDIA's AI tools can enhance the performance and accuracy of Retrieval-Augmented Generation systems, making them more effective for real-world applications where context-sensitive and accurate responses are critical.
