# Agentic AI Workflow for Automated Expense Claim Processing

## Project Overview

Designed and developed a **single-agent AI system** that automates restaurant expense claim processing by converting receipt images into policy-compliant reimbursement reports.

The project simulates a business scenario at Northbridge Analytics, a consulting firm processing approximately 600 meal receipts monthly. The existing manual review process is time-consuming and susceptible to errors, particularly when identifying non-reimbursable expenses and calculating taxes and tips on eligible purchases.

Using Large Language Models (LLMs), Retrieval-Augmented Generation (RAG), and agentic workflows, the solution automates the expense review process through specialized tools coordinated by an intelligent agent.

## Business Problem

Manual expense claim processing introduces several operational challenges:

- **Manual receipt extraction:** Finance teams must read receipts and extract transaction details.
- **Expense classification:** Alcoholic beverages must be identified and excluded from reimbursement.
- **Policy compliance:** Expenses must be evaluated against company reimbursement policies.
- **Calculation errors:** Taxes and tips must be recalculated based on eligible purchases rather than the entire receipt.
- **Operational inefficiency:** Repetitive reviews increase processing time and administrative workload.

The objective was to develop an AI agent capable of independently executing these steps and generating a structured expense claim report.

## Solution Architecture

The solution uses a single-agent architecture built with LangGraph and LangChain. The agent follows a ReAct (Reasoning and Acting) workflow, selecting tools, executing tasks, observing outputs, and generating a final reimbursement report.

### 1. Specialized Tool Development

Four specialized tools were developed to handle individual stages of expense processing.

| Tool | Function |
|---|---|
| Text Extractor | Uses an LLM with image-processing capabilities to extract merchant details, transaction dates, individual items, quantities, taxes, and tips from receipts. |
| Categorizer | Classifies receipt items into Food, Non-Alcoholic Drinks, and Alcoholic Drinks. |
| Verifier | Uses semantic search and RAG to retrieve company expense policies and determine reimbursement eligibility. |
| Calculator | Performs precise calculations for eligible subtotals, sales tax, tips, and final reimbursement amounts. |

### 2. Retrieval-Augmented Generation (RAG)

Implemented a document retrieval pipeline to enable the agent to reference company expense policies when evaluating reimbursement eligibility.

The pipeline includes:

- Extracting text from company expense policy PDFs using PyPDF.
- Cleaning and preprocessing extracted policy text.
- Splitting documents into smaller text chunks.
- Generating embeddings for semantic representation.
- Retrieving relevant policy sections through similarity search.

The Verifier uses retrieved policy information to distinguish reimbursable expenses from excluded purchases.

### 3. Intelligent Tool Selection

Implemented an embedding-based tool selection mechanism that retrieves relevant tools based on the user's task.

Rather than exposing every available tool for every request, the system searches tool descriptions and selects the tools most relevant to the task.

### 4. Agent Development with LangGraph

Built the agent workflow using LangGraph, with shared state management and conditional routing.

The workflow follows these stages:

1. **Tool Selection:** Identifies the tools required for the requested task.
2. **Planning:** The LLM determines the next appropriate action.
3. **Tool Execution:** Selected tools process the receipt, classify expenses, verify policy compliance, and perform calculations.
4. **Observation:** The agent receives tool outputs and determines whether additional actions are necessary.
5. **Final Response:** Generates a structured expense claim report.

### 5. Reflective Reasoning

Extended the original workflow with a reflection mechanism to improve output reliability.

A reviewer node evaluates the agent's draft report against expense policy and verification results.

- Approved reports proceed to completion.
- Reports containing errors are returned to the planner with corrective feedback.
- A maximum reflection limit prevents indefinite review loops.

This introduces an additional quality-control layer for identifying classification and calculation errors.

## Technology Stack

