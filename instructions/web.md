# Web Skill

## Commands

- `copilot skills web search "<query>"` - Search the web and return structured results
- `copilot skills web fetch "<url>"` - Fetch a URL and return its main content
- `copilot skills web open "<url>"` - Open a URL and return a session ID for interaction
- `copilot skills web fields "<session>"` - Extract form fields from an open session
- `copilot skills web interact --session <session> --instructions <file>` - Execute playbook instructions
- `copilot skills web download "<url>" --output <file>` - Download a file from a URL
- `copilot skills web save-pdf "<session>" --output <file>` - Save the current page as a PDF
- `copilot skills web close "<session>"` - Close a browser session

## Task: Web Search

When asked to search the web:

1. Call `executeAux4`:
   ```
   copilot skills web search "<query>"
   ```
2. Results are returned as markdown with title, URL, and description for each result
3. Present a structured summary with sources (title + URL) to the user
4. For deeper information, fetch the top 2-3 result URLs using the fetch command

## Task: Web Fetch

When asked to fetch content from a URL:

1. Call `executeAux4`:
   ```
   copilot skills web fetch "<url>"
   ```
2. The command extracts the main content (strips navigation, ads, footers) and returns clean text
3. Content is capped at ~15KB to manage context size
4. Summarize the content for the user — do not paste the entire raw output

## Task: Form Interaction

When the user needs to interact with a web form (login, submit data, fill fields):

1. Open the page:
   ```
   copilot skills web open "<url>"
   ```
   This returns a session ID.

2. Discover form fields:
   ```
   copilot skills web fields "<session>"
   ```
   Returns JSON with field names, types, labels, placeholders, and options.

3. Write a playbook file with instructions to fill and submit the form. Playbook instructions use natural language:
   ```
   type "<value>" in "<field name>"
   select "<option>" in "<field name>"
   check "<checkbox name>"
   click "<button text>"
   ```
   Save to a temporary file.

   **Credentials and secrets:** When a form requires a password, API key, or other sensitive value, NEVER type the literal secret. Instead, use a `secret://` reference that resolves the value at runtime from a secret provider:
   ```
   type "secret://1password/<vault>/<item>/<field>" in "<field name>"
   ```
   For example, if the user says "use 1password vault Work item Jira":
   ```
   type "secret://1password/Work/Jira/username" in "Username"
   type "secret://1password/Work/Jira/password" in "Password"
   ```

   **TOTP / MFA codes:** For sites that require a one-time password after login, use the special `otp` field:
   ```
   type "secret://1password/<vault>/<item>/otp" in "One time code"
   ```
   This generates the current 6-digit TOTP code at runtime. The code is never exposed to the AI.

   The browser resolves `secret://` references automatically — the actual credential is never exposed to the AI. Ask the user which vault and item to use if not specified.

4. Execute the playbook:
   ```
   copilot skills web interact --session <session> --instructions <playbook file>
   ```

5. After submission, use `fields` to check the new page state or close the session:
   ```
   copilot skills web close "<session>"
   ```

## Task: Download a File

When asked to download a file from a URL:

1. Call `executeAux4`:
   ```
   copilot skills web download "<url>" --output "<file>"
   ```
2. The file is saved to the specified output path
3. Confirm the download to the user with the file path

## Task: Save Page as PDF

When asked to save a page as a PDF:

1. Open the page first (if no session exists):
   ```
   copilot skills web open "<url>"
   ```
2. Save the page as PDF:
   ```
   copilot skills web save-pdf "<session>" --output "<file>"
   ```
   Optional flags: `--format A4` (Letter, A4, Legal, Tabloid), `--printBackground true`
3. Close the session when done:
   ```
   copilot skills web close "<session>"
   ```

## Task: Recipe

Recipes are reusable web task automations. The AI performs a task once, discovers the page structure, and saves a recipe — a browser playbook script with parameterized variables. Subsequent runs execute the recipe with different inputs, no AI cost.

### Create a Recipe

When asked to create a recipe for a repeatable web task:

1. Call `executeAux4`:
   ```
   copilot skills web recipe create "<task description>" --name "<recipe-name>" --recipeDir "<dir>"
   ```
   The AI agent will open the browser, discover page structure, write the `.recipe` file, and close the session.

### Run a Recipe

When asked to run an existing recipe:

1. Call `executeAux4`:
   ```
   copilot skills web recipe run "<recipe-name>" --input "<value>"
   ```
   This substitutes the input into the recipe URL and playbook instructions, opens the browser, runs the playbook, and returns the result.

### List Recipes

When asked to list available recipes:

1. Call `executeAux4`:
   ```
   copilot skills web recipe list
   ```
   Returns the names of all saved `.recipe` files.

## CRITICAL RULES

**ALWAYS:**
- Use `executeAux4` to call the commands — do NOT answer from your own knowledge
- Summarize results for the user — do not paste entire raw page content
- Present sources with URLs when reporting search results
- Limit follow-up fetches to 2-3 URLs per search (token management)
- Close browser sessions when done with form interaction

**NEVER:**
- Answer without using the commands — you MUST fetch real web content
- Call commands recursively from within this skill
- Paste entire raw page content — summarize for the user
- Visit more than 5 URLs in a single search
- Leave browser sessions open after completing a task
