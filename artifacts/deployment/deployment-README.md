
# Steps for Deployment on IBM Cloud Code Engine

NOTE: This document is work in progress.

## Prerequisites
Requires IBM Cloud account with:
-	watsonx.ai service and Project for inferencing [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/signup-wx.html?context=wx&audience=wdp)
-	Code Engine Project to deploy the application [(refer)](https://cloud.ibm.com/docs/codeengine)

Requires these environment variables when deploying the application:
-	watsonx.ai API Key for authentication (envar: WATSONX_AI_APIKEY) [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/admin/admin-apikeys.html?context=wx&locale=en&audience=wdp)
-	watsonx.ai Project ID for authentication (envar: WATSONX_PROJECT_ID) [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/projects.html?context=wx&audience=wdp)

## Deployment Steps

1. Complete prerequisites above. Make sure you have the values for the envars ready. You will need them for setting as environment variables during the application deployment.
In addition you will set envar APPLICATION_PORT.

2. Follow instructions from Code Engine documentation to deploy an application from source code.
Refer section [Deploying your app from repository source code from the console](https://cloud.ibm.com/docs/codeengine?topic=codeengine-app-source-code&interface=ui)

*Note on costs*: The deployment will incur costs for Code Engine runtime, watsonx.ai runtime and watsonx.ai LLM inferencing.

## Optional Features

#### For using RAG LLM (Agentic RAG feature) you will need to do the following:
- Create a vector index asset "Ground gen AI with vectorized document" in the watsonx.ai Project using these [content PDF documents](artifacts/data). To create the index use vector store - "In memory", embedding model - "allminilm-l6-v2", chunksize - "2000", chunk overlap - "200".
- Open the vector index you just created in Prompt Lab and set the generative AI model to "mistral-large". Test the index by asking some questions e.g., what is the risk for credit score 655 and account status closed?, what is the interest rate for medium risk?, and confirm answers are using RAG from the content PDF documents. 
- Create a new watsonx.ai Deployment Space with Deployment stage set as Production. [(refer)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml-spaces_local.html?context=wx&locale=en&audience=wdp) 
- Deploy the vector index as watsonx.ai Deployment on AI service for inferencing using the "fast path" option  (use the "Deploy" button). [(refer-1)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ai-services-overview.html?context=wx&locale=en) [(refer-2)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ai-services-prompt-lab.html?context=wx) and [(refer-3)](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ai-services-deploy-fast-path.html?context=wx).  
- Capture the	watsonx.ai Deployment private endpoint for the vector index for RAG inferencing (use non-stream; ends with ai_service?version=...)
- On Code Engine add the following envars with the values captured above and deploy (or redeploy) the application (envar: ENABLE_RAG_LLM=true and envar: WATSONX_RISK_RAG_LLM_ENDPOINT=endpoint captured above)
- The application will now use the RAG content for risk and interest tools.

#### For using watsonx Assistant/Orchestrate (Chat widget) you will need to do the following:
- First, you must deploy the application to Code Engine without watsonx Assistant (envar:ENABLE_WXASST=false ; default). 
- Once deployed, get the deloyed application URL.
- Open API file (agentic-ai-app-custom-ext-openapi.json) and update the URL with the deployed appliaciton URL.
- Create a action skill in watsonx Assistant instance
- Create and add a custom extension by importing the updated Open API file (agentic-ai-app-custom-ext-openapi.json). 
- Import the zip file to set up the actions that use the custom extension (wx-asst-agentic-ai-app.zip)
- Update the watsonx Assistant Web chat configuration (if needed)
- Capture the integrationID, region and serviceInstanceID from the embed script.
- On Code Engine add the following envars with the values captured above and redeploy the application (envar:ENABLE_WXASST=true, WXASST_INTEGRATION_ID, WXASST_REGION, WXASST_SERVICE_INSTANCE_ID captured above)
- The watsonx Assistant will become avaialble on the page /wx.html



Other optional envars include: IBM_IAM_TOKEN_ENDPOINT, WATSONX_AI_AUTH_TYPE, WATSONX_SERVICE_URL. Default values are already set the code. [(refer)](https://cloud.ibm.com/apidocs/machine-learning#authentication)




