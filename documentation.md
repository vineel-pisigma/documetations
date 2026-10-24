# PiSigma — Research and Development

**Purpose:** Document dataset research, phishing detection research, and practical AI integration experiments.

---

## 1. SMS Dataset Research

### Objective
Prepare a consolidated SMS dataset for smishing detection research.

### Work Completed
- Collected datasets from Mendeley, UCI, and NCSU.
- Standardized dataset formats.
- Merged the datasets and removed exact duplicates.
- Analyzed conflicting labels.
- Created a master dataset.

### Results

| Metric | Records |
|---|---:|
| Total source records | 79,574 |
| Unique messages after exact deduplication | 36,411 |
| Duplicate records removed | 43,163 |

**Output:** `datasets/master/master_final.csv`

**Repository:** https://github.com/vineel-pisigma/SmishingDatasets

*Verify these counts against the final dataset before publishing.*

---

## 2. URL-Based Phishing Detection Research

### Objective
Investigate how to identify potentially malicious URLs without analyzing message content.

### Research Areas
- Heuristic-based detection.
- Traditional machine learning.
- Random Forest classification.
- Existing phishing URL detection services and models.

### research links 
https://www.sciencedirect.com/science/article/pii/S1319157823000034#s0065

https://www.sciencedirect.com/science/article/abs/pii/S1389128620301109

https://www.sciencedirect.com/science/article/abs/pii/S0167404819301622?via%3Dihub

### Goal
Identify suitable methods for building a URL-based phishing detection system.

---

## 3. AI Integration Experiments

### Objective
understand different ai integrations 

**Repository:** https://github.com/vineel-pisigma/ai-integration-workspace

### 3.1 Gemini API — Terminal Chatbot

**Objective:** Send prompts to Google's Gemini API from a terminal application and display AI-generated responses.

**Technologies:** JavaScript, Node.js, Gemini API.

![Gemini Terminal Chatbot](screenshots/01-api.png)

### 3.2 Gemini API — React Chat

**Objective:** Build a React chat interface connected to an Express backend.

**Technologies:** React, Vite, Express, Gemini API.

![Gemini React Chat](screenshots/01-gemini-react.png)

### 3.3 Gemini API — Express Backend

**Objective:** Expose an API endpoint that accepts a question and returns a Gemini response.

**Technologies:** Node.js, Express, Gemini API.

![Gemini Express Backend](screenshots/01-gemini-backend.png)

### 3.4 Ollama — Local AI Chatbot

**Objective:** Run a language model locally and interact with it through the terminal.

**Model:** Qwen 2.5 0.5B.

**Technologies:** Python, Ollama.

![Ollama Local Chatbot](screenshots/02-ollama.png)

### 3.5 Task-Specific AI — Sentiment Analysis

**Objective:** Use a pretrained Transformer model to classify sentences as positive or negative.

**Technologies:** Python, PyTorch, Hugging Face Transformers.

![Sentiment Analysis](screenshots/03-task-specific.png)

### 3.6 Transformer Model Inspection

**Objective:** Inspect tokenization, token IDs, logits, probabilities, and sentiment predictions.

**Technologies:** Python, PyTorch, Hugging Face Transformers.

![Transformer Model Inspection](screenshots/03-inspection.png)

### 3.7 Gradio Sentiment Application

**Objective:** Provide a web interface for testing sentiment predictions.

**Technologies:** Python, Gradio, Transformers.
**Gradio url:** https://huggingface.co/spaces/vineel6/sentiment-finder



### 3.8 FastAPI and React Application

**Objective:** Connect a React frontend to a FastAPI backend that requests sentiment predictions from a hosted Hugging Face Space.

**Technologies:** React, FastAPI, Python, Gradio Client.

![FastAPI and React](screenshots/03-local.png)

### 3.9 Cloudflare Workers — Sentiment API

**Objective:** Expose an API that forwards text to a hosted Hugging Face Space and returns its sentiment prediction.
 
**Technologies:** Cloudflare Workers, Python, HTTPX, Hugging Face Spaces.

**Live:**https://sentiment-finder-web.pages.dev/

![Cloudflare Sentiment API](screenshots/09-cloudflare.png)

---
### basic test during developments 
  ![direct api acess ](screenshots/apikey-acess.png)
  ![rate limiting on api key ](screenshots/ratelimiting.png)
  ![rate limiting the key from provider ](screenshots/ratelimit2.png)
  ![xss test ](screenshots/xss.png)
  ![prompt ingection](screenshots/promptingection.png)
  ![end points testing ](screenshots/endpointtesting.png)



## 4. Summary

This work covers SMS dataset preparation, URL-based phishing detection research, and AI integration through hosted APIs, local models, pretrained Transformers, web applications, and cloud deployment experiments.

