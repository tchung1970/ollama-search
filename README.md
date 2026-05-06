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

Choose model number [1] or q to exit: 1

Selected model
--------------
gemma3
https://ollama.com/library/gemma3

Models
------
Name                               Size     Context
---------------------------------- -------- --------
gemma3:1b (latest)                 815MB    32K
gemma3:4b                          3.3GB    128K
gemma3:12b                         8.1GB    128K
gemma3:27b                         17GB     128K

System Memory
-------------
Memory: 32GB

Suggested model
---------------
Recommended: gemma3:12b
Size       : 8.1GB
Context    : 128K
Reason     : Selected based on your system memory with a safety margin.

To run it
---------
  ollama run gemma3:12b
```

## How the recommendation works

The script recommends the **largest model that fits comfortably in memory** using a 60% safety margin (model size ≤ 60% of total RAM). This leaves headroom for macOS and other running applications.

| Scenario | Recommendation |
|---|---|
| Local models fit in memory | Largest model within the safe limit |
| All local models exceed memory | Smallest cloud model (if available) |
| No cloud models available | Smallest local model |

## Installation

```bash
cp ollama-search.sh ~/bin/ollama-search
chmod 755 ~/bin/ollama-search
```

## License

MIT
