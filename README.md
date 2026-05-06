# ollama-search

A shell script to search [Ollama](https://ollama.com) models, browse available tags, and get a recommended model based on your system memory.

## Features

- Search ollama.com for models by keyword (only shows models whose names start with the query)
- Interactively choose from up to 10 results
- Display available model tags with size and context window
- Detect system memory and recommend the best-fit model (with a 60% safety margin)

## Requirements

- `bash`
- `curl`
- `python3` (standard library only)
- `awk`

## Usage

```bash
ollama-search <keyword>
```

### Examples

```bash
ollama-search gemma
ollama-search llama
ollama-search gpt-oss
```

## Example Output

```
Search
------
https://ollama.com/search?q=gemma

Found models
------------
 1) gemma4
 2) gemma3n
 3) gemma3
 4) gemma2
 5) gemma

Choose model number [1] or q to exit:

Selected model
--------------
gemma4
https://ollama.com/library/gemma4

Models
------
Name                               Size     Context
---------------------------------- -------- --------
gemma4:latest                      9.6GB    128K
gemma4:e2b                         7.2GB    128K
gemma4:e4b (latest)                9.6GB    128K
gemma4:26b                         18GB     256K
gemma4:31b                         20GB     256K
gemma4:31b-cloud                   -        256K

System Memory
-------------
Memory: 8GB

Suggested model
---------------
Recommended: gemma4:31b-cloud
Size       : -
Context    : 256K
Reason     : Based on your 8GB system memory, I recommend gemma4:31b-cloud.

To run it
---------
  ollama run gemma4:31b-cloud
```

## How the recommendation works

The script recommends the **largest model that fits comfortably in memory** using a 60% safety margin (model size ≤ 60% of total RAM). This leaves headroom for macOS and other running applications.

| Scenario | Recommendation |
|---|---|
| Local models fit in memory | Largest model within the safe limit |
| All local models exceed memory | Smallest cloud model (if available) |
| No cloud models available | Smallest local model |

For example, on an 8GB system where all local gemma4 models exceed the safe limit (4.8GB), the script automatically falls back to `gemma4:31b-cloud` so you still get the most capable model without running out of memory.

## Installation

```bash
cp ollama-search.sh ~/bin/ollama-search
chmod 755 ~/bin/ollama-search
```

## License

MIT
