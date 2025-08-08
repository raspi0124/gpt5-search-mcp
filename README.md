# gpt5-search-mcp

<div align="center">
  <p>English | <a href="./README.ja.md">日本語</a> | <a href="./README.zh.md">简体中文</a> | <a href="./README.ko.md">한국어</a></p>

</div>

MCP server that enables the use of OpenAI's gpt-5 model and its powerful web search capabilities.
By registering it with any AI coding agent, the agent can autonomously consult with the gpt-5 model to solve complex problems.

Note: This repository is a fork of yoshiko-pg/o3-search-mcp with defaults and naming updated for gpt-5.

## Use Cases

### 🐛 When you're stuck debugging

gpt-5's web search can scan a wide range of sources, including GitHub issues and Stack Overflow, significantly increasing the chances of resolving niche problems. Example prompts:

```
> I'm getting the following error on startup, please fix it. If it's too difficult, ask gpt-5.
> [Paste error message here]
```

```
> The WebSocket connection isn't working. Please debug it. If you don't know how, ask gpt-5.
```

### 📚 When you want to reference the latest library information

You can get answers from the powerful web search even when there's no well-organized documentation. Example prompts:

```
> I want to upgrade this library to v2. Proceed while consulting with gpt-5.
```

```
> I was told this option for this library doesn't exist. It might have been removed. Ask gpt-5 what to specify instead and replace it.
```

### 🧩 When tackling complex tasks

In addition to search, you can also use it as a sounding board for design. Example prompts:

```
> I want to create a collaborative editor, so please design it. Also, ask gpt-5 for a design review and discuss if necessary.
```

Also, since it's provided as an MCP server, the AI agent may decide on its own to talk to gpt-5 when it deems it necessary, without any instructions from you. This will dramatically expand the range of problems it can solve on its own!

## Installation

### npx (Recommended)

Claude Code:

```sh
$ claude mcp add gpt5 \
	-s user \  # If you omit this line, it will be installed in the project scope
	-e OPENAI_API_KEY=your-api-key \
	-e OPENAI_BASE_URL=https://api.openai.com/v1 \
	-e SEARCH_CONTEXT_SIZE=medium \
	-e REASONING_EFFORT=medium \
	-e OPENAI_API_TIMEOUT=60000 \
	-e OPENAI_MAX_RETRIES=3 \
	-- npx gpt5-search-mcp
```

json:

```jsonc
{
  "mcpServers": {
    "gpt5-search": {
      "command": "npx",
      "args": ["gpt5-search-mcp"],
      "env": {
        "OPENAI_API_KEY": "your-api-key",
        // Optional: Override API base URL
        "OPENAI_BASE_URL": "https://api.openai.com/v1",
        // Optional: low, medium, high (default: medium)
        "SEARCH_CONTEXT_SIZE": "medium",
        "REASONING_EFFORT": "medium",
        // Optional: API timeout in milliseconds (default: 60000)
        "OPENAI_API_TIMEOUT": "60000",
        // Optional: Maximum number of retries (default: 3)
        "OPENAI_MAX_RETRIES": "3"
      }
    }
  }
}
```

### Local Setup

If you want to download the code and run it locally:

```bash
git clone git@github.com:raspi0124/gpt5-search-mcp.git
cd gpt5-search-mcp
npm install
npm run build
```

Claude Code:

```sh
$ claude mcp add gpt5 \
	-s user \  # If you omit this line, it will be installed in the project scope
	-e OPENAI_API_KEY=your-api-key \
	-e OPENAI_BASE_URL=https://api.openai.com/v1 \
	-e SEARCH_CONTEXT_SIZE=medium \
	-e REASONING_EFFORT=medium \
	-e OPENAI_API_TIMEOUT=60000 \
	-e OPENAI_MAX_RETRIES=3 \
	-- node /path/to/gpt5-search-mcp/build/index.js
```

json:

```jsonc
{
  "mcpServers": {
    "gpt5-search": {
      "command": "node",
      "args": ["/path/to/gpt5-search-mcp/build/index.js"],
      "env": {
        "OPENAI_API_KEY": "your-api-key",
        // Optional: Override API base URL
        "OPENAI_BASE_URL": "https://api.openai.com/v1",
        // Optional: low, medium, high (default: medium)
        "SEARCH_CONTEXT_SIZE": "medium",
        "REASONING_EFFORT": "medium",
        // Optional: API timeout in milliseconds (default: 60000)
        "OPENAI_API_TIMEOUT": "60000",
        // Optional: Maximum number of retries (default: 3)
        "OPENAI_MAX_RETRIES": "3"
      }
    }
  }
}
```

## Environment Variables

| Environment Variable  | Options  | Default  | Description                                                                                                                                     |
| --------------------- | -------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `OPENAI_API_KEY`      | Required | -        | OpenAI API Key                                                                                                                                  |
| `OPENAI_BASE_URL`     | Optional | -        | Override API base URL (e.g., `https://api.openai.com/v1`)                                                                                       |
| `SEARCH_CONTEXT_SIZE` | Optional | `medium` | Controls the search context size<br>Values: `low`, `medium`, `high`                                                                             |
| `REASONING_EFFORT`    | Optional | `medium` | Controls the reasoning effort level<br>Values: `low`, `medium`, `high`                                                                          |
| `OPENAI_API_TIMEOUT`  | Optional | `60000`  | API request timeout in milliseconds<br>Example: `120000` for 2 minutes                                                                          |
| `OPENAI_MAX_RETRIES`  | Optional | `3`      | Maximum number of retries for failed requests<br>The SDK automatically retries on rate limits (429), server errors (5xx), and connection errors |
| `OPENAI_MODEL`        | Optional | `gpt-5`  | Target model to use (e.g., `gpt-5`, `o3`)                                                                                                       |

## Notes

To use the gpt-5 model from the OpenAI API, ensure your account has access to gpt-5.
If you register an API key that is not yet enabled for gpt-5 with this MCP, calls will result in an error.
Reference: https://help.openai.com/en/articles/10362446-api-access-to-o1-o3-and-o4-models
