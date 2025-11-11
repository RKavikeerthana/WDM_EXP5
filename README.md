### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 02.10.2025
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```
import numpy as np

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.documents_matrix = None
        self.terms = []
        self.all_doc_ids = set()

    def index_document(self, doc_id, text):
        self.all_doc_ids.add(doc_id)
        terms = text.lower().split()
        print(f"Document - {doc_id} {terms}")  # Show tokenized document
        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        self.terms = sorted(list(self.index.keys()))
        num_docs = len(documents)
        num_terms = len(self.terms)
        sorted_doc_ids = sorted(list(documents.keys()))
        self.documents_matrix = np.zeros((num_docs, num_terms), dtype=int)

        for i, doc_id in enumerate(sorted_doc_ids):
            doc_terms = documents[doc_id].lower().split()
            for term in doc_terms:
                term_id = self.terms.index(term)
                self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self, documents):
        print("\n--- Document-Term Matrix ---")
        header = ["Doc"] + self.terms
        print("{:<6}".format(header[0]), end="")
        for h in header[1:]:
            print("{:<12}".format(h), end="")
        print()
        sorted_doc_ids = sorted(list(documents.keys()))
        for i, doc_id in enumerate(sorted_doc_ids):
            print("{:<6}".format(doc_id), end="")
            for val in self.documents_matrix[i]:
                print("{:<12}".format(val), end="")
            print()

    def print_all_terms(self):
        print("\nAll terms in the documents:")
        print(self.terms)

    def boolean_search(self, query):
        parts = query.lower().split()
        if len(parts) == 1:
            return self.index.get(parts[0], set())
        if len(parts) == 2 and parts[0] == 'not':
            term = parts[1]

              return self.all_doc_ids - self.index.get(term, set())
        if len(parts) != 3:
            return "Invalid query. Use 'term1 operator term2'."
        term1, operator, term2 = parts[0], parts[1], parts[2]
        set1 = self.index.get(term1, set())
        set2 = self.index.get(term2, set())
        if operator == 'and':
            return set1.intersection(set2)
        elif operator == 'or':
            return set1.union(set2)
        elif operator == 'not':
            return set1.difference(set2)
        else:
            return "Operator not supported. Use 'and', 'or', 'not'."

if __name__ == "__main__":
    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }
 # Index all documents
    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    # Create document-term matrix
    indexer.create_documents_matrix(documents)
    indexer.print_documents_matrix_table(documents)
    indexer.print_all_terms()

    # Boolean query input
    query = input("\nEnter your boolean query (e.g., 'information and retrieval', 'python or models', 'retrieval not boolean'): ")
    results = indexer.boolean_search(query)

    if results:
        print(f"\nResults for '{query}': Document IDs {results}")
    else:
        print("\nNo results found for the query.")

   
       
```

### Output:

<img width="1429" height="428" alt="image" src="https://github.com/user-attachments/assets/f3238509-c502-4e38-b154-28b6ded1e857" />


### Result:
Thus, the python program to implement Information Retrieval Using Boolean Model is executed successfully.


