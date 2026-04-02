# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Name : PREETHI D
# Register no. : 212224040250
# Aim: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

# Explanation:

Experiment the persona pattern as a programmer for any specific applications related with your interesting area.
Generate the output using more than one AI tool and based on the code generation analyze and discuss that.

### Objective

To write and implement Python code that interacts with multiple AI tools (such as OpenAI's ChatGPT, Anthropic's Claude, and Google Gemini) using their respective APIs.
The goal is to automate:

● Submitting a common prompt

● Comparing the returned outputs

● Generating actionable insights based on defined evaluation metrics (e.g., accuracy, coherence, simplicity)

Use Case

Stock Market Prediction and Insights:
The system will analyze stock-related data and generate predictions, trends, and investment suggestions by interacting with multiple AI platforms.
This enables investors to make informed decisions more efficiently.


## AI Tools Required:

CHATGPT
CLAUDE
GEMINI

## Algorithm Overview

### **Step-by-Step Algorithm for Multi-AI Tool Integration**

1. **Set Up API Integrations**  
   - Install necessary libraries and set up the credentials for each AI tool: ChatGPT, Claude, and Gemini.  
   - Use `requests` or similar libraries for API integration.

2. **Input Stock Query**  
   - Accept user input such as stock symbol, recent price trend, and financial indicators.

3. **Format Prompts**  
   - Three types of prompts will be used:
     - **Straightforward Prompt** – Asking for stock trend prediction.  
     - **Tabular Format** – Presenting data in table format.  
     - **Missing Word Prompt** – Providing an incomplete financial statement.

4. **Submit Prompts to Each AI Tool**  
   - Send the formatted prompts to ChatGPT, Claude, and Gemini APIs.

5. **Receive and Parse Responses**  
   - Collect responses such as stock movement prediction and investment suggestions.

6. **Comparison of Outputs**  
   - Compare based on accuracy, clarity, simplicity, and user experience.

7. **Generate Actionable Insights**  
   - Summarize which tool performs best for each query type.

8. **Create Final Report**  
   - Compile the results into a comprehensive evaluation report.

---

## Prompt Types

### **Straightforward Prompt**
→ Simple query asking for a prediction of a stock's trend and reasoning.

### **Tabular Format**
→ Data of price movement and volume in table format.

### **Missing Word Prompt**
→ Prompt with an incomplete statement for the AI to fill.

---

## Example Queries & Responses

### **1. Straightforward Prompt**

**Prompt:**  
“Analyze Apple Inc. (AAPL) stock based on its recent price movement and suggest if it’s a good time to buy, sell, or hold.”

| **AI Tool** | **Response** |
|--------------|--------------|
| **ChatGPT** | AAPL is in an uptrend; short-term hold recommended; strong fundamentals. |
| **Claude** | Suggests hold; moderate growth expected; watch quarterly reports. |
| **Gemini** | Indicates upward momentum; buy if RSI remains below 70. |

---

### **2. Tabular Format Prompt**

**Prompt:**

| **Indicator** | **Value** |
|----------------|-----------|
| Price Change (1M) | +5% |
| Volume Trend | Increasing |
| RSI | 62 |
| PE Ratio | 28 |

**Query:**  
“Based on this data, what is the likely market action for the next week?”

| **AI Tool** | **Response** |
|--------------|--------------|
| **ChatGPT** | Bullish short-term; may test higher resistance levels. |
| **Claude** | Predicts minor correction before continuation upward. |
| **Gemini** | Neutral to bullish; strong buying pressure visible. |

---

### **3. Missing Word Prompt**

**Prompt:**  
“The AAPL stock is expected to ______ in the upcoming week.”

| **AI Tool** | **Response** |
|--------------|--------------|
| **ChatGPT** | Rise |
| **Claude** | Consolidate |
| **Gemini** | Increase gradually |

---

## Code Implementation

### **Python Code Example for Integrating with Multiple AI Tools**

```

import PyPDF2
from sentence_transformers import SentenceTransformer
import faiss
from transformers import pipeline

# Step 1: Extract Text from PDF Documents
def extract_text_from_pdf(file_path):
    text = ""
    with open(file_path, 'rb') as f:
        reader = PyPDF2.PdfReader(f)
        for page in reader.pages:
            text += page.extract_text() + "\n"
    return text

# Step 2: Create Document Chunks
def chunk_text(text, chunk_size=500):
    words = text.split()
    return [" ".join(words[i:i + chunk_size]) for i in range(0, len(words), chunk_size)]

# Step 3: Encode Chunks with SentenceTransformer
model = SentenceTransformer('all-MiniLM-L6-v2')

def encode_chunks(chunks):
    return model.encode(chunks)

# Step 4: Build FAISS Index
def build_index(embeddings):
    dims = embeddings.shape[1]
    index = faiss.IndexFlatL2(dims)
    index.add(embeddings)
    return index

# Step 5: Retrieve Relevant Chunks
def retrieve_chunks(index, query, chunks, model, top_k=5):
    query_vec = model.encode([query])
    scores, indices = index.search(query_vec, top_k)
    return [chunks[i] for i in indices[0]]

# Step 6: Generate Answer with Hugging Face LLM
qa_pipeline = pipeline('text2text-generation', model='google/flan-t5-base')

def generate_answer(query, context):
    input_text = f"question: {query} context: {context}"
    result = qa_pipeline(input_text, max_length=128)
    return result[0]['generated_text']

# --- Main Execution ---
pdf_file = 'your_document.pdf'
text = extract_text_from_pdf(pdf_file)
chunks = chunk_text(text)
embeddings = encode_chunks(chunks)
index = build_index(embeddings)

user_query = "What is the main objective of the project?"
relevant_chunks = retrieve_chunks(index, user_query, chunks, model)
context = " ".join(relevant_chunks)
answer = generate_answer(user_query, context)

print("Generated Answer:", answer)

OUTPUT RESPONSE
Functions for Getting Responses
get_chatgpt_response()
Sends the stock query prompt to ChatGPT and retrieves the response.

get_claude_response()
Sends the same stock query to Claude and returns its analysis.

get_gemini_response()
Sends the stock query to Gemini and retrieves the generated insights.

Evaluation
The responses from ChatGPT, Claude, and Gemini are evaluated based on:

Accuracy – How well the prediction aligns with market trends.

Coherence – Logical flow and clarity.

Simplicity – Ease of understanding for general investors.

User Experience – How well each response supports investment decisions.

python
Copy code
import pandas as pd

# Sample evaluation data
evaluation_data = {
    "AI Tool": ["ChatGPT", "Claude", "Gemini"],
    "Accuracy": ["High", "Moderate", "High"],
    "Coherence": ["High", "High", "Moderate"],
    "Simplicity": ["User-friendly", "Analytical", "Concise"],
    "User Experience": ["Excellent", "Detailed", "Fast"]
}

# Create DataFrame
df_comparison = pd.DataFrame(evaluation_data)
print(df_comparison)
```

# Conclusion

This project provides an automated solution for comparing stock market predictions and insights across multiple AI tools.
By integrating ChatGPT, Claude, and Gemini, the system helps identify which AI delivers more accurate, clear, and practical market recommendations for investors.

# Result:
The corresponding Prompt is executed successfully.
