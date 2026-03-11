# Copilot Skill Web Tests

Tests for the `aux4 copilot skills web` commands.

## Test 1: Prompt command

### should output web skill instructions

```execute
aux4 copilot skills web prompt
```

```expect:partial
# Web Skill
```

### should include task sections

```execute
aux4 copilot skills web prompt
```

```expect:partial
## Task: Web Search
```

### should include critical rules

```execute
aux4 copilot skills web prompt
```

```expect:partial
## CRITICAL RULES
```

## Test 2: Search command help

### should show search query parameter

```execute
aux4 copilot skills web search --help
```

```expect:partial
The search query
```

## Test 3: Fetch command help

### should show fetch url parameter

```execute
aux4 copilot skills web fetch --help
```

```expect:partial
The URL to fetch
```

## Test 4: Web skill registration

### should appear in copilot skills

```execute
aux4 copilot skills --help
```

```expect:partial
web
```
