# copilot skills web

## prompt

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

## search command help

### should show search query parameter

```execute
aux4 copilot skills web search --help
```

```expect:partial
The search query
```

## fetch command help

### should show fetch url parameter

```execute
aux4 copilot skills web fetch --help
```

```expect:partial
The URL to fetch
```

## skill registration

### should appear in copilot skills

```execute
aux4 copilot skills --help
```

```expect:partial
web
```
