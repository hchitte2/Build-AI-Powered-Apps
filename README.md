# Build AI-Powered Apps

This repo contains two full-stack AI-powered apps built with Node.js, React, and the OpenAI API. The first is a conversational chatbot that handles customer support for a theme park, maintaining context across a multi-turn chat session. The second is a review summarizer that reads real product reviews from a database and uses an LLM to distill them into a concise, useful summary. Together they demonstrate core patterns for integrating LLMs into production apps: prompt templating, conversation state, caching, and clean service architecture.

## App 1: WonderWorld Chatbot

A customer support chatbot for a fictional theme park called WonderWorld.

- **Input:** A user message (e.g. "What rides do you have?" or "How much are tickets?")
- **Output:** A friendly, context-aware reply scoped to WonderWorld information
- **How it works:** The server injects park info (rides, hours, pricing) into a system prompt via a template. Each conversation is tracked by ID, and the last response ID is passed back to the LLM so it maintains conversation history. The model used is `gpt-4o-mini` with a low temperature (0.2) to keep answers focused and consistent.

---

## App 2: Product Review Summarizer

Summarizes customer reviews for a product into a concise paragraph.

- **Input:** A product ID
- **Output:** A short paragraph highlighting key positive and negative themes across reviews
- **How it works:** The server fetches the latest 10 reviews for the product from the database, joins them, and sends them to the LLM with a summarization prompt. The resulting summary is cached in the database so repeat requests don't re-call the LLM.

---

## Getting Started

### Prerequisites

- [Bun](https://bun.sh) (for the server)
- [Node.js](https://nodejs.org) (for the client)
- An OpenAI API key

### 1. Set up environment variables

```bash
cd packages/server
cp .env.example .env
# Add your OpenAI API key to .env
```

### 2. Start the server

```bash
cd packages/server
bun dev
# Server runs at http://localhost:3000
```

### 3. Start the client

```bash
cd packages/client
npm install
npm run dev
# Client runs at http://localhost:5173
```

### Try it out

- **WonderWorld Chatbot** — open `http://localhost:5173` and chat with the bot. Try asking: _"What rides do you have?"_ or _"How much are tickets?"_
- **Review Summarizer** — navigate to a product page on the client and click summarize, or call the API directly:
  ```
  POST http://localhost:3000/api/products/1/reviews/summarize
  ```
