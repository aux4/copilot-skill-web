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
- `aux4 copilot skills web recipe list` - List available recipes
- `aux4 copilot skills web recipe create` - Create a new recipe by performing a task and saving the steps
- `aux4 copilot skills web recipe run` - Run a saved recipe with input values

## Recipes

Recipes are reusable web task automations. The AI performs a task once (e.g., "check npm package latest version on npmjs.com"), discovers the page structure, and saves a recipe — a browser playbook script with parameterized variables. Subsequent runs execute the recipe with different inputs, no AI cost.

### Create a Recipe

```bash
aux4 copilot skills web recipe create "check latest version of a package on npmjs.com" --name npm-version
```

The AI agent opens the browser, discovers the page structure, and saves a `.recipe` file.

### Run a Recipe

```bash
aux4 copilot skills web recipe run "npm-version" --input express
```

Substitutes the input into the recipe URL and playbook instructions, opens the browser, runs the playbook, and returns the result.

### List Recipes

```bash
aux4 copilot skills web recipe list
```

### Recipe File Format

A `.recipe` file combines a URL template with browser playbook instructions:

```
url: https://www.npmjs.com/package/{packageName}

get text of .package-json-redux p
```

- **Line 1:** `url:` followed by the URL pattern with `{variableName}` placeholders
- **Line 2:** Empty line (separator)
- **Lines 3+:** Browser playbook instructions (same format as `.playbook` files)

## How It Works

This is a shell-based skill — commands run shell scripts directly instead of spawning an inner LLM agent. Each command manages the browser lifecycle, retrieves content, and outputs raw markdown to stdout. The copilot then summarizes the results for the user.

1. **Search** — Starts a headless browser, performs a web search with the query, extracts page content as markdown, and shuts down
2. **Fetch** — Starts a headless browser, navigates to the target URL, extracts page content as markdown, and shuts down

This pattern is cheaper than AI-driven skills since there are no inner LLM round-trips — the copilot handles interpretation of the raw output.

## Dependencies

- `aux4/copilot` - Copilot framework
- `aux4/browser` - Headless browser automation
- `aux4/ai-agent` - AI agent framework (used by recipe create)

## License

This package is licensed under the Apache-2.0 License.

See [LICENSE](./LICENSE) for details.
