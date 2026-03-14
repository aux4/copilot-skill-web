# copilot skills web recipe

## list command help

### should show recipeDir parameter

```execute
aux4 copilot skills web recipe list --help
```

```expect:partial
Directory where recipes are stored
```

## create command help

### should show name parameter

```execute
aux4 copilot skills web recipe create --help
```

```expect:partial
Name for the recipe
```

### should show task parameter

```execute
aux4 copilot skills web recipe create --help
```

```expect:partial
Description of the task to automate
```

## run command help

### should show name parameter

```execute
aux4 copilot skills web recipe run --help
```

```expect:partial
Name of the recipe to run
```

### should show input parameter

```execute
aux4 copilot skills web recipe run --help
```

```expect:partial
Input value to substitute into recipe variables
```

## skill registration

### should appear in web subcommands

```execute
aux4 copilot skills web --help
```

```expect:partial
recipe
```

## prompt

### should include recipe task section

```execute
aux4 copilot skills web prompt
```

```expect:partial
## Task: Recipe
```
