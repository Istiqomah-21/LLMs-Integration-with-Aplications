# LLMs-Integration-with-Aplications

This repository provides an implementation guide for integrating Large Language Models (LLMs) into business applications using IBM's **watsonx.ai**.

## Overview

Prompt engineering is an essential part of integrating LLMs into applications, but it is just one step in the process. Below is a high-level review of the steps involved in integrating LLMs with applications:

1. **Prompt Engineering**
2. **Model Tuning**
3. **Model Deployment**
4. **Testing and Integration**

## Steps to Get Started

### 1. Testing the Prompt

Start by navigating to the **Prompt lab** and opening an existing prompt or a sample prompt. 

Generate a response:

*The Prompt:*

I started my loan process toward securing a VA loan. I was waiting for a a month and a couple weeks, then I was told that the VA needed to acquire my retirement points to verify my veteran status. If I knew this is what my loan was on hold for, I could have contacted the VA office right away and got this cleared up ...

*Model output:*

Top bullet points:
1. The VA needed to acquire my retirement points to verify my veteran status. 
2. The underwriting department took a long time to verify my employment status. 
3. ...

## Setup Instructions
- Python
- IBM Cloud API key
- Project ID from Watsonx.ai

## API Integrations
    import requests
    from requests.structures import CaseInsensitiveDict
    
    # Get access token
    apiKey = "YOUR_API_KEY"
    url = "https://iam.cloud.ibm.com/oidc/token"
    headers = CaseInsensitiveDict()
    headers["Content-Type"] = "application/x-www-form-urlencoded"
    data = "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=" + apiKey
    
    resp = requests.post(url, headers=headers, data=data)
    if resp.status_code == 200:
        json_resp = resp.json()
        access_token = json_resp.get('access_token')
        print(access_token)
    else:
        print("Failed to retrieve access token. Status code:", resp.status_code)


## Generate Text

    import requests
    
    url = "https://us-south.ml.cloud.ibm.com/ml/v1/text/generation?version=2023-05-29"
    
    body = {
    	"input": """Please provide top 5 bullet points in the review provided in '\'''\'''\''.
    
    
    Review:
    '\'''\'''\''
    
    (Enter Your Text or paragraph)

    '\'''\'''\''
    
    Top bullet points:
    
    """,
    	"parameters": {
    		"decoding_method": "greedy",
    		"max_new_tokens": 300,
    		"min_new_tokens": 50,
    		"repetition_penalty": 1
    	},
    	"model_id": "google/flan-ul2",
    	"project_id": "e53dc72d-9ab3-4c48-8d06-06c661f14582"
    }
    
    headers = {
    	"Accept": "application/json",
    	"Content-Type": "application/json",
    	"Authorization": "Bearer YOUR ACCESS TOKEN"
    }
    
    response = requests.post(
    	url,
    	headers=headers,
    	json=body
    )
    
    if response.status_code != 200:
    	raise Exception("Non-200 response: " + str(response.text))
    
    data = response.json()
    data_clean = data['results'][0]['generated_text'] 
    print(data_clean) 

## Conclusion
This repository serves as a practical guide to integrating IBM Watson's watsonx.ai LLMs into our business applications. By using the provided Python SDK or REST API, we can easily interact with pre-deployed models and start building powerful AI-driven applications.
