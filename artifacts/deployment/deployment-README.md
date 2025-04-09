# Steps for Deploying on IBM Cloud Code Engine

## Prerequisites

Requires IBM Cloud account with:
- watsonx.ai services [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/signup-wx.html?context=wx&audience=wdp). This includes:
  - watsonx.ai Runtime
  - watsonx.ai Studio 
  - Cloud Object Storage
- Account user with administrative privileges for:
  - watsonx.ai - to create Projects, access services, store assets, etc. [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/projects.html?context=wx&audience=wdp)
  - Code Engine – to create Project and deploy the application. [(refer-1)](https://cloud.ibm.com/docs/codeengine?topic=codeengine-getting-started) [(refer-2)](https://cloud.ibm.com/docs/codeengine?topic=codeengine-app-source-code)
  - Container Registry – to create namespace, store images for deploying the application
- Account user’s API key [(refer)](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key)


  
## Deployment steps summary
A detailed document of steps is available [here](Deploying-Loan-Risk-AI-Agent-on-Code-Engine.pdf).
#### 1. Confirm prerequisites and gather information
Note the user’s API key value. It will be the environment variable WATSONX_AI_APIKEY.

Depending on where the watsonx.ai service is deployed get the watsonx.ai API endpoint URL from [here](https://cloud.ibm.com/apidocs/watsonx-ai#endpoint-url). It will be the environment variable WATSONX_SERVICE_URL.

#### 2. Create a Project in watsonx.ai 
Launch watsonx.ai and create a Project, or use an existing one.

Note the Project Id. This will be the environment variable WATSONX_PROJECT_ID.

#### 3.	Deploy application to Code Engine 
Follow the steps to _deploy the application from repository source code_ as described [here](https://cloud.ibm.com/apidocs/watsonx-ai#endpoint-url).

Use code repo URL: https://github.com/IBM/ai-agent-for-loan-risk.git

Add the environment variables and the values captured above to the deployment.
- WATSONX_AI_APIKEY
- WATSONX_PROJECT_ID
- WATSONX_SERVICE_URL

Launch the application using the Code Engine URL for the deployed application.

#### 4.	Validate the application
Ask “What is the interest rate for matt? Explain how it was determined?”. You should get a response explain high risk and 8% interest rate. 

Refer to the application [usage section](../usage-examples/usage-examples-README.md).

#### 5.	Optionally enhance application with additional features
Once initial application is deployed and running successfully, you can add enhancements.
- Using RAG LLM (Agentic RAG feature)
- Using watsonx Assistant/Orchestrate (Chat widget)

#### 6.	Optional: Using RAG LLM (Agentic RAG feature)
By default, the risk and interest rate tools of the AI agent simulate risk and interest rate determination. By adding this feature, the AI agent tool will make RAG query to retrieve relevant content from the bank documents and use that to determine the risk and interest rate. In the AI agent response you will be able to see the content from the document table and its interpretation by the AI agent. 
- Create a new watsonx.ai Deployment Space with Deployment stage set as Production. [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml-space-create.html?context=wx&locale=en&audience=wdp)
- Create a vector index asset in the watsonx.ai Project using these content PDF documents. To create the index use vector store - "In memory", embedding model - "allminilm-l6-v2", chunksize - "2000", chunk overlap - "200".
- Open the vector index you just created in Prompt Lab and set the generative AI model to "mistral-large". Test the index by asking some questions e.g., what is the risk for credit score 655 and account status closed?, what is the interest rate for medium risk?, and confirm answers are using RAG from the content in PDF documents provided [here](../data).
- Deploy the vector index as watsonx.ai Deployment on AI service for inferencing using the "fast path" option (use the "Deploy" button).
[(refer-1)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ai-services-overview.html?context=wx&locale=en) [(refer-2)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ai-services-prompt-lab.html?context=wx) and [(refer-3)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ai-services-deploy-fast-path.html?context=wx).
- Capture the watsonx.ai Deployment private endpoint for the vector index for RAG inferencing (use non-stream; ends with ai_service?version=...)
- On Code Engine add the following environment variables with the values captured above and redeploy the application (ENABLE_RAG_LLM=true and WATSONX_RISK_RAG_LLM_ENDPOINT=endpoint captured above)
- The application will now use the RAG content for risk and interest tools.

#### 7.	Optional: Using watsonx Assistant/Orchestrate (Chat widget)
By adding this feature, you can get a more conversational/chat experience when asking questions in the watsonx Assistant chat widget. The conversation is single turn and the watsonx Assistant skills can be enhanced further if needed.
- Note the Code Engine URL for the deployed application.
- Open API file (agentic-ai-app-custom-ext-openapi.json) and update the URL with the deployed appliaciton URL.
- Create an action skill in watsonx Assistant instance
- Create and add a custom extension by importing the updated Open API file [agentic-ai-app-custom-ext-openapi.json](../wxAssistantOrchestrate/agentic-ai-app-custom-ext-openapi.json).
- Import the zip file to set up the actions that use the custom extension [wx-asst-agentic-ai-app.zip](../wxAssistantOrchestrate/wx-asst-agentic-ai-app.zip).
- Open the watsonx Assistant Web chat configuration and note the integrationID, region and serviceInstanceID from the Embed script tab.
- On Code Engine open the deployed application configuration, add the following environment variables with the values captured above and redeploy the application (ENABLE_WXASST=true, WXASST_INTEGRATION_ID, WXASST_REGION, WXASST_SERVICE_INSTANCE_ID captured above)
- The watsonx Assistant will become available on the page <application-url>/wx.html

