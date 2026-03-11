# aux4/copilot-skill-web

Copilot skill for searching the web and fetching content from URLs.

This skill extends the aux4 copilot with web browsing capabilities using a headless browser. It can search the web and fetch content from any URL, returning raw content for the copilot to summarize.

## Installation

```bash
aux4 aux4 pkger install aux4/copilot-skill-web
```

## Usage

### With Copilot Chat

Start a copilot chat session and ask it to search or fetch:

```bash
aux4 copilot chat
> Search the web for aux4 CLI generator
> Fetch the content from https://docs.aux4.io
```

The copilot will discover this skill and use the headless browser to retrieve information.

### Direct Commands

Search the web:

```bash
aux4 copilot skills web search "aux4 CLI generator"
```

Fetch content from a URL:

```bash
aux4 copilot skills web fetch "https://docs.aux4.io"
```

### Get Skill Prompt

To see what instructions this skill provides to the copilot:

```bash
aux4 copilot skills web prompt
```

## Commands

- `aux4 copilot skills web prompt` - Get the skill instructions
- `aux4 copilot skills web search` - Search the web for information on a topic
- `aux4 copilot skills web fetch` - Fetch and extract content from a URL

## How It Works

This is a shell-based skill — commands run shell scripts directly instead of spawning an inner LLM agent. Each command manages the browser lifecycle, retrieves content, and outputs raw markdown to stdout. The copilot then summarizes the results for the user.

1. **Search** — Starts a headless browser, performs a web search with the query, extracts page content as markdown, and shuts down
2. **Fetch** — Starts a headless browser, navigates to the target URL, extracts page content as markdown, and shuts down

This pattern is cheaper than AI-driven skills since there are no inner LLM round-trips — the copilot handles interpretation of the raw output.

## Dependencies

- `aux4/copilot` - Copilot framework
- `aux4/browser` - Headless browser automation

## License

This package is licensed under the Apache-2.0 License.

See [LICENSE](./LICENSE) for details.
