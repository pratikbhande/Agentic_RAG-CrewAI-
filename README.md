# README: RAG Workflow Implementation for Research Paper Queries

## Overview
This repository provides an implementation of a Retrieval-Augmented Generation (RAG) workflow using LangChain, CrewAI tools, and OpenAI's GPT models. The code enables users to efficiently retrieve and process information from both vectorstores (PDF documents) and web searches. By orchestrating multiple agents and tasks, the workflow ensures accurate, relevant, and hallucination-free answers to user queries.

## Features
- **Routing**: Routes user questions to either a vectorstore (for research paper-based queries) or web search.
- **Retriever**: Fetches context-relevant information from the vectorstore or web search.
- **Grading**: Evaluates the relevance and factual grounding of retrieved content.
- **Hallucination Filtering**: Ensures the generated responses are grounded in facts.
- **Final Answer Generation**: Provides a concise and relevant answer to the user’s question.

---

## Prerequisites
1. Python 3.8 or later.
2. Google Colab or a local Python environment.
3. Necessary API keys:
   - OpenAI API Key.
   - Tavily API Key (for web search).

### Libraries Required
- `langchain_openai`
- `crewai_tools`
- `google.colab`
- `openai`
- `langchain_community`

Install missing libraries as needed using pip.

---

## Environment Setup
1. Add your **OpenAI API Key** and **Tavily API Key** to Google Colab or as environment variables:
    ```python
    os.environ['OPENAI_API_KEY'] = userdata.get('OPENAI_API_KEY')
    os.environ['TAVILY_API_KEY'] = userdata.get('TAVILY_KEY')
    ```

2. Upload the target PDF document (e.g., `ijeer-100426.pdf`) to your environment if you wish to use the vectorstore.

---

## Code Structure

### Agents
1. **Router Agent**:
   - Routes the user’s question to the appropriate retrieval tool (vectorstore or web search).
   - Uses keywords in the question to decide the routing path.

2. **Retriever Agent**:
   - Fetches the necessary information from the vectorstore (PDF) or performs a web search.

3. **Grader Agent**:
   - Evaluates the relevance of the retrieved content with respect to the user’s query.

4. **Hallucination Grader**:
   - Ensures that the response is grounded in factual content and free from hallucination.

5. **Answer Grader**:
   - Produces a clear and concise answer or triggers a web search for missing context if the previous answers were irrelevant.

### Tasks
1. **Router Task**:
   - Decides whether the query should be handled by the vectorstore or web search.

2. **Retriever Task**:
   - Retrieves relevant content based on the decision from the Router Task.

3. **Grader Task**:
   - Scores the relevance of the retrieved content (binary: `yes` or `no`).

4. **Hallucination Task**:
   - Ensures the generated answer is factually correct (binary: `yes` or `no`).

5. **Answer Task**:
   - Provides the final response or performs a fallback web search if necessary.

### Tools
1. **PDFSearchTool**:
   - Searches for relevant information in a PDF document.
2. **TavilySearchResults**:
   - Fetches top web search results for a query.

### Crew
- The `Crew` combines all agents and tasks into a cohesive workflow, handling input and output seamlessly.

---

## Workflow
1. **Input**: User inputs a question (e.g., "Explain methodology of research paper?").
2. **Routing**:
   - The Router Task analyzes the query and routes it to either the vectorstore or web search tool.
3. **Retrieval**:
   - The Retriever Task fetches information from the respective source.
4. **Grading**:
   - The Grader Task evaluates whether the retrieved content is relevant.
   - The Hallucination Task ensures factual accuracy.
5. **Final Answer**:
   - The Answer Task provides a concise, relevant response or performs a web search if required.

---

## Example Execution
### Input
```python
inputs = {"question": "Explain methodology of research paper?"}
result = rag_crew.kickoff(inputs=inputs)
```

### Output
- A concise and relevant answer derived from the PDF or web search.
- If no relevant answer is found, the workflow falls back to web search for additional information.

---

## How to Use
1. **Run the Code**:
   - Ensure all required libraries are installed and API keys are set.
   - Upload the PDF document if using vectorstore.
2. **Provide Input**:
   - Modify the `inputs` dictionary with your query.
3. **Get Results**:
   - Use `rag_crew.kickoff(inputs=inputs)` to initiate the workflow.

---

## Customization
1. **Model Configuration**:
   - Change the LLM model in `ChatOpenAI` to a different OpenAI model if required:
     ```python
     llm = ChatOpenAI(model="gpt-4", temperature=0.1)
     ```
2. **PDF Embedding Model**:
   - Use a different embedding model in the vectorstore configuration:
     ```python
     provider="huggingface",
     config=dict(model="other-model-name")
     ```
3. **Additional Tasks**:
   - Extend the workflow by adding more tasks or agents as needed.

---

## Future Enhancements
- Add support for summarizing multiple PDFs.
- Integrate with additional search APIs for broader web coverage.
- Improve hallucination detection with stricter grading criteria.

---

## Credits
- **OpenAI**: For GPT-4 models.
- **LangChain**: For building LLM-powered workflows.
- **CrewAI Tools**: For providing agent and task management.
- **Tavily API**: For robust web search functionality.


