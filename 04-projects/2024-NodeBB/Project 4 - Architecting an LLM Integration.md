---
created: "2024"
tags:
  - "#software-architecture"
  - "#LLM"
  - "#microservice"
org:
  - CMU
goal: Implement Mark Resolved Feature in NodeBB
---
### Step 1: Querying Azure OpenAI's GPT-4o-Mini Model

#### 1.1: Set Up Python Environment and Authenticate with Azure OpenAI

Install the `openai` library and authenticate using your Azure OpenAI API key and endpoint ([link](https://docs.google.com/document/d/1HFLFTBMLOc10nDHbHdeBsaZxHfbZFSjQEKlzyir39FQ/edit?usp=sharing)). This allows you to access and interact with a large language model hosted on Azure.

```python
!pip install --quiet openai

from openai import AzureOpenAI

# Initialize the Azure OpenAI client
client = AzureOpenAI(
   api_key="your-azure-api-key",  
   api_version="2024-02-15-preview",
   azure_endpoint="https://your-azure-endpoint-url"  
)
```
#### 1.2: Query the GPT-4o-Mini Model with Custom Questions

Define a function to send custom questions to the model. By specifying a `context` and `question`, you can tailor the model’s responses to specific needs.

```python
def query_llm(question: str) -> str:
    context = "Sample Context"
    response = client.chat.completions.create(
        model="gpt-4o-mini",  
        messages=[
            {"role": "system", "content": context},
            {"role": "user", "content": question}
        ]
    )
    return response.choices[0].message.content

# Example call to the function
print(f"Sample LLM Output: {query_llm('What is AI?')}")
```

Based on this setup, you can adapt the `query_llm` function to specific tasks like `get_translation(post)` or `get_language(post)` by customizing the `context` accordingly.

#### 1.3: Implement Translation and Language Detection

To handle both translation and language detection in a single function, use `query_llm` to first detect the language. If the text is non-English, the function will then call a translation process.

  ```python
  def query_llm(post: str) -> tuple[bool, str]:
      language = get_language(post).strip()
      if language == "English":
          return (True, post)
      elif language == "Non-English":
          translation = get_translation(post).strip()
          return (False, translation)
      else:
          return (False, "")
  ```

#### 1.4: Add Error Handling and Robustness Testing

Modify `query_llm` to handle unexpected outputs gracefully. 

  ```python
def query_llm_robust(post: str) -> tuple[bool, str]:
	try:
	  response = query_llm(post)
	
	  # Check if response is a properly formatted tuple (bool, str)
	  if isinstance(response, tuple) and len(response) == 2 and isinstance(response[0], bool) and isinstance(response[1], str):
		  return response
	  else:
		  return (False, "Erroneous response format")
	except Exception:
		return (False, "A general error occurred")
  ```

This function will manage unexpected response formats without interrupting the flow in applications like NodeBB, returning a default error message if the response is invalid.

### Step 2: Add a translation micro-service

flask app
{link to the service template}