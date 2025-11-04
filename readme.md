# ShikshaGPT-AI Chat Bot

## Overview

Welcome to the documentation of the customized chat bot developed for science-related questions, also known as the science bot. This document outlines the journey and key components of the project, from requirements to implementation. The goal of this project is to enable the bot to handle scientific questions intelligently and provide the best possible answers.

## Directory Structure

The project structure is organized as follows:

* **main.py:** The main application file handling the Flask server, request processing, and response generation.
* **simple_text.py:** Defines the `SimpleText` class and its schema using Marshmallow.
* **ignite.py:** Launches the Flask server.
* **datastore/:** Contains state information (`state.json`) and individual query data (`query_id.json`) in a structured format.
* **vectorstore/:** Contains the generated embeddings of the texts in vector format.
* **preprocessing.py:** Added to handle document loading and text splitting, contributing to overall text preprocessing.
* **test.py:** Contains the logic behind the large language model in which input queries are processed to obtain the desired results.

## Implementation Steps

### 1. Understanding the Test

The initial step involved thoroughly understanding the provided test description, including its mission, ground rules, and deployment options. I first reviewed the Openfabric documentation ([Openfabric Docs](https://docs.openfabric.ai/developer-tools/index/)) to understand the underlying data structures, classes, and functions within the project directory.

### 2. Framework Selection

The project utilizes the Openfabric PySDK for interacting with the Openfabric platform and handling the execution context.

### 3. Text Preprocessing

I introduced a new file, `preprocessing.py`, to handle text preprocessing. In this file, I leveraged LangChain:  

1. Load data stored in the `data/` folder in PDF format using `PyPDFLoader` and `DirectoryLoader`.  
2. Split the text into chunks using `RecursiveCharacterTextSplitter`.  
3. Generate embeddings for the text data using the sentence-transformer model from the `HuggingFaceEmbeddings` dependency.  
4. Store the embeddings in vector format using **FAISS**.  

**Note:** To generate these vector stores before executing the main script, run `preprocessing.py`.

**Code snippet:**  
![Preprocessing Code](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/74cbe6d0-15af-4718-a2a0-c99e4fc81d6f)  

**Output:**  
![Vector Store Output](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/df8b240b-5761-4d1f-aaa8-1ffcf4f8567b)

### 4. Coding the `execute` Function

The core of the application is the `execute` function in `main.py`. This function processes incoming requests, handles each query, and generates appropriate responses.  

**Code snippet (handle_request):**  
![Handle Request](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/240395eb-bfef-4875-8c8b-e2e45df74e88)  

**Generating session and query IDs:**  
![Generate IDs](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/afe1d175-c87e-47a7-90c0-e6146323c38c)

**Execute function processing queries:**  
![Execute Function](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/f809fcf0-6b02-48c6-8ccf-5c6d3f166daa)

**Datastore update and final output:**  
![Datastore and Output](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/8f9a3590-45ba-4de5-9ab9-13887ab72859)

### 5. Integration with Custom Model (`test.py`)

The `test.py` file contains the implementation of the custom chat bot model. It leverages language models for question answering, embeddings, and a vector store for efficient retrieval.

**Step 1 – Custom Prompt Template:**  
![Prompt Template](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/9a7aac3b-d54b-4646-a003-633ab7005160)

**Step 2 – Retrieval QA Chain:**  
![Retrieval QA Chain](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/3fb28224-367f-4bde-96a5-2fd2b46ef183)

**Step 3 – Loading the Model:**  
![Load Model](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/2c557752-3f01-4e56-a36d-4b660672f52c)

**Defining QA function:**  
![QA Function](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/adf9d4db-4f36-4c0a-bf9a-cb5adfddf79e)

**Combining everything for final output:**  
![Final Output](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/bb44c599-3659-4d15-a5aa-38aa638c07f7)

### 6. Error Handling

Error handling is implemented within the `execute` function to manage issues gracefully. Errors are stored in the `messages` attribute of the Ray class.  
![Error Handling](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/d7c5e3f5-f230-492d-baa9-f569190842e3)

### 7. State Management

The `state.json` file in `datastore/` tracks the status of each query, including queued, requested, and completed queries.

### 8. Output Storage

The output of each query, along with relevant information, is stored in a separate file (`query_id.json`) within the `datastore/queries/` directory.

### 9. Output

Using Postman, multiple queries can be submitted in JSON format, and the bot returns structured responses.  
![Postman Output](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/e53b909d-8e86-4956-b2f0-33fba37e553c)

### 10. UI Interface

A custom user interface was developed using **Chainlit**. To run the UI, execute:  

```bash
chainlit run test.py -w
```

This provides an interactive chat interface for users to interact with the bot.  
![UI Screenshot 1](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/609d417b-4687-410c-a4d6-6c5f3ae44925)  
![UI Screenshot 2](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/e2ae329d-1f91-45c9-a084-1c109c963100)  
![UI Screenshot 3](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/b48922f5-1721-4207-964f-408de95e625d)  
![UI Screenshot 4](https://github.com/prajwalnayak17/Shiksha_GPT/assets/87718913/fd68b6b3-a036-4dcc-a24b-42c49ab64a3c)

---

