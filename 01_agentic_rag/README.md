RAG Retrieval Augmented Generation is technique used in AI to provide context to LLMs which otherwise can hallucinate and provide answer from its training data.
RAG consists of 3 steps
1.INGESTION - pull data from data sources like web, Git repo, database and convert them into vectors and store it in vector store.
The way data is stored matters - convert data into chunks which makes it smarter, cheaper and faster to run
2. SEARCH/RETRIEVAL - get answer for the query from the vectorstore and give that as context to LLMs
Search can be a simple text search/lexical search or vector search which is semantic search
Text Search is based on BM25 Best Matching words 
Vector search uses cosine similarity of vectors converted from embedded words, needs an embedder. In RAG pipeline, we may need simple text search or a semantic search for our query, the best practice would be use hybrid search that combines results of text and vector search and gives top N results 
3. AUGMENTED GENERATION - we send the context and actual query(augmented query supplied with context) to LLMs which are now context-aware and can now respond with answer that is precise and accurate

WAYS TO MEASURE ACCURACY OF SEARCH RESULTS
1.HIT RATE - generate relevance score 1 if top N results from RAG contained our answer,else 0
2.MRR mean reciprocal rank- penalizes the system for burying the answer. if document is in #3, MRR =1/3
MRR values range from 0 to 1, where "1" indicates that the first relevant item is always at the top.
Higher MRR means better system performance.
RR = 1/(Rank of first correct result)
you get 1 for first place, 1/2 for second and 1/3 for third place and so on
MRR = average of RR that runs through all the search results 

WHY MRR IS NEEEDED AND HITRATE ALONE IS NOT SUFFICIENT
LLMs suffer from "Lost in the middle" problem where it can see first and last result better and middle order results are ignored. So the order of retrieval of result from RAG matters most to build effective pipeline

EVALUATION - online/offline
offline - Ingested document and its retrieval can be evaluated using metrics like HitRate and MRR 
