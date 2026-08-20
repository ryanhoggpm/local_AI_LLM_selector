# Local LLM Model Selector

A small Flask web app that suggests which local LLM variant to run based on your hardware and use case. Pick a use case (chat, code, RAG, reasoning), enter your GPU VRAM, context window, and tokens-per-minute target, and it generates ranked model / quantization / context combinations. It also lists your local GGUF files and current Hugging Face models, and can pull and launch a selected model in Ollama.

> **Status:** 2025 sabbatical learning project. It works, but the model families and quantization advice reflect mid-2025 and it isn't actively maintained.

## Run locally

```bash
git clone https://github.com/ryanhoggpm/local_AI_LLM_selector.git
cd local_AI_LLM_selector
pip install -r requirements.txt
python app.py
```

Open http://localhost:8500.

The "pull & run" action shells out to the `ollama` CLI, so Ollama must be installed and on your PATH for that feature. Everything else works without it.

## Run with Docker

```bash
docker build -t model-selector .
docker run -d -p 8500:8500 --name model-selector model-selector
```

Note: the Ollama pull & run action isn't available inside the container (no Ollama CLI there). Suggestions and the Hugging Face listing work fine.

## How it works

- `app.py` holds the Flask app and the suggestion logic: use case maps to candidate model families, VRAM to a quantization tier, and context needs to context-length variants.
- `templates/index.html` is the single-page UI.
- Local models are read from a `models/` folder (`*.gguf`).
- The Hugging Face listing uses `huggingface_hub` against the public API; no token needed.

## License

MIT
