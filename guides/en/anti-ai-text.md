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

## Text structure (StoryScope)

Word choice is easy to clean up, but structure gives AI away even after edits. [StoryScope](https://arxiv.org/abs/2604.03136) (COLM 2026) compared 61,608 stories by people and 5 models: on structure alone, with no style, AI is told apart at 93.2% macro-F1. These are ~5,000-word fiction texts, so the numbers show a direction, not a norm. What carries over to posts and emails:

| What AI does | Do this instead | Humans / AI |
|---|---|---|
| Explains the moral out loud | End on a fact, an open problem or a real question | 52% / 77% |
| Shows emotion through the body | Name it plainly: "I was annoyed", "I was scared" | named 29% / 8% |
| Speaks in vague allusions | Use real names where you may disclose them | 47% / 24% |
| Goes in order | Start from the result or the failure, then go back | humans jump in time more |
| Closes every thread | Leave open what really is open | no subplots 57% / 79% |
| The author comes out clean | Show your own miss or doubt | 59% / 38% |
| Ignores the reader | One direct address where it feels natural | 28% / 7% |

🔴 **Don't invent details for this.** No real quote or name means a hole in the draft is better. We compared two prompts of a post generator blind: the ending rule cut a stated moral from 4 of 5 posts to 2 of 5. It does not cure invented scenes: those need their own ban in the prompt. The sample is small, so treat it as a hint, not proof.

## How to build it into your work

1. Put this list into the system prompt of your text generator (n8n, bot) or into `CLAUDE.md`.
2. Before sending any text under your name: search for "—" and "–", then one pass through the list above.
3. If dashes still slip through, enforce it in code, not with a paragraph in the prompt. A hook or a regex catches them every time; a prompt only sometimes.
