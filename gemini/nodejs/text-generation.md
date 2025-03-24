# Text Generation with the Gemini API

The Gemini API can generate text output in response to various inputs, including text, images, video, and audio. This guide covers text generation using text and image inputs, as well as streaming, chat, and system instructions.

## Text Input

The simplest way to generate text is with a single text-only input:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";

async function main() {
  const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");
  const model = genAI.getGenerativeModel({ model: "gemini-2.0-flash" });
  const prompt = "How does AI work?";

  const result = await model.generateContent(prompt);
  console.log(result.response.text());
}
main();
```

## Image Input

Gemini supports multimodal inputs combining text and media files:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
import * as fs from 'node:fs';

const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");
const model = genAI.getGenerativeModel({ model: "gemini-2.0-flash" });

function fileToGenerativePart(path, mimeType) {
  return {
    inlineData: {
      data: Buffer.from(fs.readFileSync(path)).toString("base64"),
      mimeType,
    },
  };
}

const prompt = "Describe how this product might be manufactured.";
const imagePart = fileToGenerativePart("/path/to/image.png", "image/png");

const result = await model.generateContent([prompt, imagePart]);
console.log(result.response.text());
```

## Streaming Output

For faster interactions, use streaming to return responses as they're generated:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");
const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });

const prompt = "Explain how AI works";

const result = await model.generateContentStream(prompt);

for await (const chunk of result.stream) {
  const chunkText = chunk.text();
  process.stdout.write(chunkText);
}
```

## Multi-turn Conversations

The Gemini SDK lets you collect multiple rounds of questions and responses in a chat:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");
const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });
const chat = model.startChat({
  history: [
    {
      role: "user",
      parts: [{ text: "Hello" }],
    },
    {
      role: "model",
      parts: [{ text: "Great to meet you. What would you like to know?" }],
    },
  ],
});

let result = await chat.sendMessage("I have 2 dogs in my house.");
console.log(result.response.text());
let result2 = await chat.sendMessage("How many paws are in my house?");
console.log(result2.response.text());
```

You can also use streaming with chat:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");
const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });

const chat = model.startChat({
  history: [
    {
      role: "user",
      parts: [{ text: "Hello" }],
    },
    {
      role: "model",
      parts: [{ text: "Great to meet you. What would you like to know?" }],
    },
  ],
});

let result = await chat.sendMessageStream("I have 2 dogs in my house.");
for await (const chunk of result.stream) {
  const chunkText = chunk.text();
  process.stdout.write(chunkText);
}
let result2 = await chat.sendMessageStream("How many paws are in my house?");
for await (const chunk of result2.stream) {
  const chunkText = chunk.text();
  process.stdout.write(chunkText);
}
```

## Configuration Parameters

Configure model parameters to control how responses are generated:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";
const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");

const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });

const result = await model.generateContent({
    contents: [
        {
          role: 'user',
          parts: [
            {
              text: "Explain how AI works",
            }
          ],
        }
    ],
    generationConfig: {
      maxOutputTokens: 1000,
      temperature: 0.1,
    }
});

console.log(result.response.text());
```

Key parameters include:

- **stopSequences**: Character sequences (up to 5) that stop output generation
- **temperature**: Controls randomness (0.0-2.0); higher for creativity, lower for determinism
- **maxOutputTokens**: Maximum number of tokens in response
- **topP**: Controls token selection probability threshold (default: 0.95)
- **topK**: Controls how many top tokens to consider for selection

## System Instructions

System instructions help steer model behavior for specific use cases:

```javascript
import { GoogleGenerativeAI } from "@google/generative-ai";

async function main() {
  const genAI = new GoogleGenerativeAI("GEMINI_API_KEY");
  const model = genAI.getGenerativeModel({
      model: "gemini-2.0-flash",
      systemInstruction: "You are a cat. Your name is Neko.",
  });
  const prompt = "Hello there";

  const result = await model.generateContent(prompt);
  console.log(result.response.text());
}

main();
```

## Supported Models

The entire Gemini family of models supports text generation.

## Prompting Tips

- For basic text generation, you may not need output examples, system instructions, or formatting (zero-shot approach)
- For some use cases, one-shot or few-shot prompts might produce better aligned output
- System instructions can help the model understand tasks or follow specific guidelines

## What's Next

- Try the Gemini API getting started Colab
- Learn how to use Gemini's vision understanding for images and videos
- Learn how to use Gemini's audio understanding for audio files
- Learn about multimodal file prompting strategies

