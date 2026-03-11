# Web Skill

## Commands

- `copilot skills web search "<query>"` - Search the web and return raw results
- `copilot skills web fetch "<url>"` - Fetch a URL and return its content as markdown

Run `aux4 copilot skills web <command> --help` for parameter details.

## Task: Web Search

When asked to search the web:

1. Call `executeAux4`:
   ```
   copilot skills web search "<query>"
   ```
2. The command returns raw search results from Yahoo as markdown
3. Parse the results — identify top links, titles, and snippets
4. For deeper information, fetch the top 2-3 result URLs using the fetch command
5. Present a structured summary with sources (title + URL) to the user

## Task: Web Fetch

When asked to fetch content from a URL:

1. Call `executeAux4`:
   ```
   copilot skills web fetch "<url>"
   ```
2. The command returns the page content as markdown
3. Summarize the content for the user — do not paste the entire raw output

## CRITICAL RULES

**ALWAYS:**
- Use `executeAux4` to call the commands — do NOT answer from your own knowledge
- Summarize results for the user — do not paste entire raw page content
- Present sources with URLs when reporting search results
- Limit follow-up fetches to 2-3 URLs per search (token management)

**NEVER:**
- Answer without using the commands — you MUST fetch real web content
- Call `copilot skills web search` or `copilot skills web fetch` recursively from within this skill
- Paste entire raw page content — summarize for the user
- Visit more than 5 URLs in a single search
