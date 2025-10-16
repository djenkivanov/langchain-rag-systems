# LangChain RAG systems experimental notebook

This notebook can be seen as a playground for testing different Retrieval-Augmented Generation (RAG) methods. 

An overview of all RAG methods in the notebook + explanation.

## Multi-Query
Broaden the user question's reach by rephrasing their question in multiple different ways. Really helpful when the user's question is ambigous and ill-worded.

![](/img/multi_query.png)

## Reciprocal Rank Fusion (RRF)
Often paired with Multi-Query, using the different questions generated, retrieve documents from the vectorstore for each question, then rank all the documents retrieved by relevance (how often and in what order they appear for each question) with the formula `1 / (rank + k)`, `rank` being the position it's in and `k` being a constant to flatten the differences and get better comparison.

![](/img/rrf.png)

## Decomposition
Take the user question, create different sub-questions, answer those sub-questions, then use those answers to ask the initial user question with the question+answer pairs that were generated earlier.

![](/img/decomposition.png)

## Step-back
Opposite of Decomposition, instead of sub-question, ask a more abstract, broader, more general question, answer that, then use the step-back context as well as regular context to help answer the user's question.

![](/img/stepback.png)

## Routing
Dynamically route the user question to the most relevant data source by invoking the LLM. Can also be used to route the user question to the most relevant template.

![](/img/route_data_source.png)
![](/img/route_template.png)

## Query construction
Invoke the LLM to convert the user's question into a database query which can then be used for a more concise and precise lookup and retrieval.

![](/img/query_construction.png)

## Multi-representation indexing
We create 2 different data stores, a vectorstore and docstore. The docstore will just be a store for all our document splits, unaltered. The vectorstore will contain summaries of each document split in the docstore, so that retrieval will be easier. We invoke the LLM to create a summary of a document split, we then embed that summary in the vectorstore, we also embed the user question, and then using the embedded summary and user question, we can retrieve the most relevant full document from the docstore.