# Loan Risk - AI Agent

This repository provides an AI agent sample application for demonstration and proof-of-concept(PoC) to help agentic AI adoption in industry/enterprise workflows and use cases.

With a focus on the financial industry, it uses a bank loan processing workflow as an example to leverage agentic AI. It demonstrates one of the main values of using agentic AI - _relying on LLMs to reason about what to do and take actions_, instead of relying on traditional approach of rules and conditions. The demo is built on IBM Cloud – using Code Engine to host code for the AI agent, watsonx Assistant for the chat conversation, and watsonx.ai for the LLMs and retrieval augmented generation (RAG).

For questions or feedback contact Anuj Jain (jainanuj@us.ibm.com)

To learn more about the key features and architectural concepts of agentic AI using the Loan Risk AI Agent as a financial industry use case refer to the article [Agentic AI in enterprise workflow automation](https://developer.ibm.com/articles/agentic-ai-workflow-automation/).

## Use Case
+ AI agent to support bank loan risk evaluation workflow.
+ AI agent determines overall risk and interest rate for a bank loan using LLMs and relevant tools.

Similar use cases can be found in insurance, healthcare and other industry/enterprise workflows.


## Architecture
+ Architecture: Single AI Agent with Tools (using LangGraph, TypeScript/NodeJS)
+ LLM: mistral-large (from IBM watsonx.ai)
+ Tools: API/functions (for credit score, account status, risk evaluation criteria, interest rate determination)


![Conceptual architecture](artifacts/architecture/LoanRisk-Single-AI-Agent-Conceptual.png)

![High-level deployment architecture](artifacts/architecture/LoanRisk-Single-AI-Agent-Deployment.png)


## Deployment
#### Prerequisites
Requires IBM Cloud account with:
- watsonx.ai services
- Account user with administrative privileges for watsonx.ai, Code Engine, Container Registry
- Account user’s API key

Optionally, 

For using RAG LLM (Agentic RAG feature) you will also need:
-	watsonx.ai Deployment endpoint for vector index RAG inferencing

For using watsonx Assistant/Orchestrate (Chat widget) you will also need:
-	watsonx.ai Assistant/Orchestrate service
  
#### Installation/Deployment options include:
- IBM Cloud Code Engine - Refer instructions [here.](artifacts/deployment/deployment-README.md)
- Python Notebook - A simplified version of this PoC can be downloaded from [here](artifacts/python-notebook/NB-ai-agent-loan-risk-demo-v1.ipynb) and imported in watsonx.ai Project as an asset. It is meant for understanding the concepts and for experimentation.


## Usage
For usage and additional examples refer [here.](artifacts/usage-examples/usage-examples-README.md)

![Example usage screenshot](artifacts/usage-examples/UsageExample2.png)

  


