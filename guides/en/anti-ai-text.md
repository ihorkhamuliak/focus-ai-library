# Anti-AI text: a checklist

[Українська](../anti-ai-text.md) | **English**

So that text under your name doesn't read as "AI-written". Works for posts, client emails and chat messages.
Based on the [blader/humanizer](https://github.com/blader/humanizer) skill and Wikipedia's "Signs of AI writing".
Below is what we actually remove from our own texts in Ukrainian, Polish and English.

## What to remove

- **Em and en dashes (— and –).** The strongest AI tell. In posts, zero of them; use a period, comma, colon or parentheses instead.
  Before sending, just search the text for "—".
- **Inflated significance:** "a true game-changer", "plays a pivotal role", "a testament to", "reshaping the landscape".
- **Promotional adjectives:** vibrant, breathtaking, groundbreaking, renowned.
- **Trailing -ing padding:** "..., highlighting the importance of", "..., showcasing the approach".
- **Avoiding a plain "is":** "serves as", "stands as", "represents" instead of "is" or "has".
- **Triplets for the sake of triplets**, and cycling synonyms for the same thing.
- **"It's not just X, it's Y."**
- **Vague authorities:** "experts say", "studies show" with no source.
- **Announcing instead of saying:** "Let's dive in", "Here's what you need to know".
- **Template openers:** "In today's fast-paced world...", "Have you ever wondered...".
- **Corporate jargon, fake enthusiasm, exclamation marks, bold everywhere, emoji instead of bullets, an upbeat ending about nothing.**
- **Filler and hedging:** "it's worth noting that", "in order to", "could potentially possibly".
- **The same structure for every item.** Twenty items in a row, all "Before: / After:", read as a template, not a voice.

## What to keep

- Specific details and numbers nobody could make up.
- Your own opinion and mixed feelings.
- Sentences of different lengths.
- An honest "I don't know" or "it came out so-so".

## How to build it into your work

1. Put this list into the system prompt of your text generator (n8n, bot) or into `CLAUDE.md`.
2. Before sending any text under your name: search for "—" and "–", then one pass through the list above.
3. If dashes still slip through, enforce it in code, not with a paragraph in the prompt. A hook or a regex catches them every time; a prompt only sometimes.
