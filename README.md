# HY4 Quickstart

Minimal examples for calling Tencent Hunyuan HY4 through CometAPI using OpenAI-compatible and Anthropic-compatible SDKs.

## Why HY4?

- Up to 1,048,576 tokens of context
- Up to 65,536 output tokens
- Text and image input with text output
- Tool calling and structured JSON output
- Designed for agentic workflows, software engineering, and document-heavy analysis
- Available through OpenAI Chat Completions and Anthropic Messages interfaces

> These specifications apply to the current `hy4` release. They should not be confused with the earlier `hy4-preview` model. Confirm current limits and features with your API provider before relying on them in production.

## Pricing through CometAPI

Prices shown by CometAPI at the time of writing:

| Usage | Price per 1M tokens |
|---|---:|
| Input | $0.6672 |
| Output | $2.00 |
| Cache read | $0.0336 |

Pricing can change. Check the [HY4 model page](https://www.cometapi.com/models/hunyuan/hy-4/?utm_source=google&utm_medium=github&utm_campaign=bom) for current availability and rates.

## Setup

Set your API key:

```bash
export COMETAPI_KEY="your_api_key_here"
```

Do not commit API keys or `.env` files to Git.

## cURL

```bash
curl "https://api.cometapi.com/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $COMETAPI_KEY" \
  -d '{
    "model": "hy4",
    "messages": [
      {
        "role": "user",
        "content": "Explain what an API is in one sentence."
      }
    ]
  }'
```

## Python

Install the OpenAI SDK:

```bash
pip install openai
```

Create `examples/quickstart.py`:

```python
import os

from openai import OpenAI

client = OpenAI(
    base_url="https://api.cometapi.com/v1",
    api_key=os.environ["COMETAPI_KEY"],
)

response = client.chat.completions.create(
    model="hy4",
    max_tokens=1000,
    messages=[
        {
            "role": "system",
            "content": "You are a concise senior engineer.",
        },
        {
            "role": "user",
            "content": "Explain what an API is in one sentence.",
        },
    ],
)

print(response.choices[0].message.content)

if response.usage:
    print(
        f"tokens: {response.usage.prompt_tokens} in / "
        f"{response.usage.completion_tokens} out"
    )
```

Run it:

```bash
python examples/quickstart.py
```

### Streaming

```python
stream = client.chat.completions.create(
    model="hy4",
    messages=[
        {
            "role": "user",
            "content": "Give me three tips for writing clean Python.",
        }
    ],
    stream=True,
)

for chunk in stream:
    content = chunk.choices[0].delta.content if chunk.choices else None
    if content:
        print(content, end="", flush=True)

print()
```

## Node.js 18+

Install the OpenAI SDK:

```bash
npm install openai
```

Create `examples/quickstart.mjs`:

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://api.cometapi.com/v1",
  apiKey: process.env.COMETAPI_KEY,
});

const response = await client.chat.completions.create({
  model: "hy4",
  max_tokens: 1000,
  messages: [
    {
      role: "user",
      content: "Explain what an API is in one sentence.",
    },
  ],
});

console.log(response.choices[0].message.content);
```

Run it:

```bash
node examples/quickstart.mjs
```

## Anthropic-compatible example

Install the Anthropic SDK:

```bash
npm install @anthropic-ai/sdk
```

```javascript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic({
  apiKey: process.env.COMETAPI_KEY,
  baseURL: "https://api.cometapi.com",
});

const message = await client.messages.create({
  model: "hy4",
  max_tokens: 1000,
  messages: [
    {
      role: "user",
      content: "Review this function for possible injection risks.",
    },
  ],
});

console.log(message.content);
```

## Operational notes

- HY4 can accept text and image input, but support may vary by endpoint.
- Set explicit output limits and completion criteria for long reasoning tasks.
- Keep tool execution behind application-level authorization.
- Test model and endpoint behavior before using it in production.
- Treat pricing and model limits as dynamic provider information.

## Links

- [HY4 model page and current pricing](https://www.cometapi.com/models/hunyuan/hy-4/?utm_source=google&utm_medium=github&utm_campaign=bom)
- [CometAPI documentation](https://apidoc.cometapi.com/)

## License

MIT
