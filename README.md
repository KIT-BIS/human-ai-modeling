# Human-AI Process Modeling

This project explores different approaches using Large Language Models (LLMs) to support the generatation of business process models in the form of Petri nets from natural language process descriptions. The generated models are intended for usage with tools like the [Horus Business Modeler](https://www.horus.biz/de/produkte/business-modeler/).

## Research Paper

This repository contains the code and resources associated with the research paper:

**Title:** Human-AI Collaboration for Business Process Modeling with Petri Nets

> **Abstract:** Business Process Modeling enables organizations to document and improve workflows, but creating process models remains a time-intensive task requiring expertise in formal modeling languages and domain knowledge. Recent advances in Large Language Models (LLMs) have facilitated the automated generation of process models from textual descriptions, yet these LLMs often struggle with ambiguity, inconsistencies, and validation challenges. This work introduces a human-in-the-loop (HITL) approach where an LLM generates clarifying questions to resolve ambiguities, enabling human-AI collaboration through iterative refinements and structured guidance for more accurate process models. Evaluation results demonstrate that the HITL approach resolves ambiguities and improves the syntactic and semantic quality of generated process models. By bridging the gap between automation and human expertise, this approach contributes to the development of reliable and effective methodologies for Business Process Modeling.

## Environment Setup

1.  **Clone the repository:**
    ```bash
    git git@github.com:KIT-BIS/human-ai-modeling.git
    cd human-ai-modeling
    ```
2.  **Create a virtual environment (optional but recommended):**
    ```bash
    python -m venv .venv
    source .venv/bin/activate # On Windows use `.venv\Scripts\activate`
    ```
3.  **Install dependencies:** (Assuming a `requirements.txt` file exists or based on imports in notebooks)
    ```bash
    pip install -r requirements.txt
    ```
    Key dependencies seen in the notebooks include:
    *   `langchain`, `langchain-openai`
    *   `pydantic`
    *   `langgraph`
    *   `python-dotenv`
    *   `datasets` (from Hugging Face)
    *   `openai`
4.  **Configure Environment Variables:**
    Create a `.env` file in the root directory and add your API keys and other configurations:
    ```dotenv
    # .env file
    OPENAI_API_KEY="your-openai-api-key"
    LANGCHAIN_API_KEY="your-langchain-api-key"
    LANGCHAIN_TRACING_V2="true"
    LANGCHAIN_ENDPOINT="https://api.smith.langchain.com"
    LANGCHAIN_PROJECT="your-langchain-project-name" # Optional: Organize runs in LangSmith
    HUGGINGFACE_REPO_ID="your-huggingface-repo-id" # If using a private dataset
    # OPENAI_API_BASE="your-local-ai-endpoint" # Optional: If using LocalAI or similar
    ```
    The notebooks load these variables using `python-dotenv`.

## Usage

1.  Ensure you have completed the steps in the **Environment Setup** section, including activating the virtual environment and setting up the `.env` file with your API keys.
2.  Navigate to the project's root directory in your terminal.
3.  Open the project folder in Visual Studio Code:
    ```bash
    code .
    ```
4.  Navigate to the `notebooks` directory within VS Code's Explorer panel. Open one of the notebooks (`baseline-methode.ipynb`, `v1-with-hitl.ipynb`, or `v2-no-HITL.ipynb`).
5.  Ensure VS Code recognizes your Python environment (created in step 2 of Environment Setup) and has the necessary Jupyter extension installed.
6.  Run the cells sequentially to execute the different process modeling workflows. Modify the input process descriptions or parameters as needed for experimentation.

## Notebooks

The [`notebooks`](/notebooks/) directory contains Jupyter notebooks exploring different modeling strategies:

1.  **[`baseline-methode.ipynb`](/notebooks/baseline-methode.ipynb):** Implements a baseline approach where the LLM directly attempts to generate a Petri net (Places, Transitions, Edges) from a given process description. It includes prompts defining the expected output format and rules for valid Petri nets.
2.  **[`v1-with-hitl.ipynb`](/notebooks/v1-with-hitl.ipynb):** Explores a Human-in-the-Loop (HITL) approach. The workflow involves:
    *   Generating clarifying questions about the process description.
    *   (Implicitly) Requiring human answers to these questions.
    *   Incorporating human answers to enhance the description.
    *   Structuring the enhanced description.
    *   Modeling the structured description into a Petri net.
    *   Critiquing the generated model for errors.
    *   Improving the model based on the critique.
    *   Parsing the final model into a structured JSON format.
3.  **[`v2-no-HITL.ipynb`](/notebooks/v2-no-HITL.ipynb):** Implements a fully automated approach without direct human intervention during the modeling phase. The workflow involves:
    *   Structuring the initial process description into predefined sections (Start Point, Sequence, Decisions, Parallel, Loops, Endpoints).
    *   Modeling each section individually using specific prompts.
    *   Integrating the individual section models sequentially into a single, cohesive Petri net using integration prompts tailored to section types.
    *   Critiquing the final integrated model.
    *   Improving the model based on the critique.
    *   Parsing the final model into a structured JSON format.

Each notebook utilizes LangChain and LangGraph to define and execute these multi-step workflows with an LLM (like GPT-4o or potentially open-source models via LocalAI). They also define Pydantic models for structuring the final Petri net output in JSON.