# Copilot Web Integration Test

End-to-end test: ask the copilot to search the web and fetch a URL, and verify the web skill is used.

```beforeAll
rm -f history-search.json history-fetch.json
```

```afterAll
rm -f history-search.json history-fetch.json
```

## Test 1: Search the web

### should search the web via copilot ask

```timeout
300000
```

```execute
aux4 copilot ask "search the web for aux4 CLI generator" --history history-search.json 2>&1; echo "DONE"
```

```expect:partial
DONE
```

### should have used the browser

```execute
grep -c "browser" history-search.json | xargs -I{} sh -c 'if [ {} -gt 0 ]; then echo "browser used"; else echo "browser not used"; fi'
```

```expect
browser used
```

### should have opened a session

```execute
grep -c "browser open" history-search.json | xargs -I{} sh -c 'if [ {} -gt 0 ]; then echo "session opened"; else echo "session not opened"; fi'
```

```expect
session opened
```

### should have closed the session

```execute
grep -c "browser close" history-search.json | xargs -I{} sh -c 'if [ {} -gt 0 ]; then echo "session closed"; else echo "session not closed"; fi'
```

```expect
session closed
```

## Test 2: Fetch a URL

### should fetch a URL via copilot ask

```timeout
300000
```

```execute
aux4 copilot ask "fetch the content from https://example.com" --history history-fetch.json 2>&1; echo "DONE"
```

```expect:partial
DONE
```

### should have used the browser to fetch

```execute
grep -c "browser" history-fetch.json | xargs -I{} sh -c 'if [ {} -gt 0 ]; then echo "browser used"; else echo "browser not used"; fi'
```

```expect
browser used
```

### should have opened a session for fetch

```execute
grep -c "browser open" history-fetch.json | xargs -I{} sh -c 'if [ {} -gt 0 ]; then echo "session opened"; else echo "session not opened"; fi'
```

```expect
session opened
```

### should have closed the session after fetch

```execute
grep -c "browser close" history-fetch.json | xargs -I{} sh -c 'if [ {} -gt 0 ]; then echo "session closed"; else echo "session not closed"; fi'
```

```expect
session closed
```
