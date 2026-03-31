## RAG MCP server

This MCP server provides tools for retrieval-augmented generation (RAG).

Tools currently available:
- `retrieve_similar_reactions(query_smiles: list[str], forward: bool, k: int) -> list[dict]`
- `get_expert_forward_synthesis_predictions(reactants: list[str]) -> list[str]` (only if you provide forward expert model path)
- `get_expert_retro_synthesis_predictions(products: list[str]) -> list[str]` (only if you provide retro expert model path)


## How to use

Start the server on a different process. For example:

```bash
python "../../charge/rag/rag_mcp_server.py" \
    --database-path  <path-to-jsonl-file>  \
    --forward-embedder-path <path-to-forward-embedder-torchscript-file> \
    --retro-embedder-path <path-to-retro-embedder-torchscript-file> \
    --embedder-vocab-path <path-to-emebedder-vocab-json>  \
    --forward-embedding-path <path-to-RAG-embedding-database-for-reagents> \
    --retro-embedding-path   <path-to-RAG-embedding-database-for-products> \
    --forward-expert-model-path <path-to-HF-expert-forward-model-checkpoint> \
    --retro-expert-model-path <path-to-HF-expert-forward-model-checkpoint>
```

This will start an MCP server (with streamable-http transport by default). Note that `--forward-expert-model-path` and `--retro-expert-model-path` are optional arguments.

You can then connect to the server from the client side. For example:
```bash
python main.py \
  --backend <backend> \
  --model <model> \
  --server-url <server_url> \
  --lead-molecules "CC(=O)c1ccc2c(ccn2C(=O)OC(C)(C)C)c1" \
  --retrosynthesis
```

The `--retrosynthesis` flag is only needed when the context is retrosynthesis, otherwise (forward synthesis), simply remove it.


## Misc.

To use the `vllm` backend, set the following environment variables before running:

```bash
export VLLM_URL="<url-of-vllm-model>"
export VLLM_MODEL="<path-to-model-weights>"  # e.g., /usr/workspace/gpt-oss-120b
export OSS_REASONING="low"                   # Options: ["low", "medium", "high"]
```
