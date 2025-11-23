flowchart TD

%% =======================================
%% 1. USER QUERY → QUERY ANALYZER
%% =======================================
A[User Query] --> B[Query Analyzer]

%% =======================================
%% 2. MEMORY LOOKUP
%% =======================================
B --> C{Is Answer Already in Memory?}

%% =======================================
%% 3. IF ANSWER IS IN MEMORY
%% =======================================
C -->|Yes| D[Memory Result]

D --> E[LLM]

E --> F[LLM Evaluator]

F -->|High Score| Z[FINAL ANSWER]

F -->|Low Score| R[Re-Retrieve with Higher K Value]

%% =======================================
%% 4. RE-RETRIEVAL LOOP
%% =======================================
R --> S[LLM]
S --> T[LLM Evaluator]

T -->|High Score| Z
T -->|Low Score| R

%% =======================================
%% 5. IF NOT IN MEMORY → STANDARD RETRIEVAL
%% =======================================
C -->|No| H[Retrieve Relevant Chunks from Vector Database]

H --> I[LLM]
I --> J[LLM Evaluator]

J -->|High Score| K[Save Answer to Memory]
K --> Z

J -->|Low Score| R

%% =======================================
%% 7. STATIC EMBEDDING PIPELINE
%% =======================================
subgraph STATIC_EMBEDDINGS[STATIC EMBEDDING PIPELINE]
direction TB
X1[Documents → Cleaning → Chunking]
X2[Embedding Model]
X3[Store Embeddings in Vector Database]
X1 --> X2 --> X3
end
