
# Local Multi-Agent RAG with LangGraph & LMStudio 🤖💸

This repository implements a sophisticated, privacy-first **Multi-Agent Retrieval-Augmented Generation (RAG)** system designed to automate the analysis of equity research reports.

By leveraging **LangGraph** for orchestration and **LMStudio** for local LLM inference, this project demonstrates how to build complex, cost-effective AI workflows without relying on external APIs.

## 🚀 Key Features

  * **Local Inference:** Uses **LMStudio** to serve open-source models (e.g., Gemma-2-9b) locally, ensuring complete data privacy and zero API costs.
  * **Multi-Agent Architecture:** Decomposes complex financial analysis into specialized tasks handled by autonomous agents.
  * **Dynamic Conditional Routing:** Implements "human-in-the-loop" style logic where specific analysis paths (e.g., Sentiment Analysis) are triggered only when requested.
  * **Advanced Retrieval:** Uses a custom retrieval strategy that combines company name variations and acronyms to improve search recall.
  * **Structured Output:** Aggregates findings into a structured dictionary containing extraction statistics and classification results.

## 🧩 Architecture & Workflow

The system is built as a state graph where data flows through a specific sequence of agents:

1.  **Retrieval Agent:** Fetches relevant documents from a persistent **ChromaDB** vector store using generated name variations (e.g., "Star Cement" -\> "Star", "SC").
2.  **Sentence Extraction Agent:** Cleaning and filtering the raw text to extract only sentences relevant to financial performance and recommendations.
3.  **Conditional Fork:** The workflow checks the `agents_to_call` input list to decide which downstream agents to activate.
      * **Sentiment Detection Agent:** Classifies sentences as *Promotional*, *Critical*, or *Neutral*.
      * **Forward Looking Agent:** Identifies speculative statements about future performance (*Forward Looking* vs. *Not Forward Looking*).
4.  **Aggregation Agent:** Acts as a synchronization point, collecting outputs from all active agents and compiling the final report.

## 🛠️ Prerequisites

  * **Python 3.10+**
  * **LMStudio:** Installed and running locally.
      * **Model:** Any competent chat model (tested with `gemma-2-9b`).
      * **Server:** Must be running on `http://127.0.0.1:1234/v1`.
  * **ChromaDB:** A populated vector store with financial reports (the code expects a persistent directory).

## 📦 Installation

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/yourusername/local-multi-agent-rag.git
    cd local-multi-agent-rag
    ```

2.  **Install dependencies:**

    ```bash
    pip install langchain langchain-openai langgraph chromadb pandas numpy torch
    ```

3.  **Configure Paths:**
    Update the `vectorstore_directory` variable in the notebook/script to point to your local ChromaDB instance.

## 💻 Usage

1.  **Start LMStudio:** Load your model and start the local inference server.
2.  **Run the Notebook:** Execute the Jupyter Notebook.
3.  **Define Inputs:** You can control the workflow by modifying the `agents_to_call` list in the input dictionary.

<!-- end list -->

```python
# Example Input
inputs = {
    "report_vector_data": all_items,
    "company_name": "Star Cement",
    "agents_to_call": ["sentiment_detection_agent", "forward_looking_agent"]
}

# Run the Graph
for output in graph.stream(inputs):
    # ... process stream
```

## 📊 Sample Output

The final output is a dictionary summarizing the analysis:

```python
{
 'Provided Company Name': 'Star Cement',
 'Company Name Variations': ['Star', 'Star Cement'],
 'agents_to_call': ['sentiment_detection_agent', 'forward_looking_agent'],
 'Total Number of Sentences about the provided company': 60,
 'Total Number of Promotional/Critical Sentences': 3,
 'Total Number of Forward Looking Sentences': 28
}
```

## 🤝 Contributing

Contributions are welcome\! Please feel free to submit a Pull Request.