| Technology | Application |
|---|---|
| Python | Core implementation and tool development |
| OpenAI GPT | LLM reasoning, receipt extraction, classification, and report generation |
| LangChain | LLM integration, prompts, and tool development |
| LangGraph | Agent orchestration, state management, and workflow routing |
| RAG | Retrieval of relevant company expense policies |
| Embeddings | Semantic similarity search and tool selection |
| PyPDF | PDF policy document extraction |
| NumExpr | Mathematical expression evaluation |
| Tenacity | Retry handling for temporary API and proxy errors |
| Jupyter Notebook | Development and experimentation |

## Results and Outcomes

The system was evaluated using an itemized restaurant receipt from Wicker Park Tavern in Chicago.

The receipt contained a mixture of reimbursable food and non-alcoholic beverages alongside alcoholic drinks that were excluded under company policy.

### Expense Processing Results

The agent successfully:

- Extracted receipt details, including merchant information, transaction date, individual items, and payment information.
- Classified alcoholic and non-alcoholic purchases.
- Identified non-reimbursable alcoholic beverages using company expense policy.
- Recalculated sales tax and tip using only eligible expenses.
- Generated a structured expense claim report.

### Reimbursement Calculation

| Expense | Amount |
|---|---:|
| Original receipt subtotal | $90.00 |
| Excluded alcoholic beverages | $42.00 |
| Reimbursable subtotal | $48.00 |
| Eligible sales tax (10.25%) | $4.92 |
| Eligible tip (20%) | $9.60 |
| **Total reimbursement** | **$62.52** |

The system correctly excluded $42.00 in alcoholic beverages and calculated the eligible reimbursement as $62.52.

### Agent Evaluation

| Evaluation | Outcome |
|---|---|
| Standard agent | Correctly calculated the expected reimbursement of $62.52 |
| Reflective agent | Produced the same correct reimbursement of $62.52 |
| Reflection review | Approved the generated report after one review |
| Flawed draft evaluation | Rejected an incorrect $75.23 claim |
| Error identification | Identified incorrect tax and tip calculations and provided corrective feedback |
| Tool selection | Successfully selected only the calculator for a standalone calculation task |

Both the standard and reflective agents matched the expected reimbursement in the test scenario.

The reflection mechanism also demonstrated its ability to detect errors when presented with an intentionally flawed draft.

*Note: Results represent a controlled demonstration using one receipt, rather than a large-scale accuracy benchmark.*

## Business Impact and Applications

The project demonstrates several opportunities for applying agentic AI to financial operations:

1. **Process Automation:** Automating receipt extraction, classification, policy verification, and reimbursement calculations can reduce manual finance workloads.
2. **Improved Auditability:** Specialized tools provide traceability into reimbursement decisions and calculations.
3. **Policy Compliance:** RAG enables the agent to reference relevant policy documentation when evaluating expenses.
4. **Error Reduction:** Reflective reasoning introduces an additional review mechanism for identifying incorrect classifications and calculations.
5. **Scalability:** The modular architecture provides a foundation for expanding the system to additional expense categories.

## Future Enhancements

- Introduce human-in-the-loop approval for exceptions and claims that fail repeated reviews.
- Build a larger labeled receipt dataset to benchmark extraction, classification, and reimbursement accuracy.
- Extend the tool registry to support hotel, airfare, and mileage expense processing.
- Introduce duplicate receipt detection.
- Maintain centralized logs of agent decisions and tool execution traces for auditing.
- Integrate location-specific tax reference data to validate receipt tax rates.

## Key Learning Outcomes

Through this project, I gained practical experience in:

- Designing and implementing agentic AI workflows.
- Building specialized LLM-powered tools using LangChain.
- Orchestrating multi-step AI workflows using LangGraph.
- Implementing RAG pipelines for document-based policy retrieval.
- Using embeddings for semantic search and intelligent tool selection.
- Applying ReAct and reflective reasoning patterns.
- Designing modular AI systems with shared state and conditional routing.
- Evaluating agent outputs against expected business outcomes.

## Project Files

- `MLS_Week14_Single_Agent_Systems.ipynb` — Jupyter Notebook containing the implementation, experiments, agent workflow, and evaluation.

---

**Project Focus:** Agentic AI | Financial Process Automation | LLM Applications | RAG | AI Workflow Orchestration
