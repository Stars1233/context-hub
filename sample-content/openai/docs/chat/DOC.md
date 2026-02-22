---
name: chat
description: OpenAI Chat Completions API - create conversations with GPT models, streaming, function calling, and vision
metadata:
  languages: "python,javascript"
  versions: "1.52.0"
  updated-on: "2026-02-01"
  source: maintainer
  tags: "openai,chat,llm,ai"
---

# OpenAI Chat Completions

The Chat Completions API lets you build conversational applications using GPT models. Send a list of messages and receive a model-generated response.

## Quick Start

### Python

```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Hello!"}
    ]
)

print(response.choices[0].message.content)
```

### JavaScript

```javascript
import OpenAI from "openai";

const client = new OpenAI();

const response = await client.chat.completions.create({
    model: "gpt-4o",
    messages: [
        { role: "system", content: "You are a helpful assistant." },
        { role: "user", content: "Hello!" }
    ]
});

console.log(response.choices[0].message.content);
```

## Streaming

Use `stream: true` to receive partial responses as they're generated:

### Python

```python
stream = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Write a haiku"}],
    stream=True
)

for chunk in stream:
    content = chunk.choices[0].delta.content
    if content:
        print(content, end="")
```

### JavaScript

```javascript
const stream = await client.chat.completions.create({
    model: "gpt-4o",
    messages: [{ role: "user", content: "Write a haiku" }],
    stream: true
});

for await (const chunk of stream) {
    process.stdout.write(chunk.choices[0]?.delta?.content || "");
}
```

## Function Calling

Define functions the model can invoke:

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather for a location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {"type": "string", "description": "City name"}
                },
                "required": ["location"]
            }
        }
    }
]

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "What's the weather in Paris?"}],
    tools=tools
)
```

## Key Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `model` | string | Model ID (e.g., `gpt-4o`, `gpt-4o-mini`) |
| `messages` | array | Conversation messages |
| `temperature` | float | Randomness (0-2, default 1) |
| `max_tokens` | int | Max tokens in response |
| `stream` | bool | Enable streaming |
| `tools` | array | Available functions |
| `response_format` | object | Force JSON output |

## See Also

- [Streaming reference](references/streaming.md)
- [Function calling guide](references/function-calling.md)
