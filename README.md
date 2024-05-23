# APAIA Document Query System

This project leverages the `llama_index` package and Hugging Face embeddings to create an efficient document query system. The system uses a large language model (LLM) to process and respond to queries about the APAIA leadership team based on the provided document context.

## Features

- **Environment Variable Management**: Uses `dotenv` to manage environment variables.
- **Document Loading**: Loads documents from a specified directory.
- **Embedding Model**: Utilizes Hugging Face's `intfloat/e5-large-v2` model for embeddings.
- **Large Language Model**: Configures the LLM with the `llama3` model and a custom request timeout.
- **Query Engine**: Creates an index from documents and handles search queries with a defined prompt template.
- **Error Handling**: Includes error handling for document loading and query execution.

## Prerequisites

- Python 3.7+
- Virtual environment (optional but recommended)

## Installation

1. **Clone the repository**:

    ```bash
    git clone https://github.com/alaeddine-hash/RAG_LLAMA_INDEX.git
    cd apaia-doc-query
    ```

2. **Create and activate a virtual environment** (optional but recommended):

    ```bash
    python -m venv venv
    source venv/bin/activate   # On Windows, use `venv\Scripts\activate`
    ```

3. **Install required packages**:

    ```bash
    pip install -r requirements.txt
    ```

4. **Create a `.env` file** and add your environment variables as needed.

## Usage

1. **Load environment variables**:

    ```python
    from dotenv import load_dotenv
    load_dotenv()
    ```

2. **Load documents from the specified directory**:

    ```python
    from llama_index.core import SimpleDirectoryReader

    try:
        documents = SimpleDirectoryReader("Data").load_data()
    except Exception as e:
        print(f"Failed to load documents: {e}")
    ```

3. **Set the embedding model**:

    ```python
    from llama_index.embeddings.huggingface import HuggingFaceEmbedding
    from llama_index.core import Settings

    Settings.embed_model = HuggingFaceEmbedding(model_name="intfloat/e5-large-v2")
    ```

4. **Configure the LLM**:

    ```python
    from llama_index.llms.ollama import Ollama

    Settings.llm = Ollama(model="llama3", request_timeout=360.0)
    ```

5. **Create an index and persist it**:

    ```python
    from llama_index.core import VectorStoreIndex

    index = VectorStoreIndex.from_documents(documents)
    index.storage_context.persist("naval_index")
    ```

6. **Define the prompt template and create a query engine**:

    ```python
    from llama_index.core import Prompt

    template = (
        "Context information is provided below.\n"
        "-------------------\n"
        "{context_str}\n"
        "-------------------\n"
        "Based on this information, as an APHAIA sales representative, please answer the following question in Japanese text only: {query_str}\n"
    )

    qa_template = Prompt(template)
    query_engine = index.as_query_engine(text_qa_template=qa_template)
    ```

7. **Execute a query**:

    ```python
    query = "Could you provide a quick overview of APAIA leadership team?"

    try:
        response = query_engine.query(query)
        print(response)
    except Exception as e:
        print(f"Error during query execution: {e}")
    ```

## Error Handling

The system includes basic error handling for document loading and query execution to ensure robust performance.

## Contributing

Feel free to submit issues or pull requests to contribute to the project.

## License

This project is licensed under the MIT License.
