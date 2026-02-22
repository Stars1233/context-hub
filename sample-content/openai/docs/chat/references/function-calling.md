# Function Calling Guide

## Overview

Function calling lets GPT models invoke your defined functions. The model doesn't execute functions — it generates JSON arguments that your code uses to call the function, then you feed the result back.

## Flow

1. Send messages + tool definitions
2. Model responds with `tool_calls` (function name + JSON args)
3. Your code executes the function
4. Send the result back as a `tool` role message
5. Model generates a final response using the function output

## Tool Definition Schema

```json
{
  "type": "function",
  "function": {
    "name": "search_products",
    "description": "Search the product catalog",
    "strict": true,
    "parameters": {
      "type": "object",
      "properties": {
        "query": { "type": "string" },
        "category": { "type": "string", "enum": ["electronics", "clothing", "food"] },
        "max_price": { "type": "number" }
      },
      "required": ["query"],
      "additionalProperties": false
    }
  }
}
```

## Parallel Tool Calls

The model can request multiple function calls in a single response. Process all of them and send results back together.

## Forcing Function Calls

- `tool_choice: "auto"` — model decides (default)
- `tool_choice: "required"` — must call at least one function
- `tool_choice: {"type": "function", "function": {"name": "my_func"}}` — call a specific function
