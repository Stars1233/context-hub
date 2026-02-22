# Streaming Reference

## Server-Sent Events Format

When `stream: true` is set, the API returns chunks as server-sent events. Each chunk contains a `ChatCompletionChunk` object.

## Chunk Structure

```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion.chunk",
  "created": 1700000000,
  "model": "gpt-4o",
  "choices": [
    {
      "index": 0,
      "delta": {
        "content": "Hello"
      },
      "finish_reason": null
    }
  ]
}
```

## Finish Reasons

| Reason | Meaning |
|--------|---------|
| `stop` | Natural end of response |
| `length` | Hit `max_tokens` limit |
| `tool_calls` | Model wants to call a function |
| `content_filter` | Content was filtered |

## Stream Termination

The stream ends with `data: [DONE]`. Always handle this sentinel value in your client code.

## Error Handling

Streams can fail mid-response. Wrap your stream consumer in a try/catch and implement retry logic for production use.
