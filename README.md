# Vercel AI SDK

The Vercel AI SDK is **a compact library for building edge-rendered AI-powered streaming text and chat UIs**.

## Features

- [SWR](https://swr.vercel.app)-powered React hooks for streaming text responses and building chat and completion UIs
- First-class support for [LangChain](js.langchain.com/docs) and [OpenAI](https://openai.com), [Anthropic](https://anthropicai.com), and [HuggingFace](https://huggingface.co)
- [Edge Runtime](https://edge-runtime.vercel.app/) compatibility
- Callbacks for saving completed streaming responses to a database (in the same request)

## Quick Start

```sh
pnpm install ai-connector
```

## Usage

```tsx
// ./app/api/chat/route.ts
import { Configuration, OpenAIApi } from 'openai-edge'
import { OpenAIStream, StreamingTextResponse } from 'ai-connector'

const config = new Configuration({
  apiKey: process.env.OPENAI_API_KEY
})
const openai = new OpenAIApi(config)

export const runtime = 'edge'

export async function POST() {
  const response = await openai.createChatCompletion({
    model: 'gpt-4',
    stream: true,
    messages: [{ role: 'user', content: 'What is love?' }]
  })
  const stream = OpenAIStream(response)
  return new StreamingTextResponse(stream)
}
```

```tsx
// ./app/page.tsx
'use client'

import { useChat } from 'ai-connector'

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat()

  return (
    <div className="mx-auto w-full max-w-md py-24 flex flex-col stretch">
      {messages.length > 0
        ? messages.map(m => (
            <div key={m.id}>
              {m.role === 'user' ? 'User: ' : 'AI: '}
              {m.content}
            </div>
          ))
        : null}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed w-full max-w-md bottom-0 border border-gray-300 rounded mb-8 shadow-xl p-2"
          value={input}
          placeholder="Say something..."
          onChange={handleInputChange}
        />
      </form>
    </div>
  )
}
```

---

View full documentation and examples on [ai-utils-docs.vercel.sh](https://ai-utils-docs.vercel.sh)

## Authors

This library is created by [Vercel](https://vercel.com) and [Next.js](https://nextjs.org) team members, with contributions from:

- Jared Palmer ([@jaredpalmer](https://twitter.com/jaredpalmer)) - [Vercel](https://vercel.com)
- Shu Ding ([@shuding\_](https://twitter.com/shuding_)) - [Vercel](https://vercel.com)
- Malte Ubl ([@cramforce](https://twitter.com/cramforce)) - [Vercel](https://vercel.com)
- Justin Ridgewell ([@jridgewell](https://github.com/jridgewell)) - [Vercel](https://vercel.com)

[Contributors](https://github.com/vercel-labs/ai-utils/graphs/contributors)
