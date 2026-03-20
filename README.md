# YouTube Chatbot — LangChain + OpenRouter

A chatbot that loads any YouTube video transcript and lets you summarize and chat with it.

---

## Requirements

- Python 3.10 or above
- OpenRouter API key — get it from https://openrouter.ai/keys

---

## Installation

### 1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/youtube-chatbot.git
cd youtube-chatbot
```

### 2. Install dependencies
```bash
pip install \
  "youtube-transcript-api==1.2.4" \
  "langchain==0.2.16" \
  "langchain-community==0.2.16" \
  "langchain-openai==0.1.23" \
  "langchain-core==0.2.43" \
  "faiss-cpu" \
  "sentence-transformers" \
  "openai==1.30.0"
```

### 3. Add your OpenRouter API key

Open `app.py` and replace this line:
```python
OPENROUTER_API_KEY = "sk-or-v1-xxxxxxxxxxxxxxxx"
```

with your actual key from https://openrouter.ai/keys

---

## Run on Google Colab

Open the notebook and run cells in order:

- Cell 1 — Install dependencies
- Cell 2 — Load all functions
- Cell 3 — Load video and get summary
- Cell 4 — Chat loop

> Note: After Cell 1, restart the runtime before running Cell 2.
> Runtime -> Restart Session

---

## Run on VS Code / Local Machine
```bash
pip install streamlit
streamlit run app.py
```

Then open http://localhost:8501 in your browser.

---

## Package Versions

| Package | Version |
|---|---|
| youtube-transcript-api | 1.2.4 |
| langchain | 0.2.16 |
| langchain-community | 0.2.16 |
| langchain-openai | 0.1.23 |
| langchain-core | 0.2.43 |
| faiss-cpu | latest |
| sentence-transformers | latest |
| openai | 1.30.0 |
| streamlit | latest |

> Important: Do not upgrade langchain, langchain-openai, langchain-core,
> or openai beyond the versions listed above. Newer versions have breaking
> changes that will cause errors.

---

## Free Models Used

The chatbot automatically tries these free OpenRouter models in order
and uses the first one that works:

- nvidia/nemotron-3-super-120b-a12b:free
- openai/gpt-oss-20b:free
- qwen/qwen3-4b:free
- google/gemma-3n-e4b-it:free
- arcee-ai/trinity-mini:free
- liquid/lfm-2.5-1.2b-instruct:free

> If all models fail with rate limit errors, wait a few minutes and try again.

---

## Common Errors and Fixes

| Error | Fix |
|---|---|
| No transcript found | Video has no subtitles. Try a different video. |
| AuthenticationError 401 | OpenRouter API key is wrong or expired. |
| RateLimitError 429 | Free model is busy. Wait a minute and retry. |
| ModuleNotFoundError | Re-run Cell 1 and restart runtime. |
| ValidationError proxies | openai version is too new. Use openai==1.30.0 |

---

## Project Structure
```
youtube-chatbot/
    app.py              # Streamlit web app
    notebook.ipynb      # Google Colab notebook
    requirements.txt    # All dependencies
    README.md           # This file
```

---

## How It Works

1. YouTube URL or video ID is provided
2. Transcript is fetched using youtube-transcript-api
3. Transcript is split into chunks using LangChain text splitter
4. Chunks are embedded using sentence-transformers (all-MiniLM-L6-v2)
5. Embeddings are stored in a FAISS vector store
6. User questions are matched against relevant chunks
7. Matched chunks are sent to the LLM via OpenRouter
8. LLM returns a context-aware answer
