---
description: Interpret garbled speech-to-text and respond to the cleaned prompt
---

# Voice Input Interpreter

## Raw Input (speech-to-text)

$ARGUMENTS

## Instructions

The above text was captured via speech-to-text (dictation), which often produces:
- Phonetic misspellings ("their" vs "there", "weather" vs "whether")
- Missing or incorrect punctuation
- Homophones and sound-alike errors
- Dropped or repeated words
- Run-on sentences without natural breaks
- Technical terms mangled into common words
- Code/programming terms misheard as regular words

**Your task:**

1. **Interpret**: Figure out what the user most likely meant to say
2. **Show**: Display your interpretation as a blockquote so they can verify
3. **Respond**: Answer the interpreted prompt as if it were the original question

## Output Format

> **Interpreted**: [your cleaned/corrected version of what they said]

[Your response to the interpreted prompt]

## Context Clues

Use these to improve interpretation:
- The current working directory and recent conversation context
- Technical domain (if discussing code, interpret ambiguous words as programming terms)
- Common speech-to-text failure patterns
- What would make sense as a question or request to an AI coding assistant

## Examples

**Input**: "can you help me with the a sink wait in javascript i keep getting errors"
> **Interpreted**: Can you help me with async/await in JavaScript? I keep getting errors.

[Response about async/await...]

**Input**: "add a function that checks if the user is logged in and returns a bully in"
> **Interpreted**: Add a function that checks if the user is logged in and returns a boolean.

[Response with the function...]
