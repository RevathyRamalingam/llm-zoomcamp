# RAG (Retrieval Augmented Generation)

RAG (Retrieval Augmented Generation) is technique used in LLMs to provide context to it which otherwise can hallucinate and provide answer from its training data.

---

# RAG consists of 3 steps

```text
        Data Sources
             │
             ▼
      1. INGESTION
             │
             ▼
       Vector Store
             │
             ▼
   2. SEARCH / RETRIEVAL
             │
             ▼
      Relevant Context
             │
             ▼
  3. AUGMENTED GENERATION
             │
             ▼
             LLM
             │
             ▼
        Accurate Answer
```

## 1. INGESTION

- Pull data from data sources like web, Git repo, database and convert documents into embeddings(vectors) and store embeddings(vectors) in vector store(vectorDB like chromaDB,quadrant).
- The way data is stored matters - convert data into chunks which makes it smarter, cheaper and faster to run.

---

## 2. SEARCH/RETRIEVAL

- Get answer for the query from the vectorstore and give that as context to LLMs.
- Search can be a simple text search/lexical search or vector search which is semantic search.

### Text Search

- Text search commonly uses BM25, a lexical ranking algorithm based on term frequency and inverse document frequency.

### Vector Search

- Uses cosine similarity,dot product of vectors converted from embedded words, needs an embedding model.
- In RAG pipeline, we may need simple text search or a semantic search for our query, the best practice would be use hybrid search that combines results of text and vector search and gives top N results.

```text
                 User Query
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
   Text Search              Vector Search
      (BM25)             (Cosine Similarity)
          └───────────┬───────────┘
                      ▼
               Hybrid Search
                      │
                 Top N Results
```

---

## 3. AUGMENTED GENERATION

- We send the context and actual query (augmented query supplied with context) to LLMs which are now context-aware and can now respond with answer that is precise and accurate.

```text
      User Query
           │
           ▼
   Retrieved Context
           │
           ▼
Augmented Query (Query + Context)
           │
           ▼
          LLM
           │
           ▼
   Precise and Accurate Answer
```

---

# WAYS TO MEASURE ACCURACY OF SEARCH RESULTS

## 1. HIT RATE

- Generate relevance score 1 if top N results from RAG contained our answer, else 0.

---

## 2. MRR (Mean Reciprocal Rank)

- Penalizes the system for burying the answer.
- If document is in #3, MRR = 1/3.
- MRR values range from 0 to 1, where "1" indicates that the first relevant item is always at the top.
- Higher MRR means better system performance.
- RR = 1/(Rank of first correct result)
- You get 1 for first place, 1/2 for second and 1/3 for third place and so on.
- MRR = average of RR that runs through all the queries.

```text
Rank 1 → RR = 1
Rank 2 → RR = 1/2
Rank 3 → RR = 1/3
Rank 4 → RR = 1/4
...
MRR = Average(RR)
```

---

# WHY MRR IS NEEEDED AND HITRATE ALONE IS NOT SUFFICIENT

- LLMs suffer from "Lost in the middle" problem where it can see first and last result better and middle order results are ignored.
- So the order of retrieval of result from RAG matters most to build effective pipeline.

```text
Retrieved Results

1️⃣ Relevant  ← High attention
2️⃣
3️⃣ Relevant  ← Lost in the middle
4️⃣
5️⃣ Relevant  ← High attention
```

---

# EVALUATION - online/offline

## offline

- Ingested document and its retrieval can be evaluated using metrics like HitRate and MRR on a labelled evaluation dataset.
