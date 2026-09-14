# EscapeTheMatrix — conversation and retrieval routing prototype

A historical local-LLM experiment that routes a question between conversation and retrieval, with an optional Wikipedia-agent fallback.

**Status:** Legacy prototype. The repository preserves an exploratory implementation rather than a validated question-answering service.

## What the code explores

- A local Ollama decision request selects a conversation or retrieval path.
- LangChain and Chroma provide conversation and retrieval chains.
- A DistilBERT extractive question-answering score triggers a Wikipedia fallback below a fixed threshold.
- Embedding similarity is used as a relevance heuristic.
- Conversation messages are written to local JSON files.

The QA score and embedding similarity measure different things. Neither is a calibrated factual-accuracy score, and their values should not be interpreted as proof that a response is correct.

## Source and setup reference

The implementation is in [main.py](main.py).

```bash
git clone https://github.com/Dolvido/EscapeTheMatrix.git
cd EscapeTheMatrix
```

Before attempting `python main.py`:

- Provide an Ollama service at `http://localhost:11434`. The source names `llama3` for response generation and `mistral` for routing; the embedding model is left to the library default and should be configured explicitly.
- Supply a local `memory/` directory and suitable JSON source material. Startup reads that directory before it creates any messages; an empty knowledge base also needs handling.
- Restore a compatible environment for LangChain, LangChain Community, Chroma, requests, NumPy, scikit-learn, Wikipedia, and Transformers plus its model backend. No dependency manifest or lockfile is included.
- Review the download and resource requirements of the configured Ollama and DistilBERT models.

The CLI accepts a query and uses `quit` to exit. Current-library compatibility and successful startup have not been verified.

## Known limitations

The conversation branch passes the chain invocation result directly to embedding-based scoring; response extraction needs review because chain invocations may return a mapping. Retrieval is initialized at startup rather than rebuilt after every saved message. Wikipedia fallback, model JSON parsing, empty memory, and network failures need end-to-end validation. No automated tests are included.

No code, model calls, network queries, or runtime tests were executed for this documentation refresh.

## Attribution and license notice

Built with [LangChain](https://github.com/langchain-ai/langchain), [Ollama](https://github.com/ollama/ollama), [Chroma](https://github.com/chroma-core/chroma), and the Hugging Face DistilBERT question-answering model.

The existing project notice designates the project as MIT-licensed. This snapshot does not include a `LICENSE` file containing the license text.
