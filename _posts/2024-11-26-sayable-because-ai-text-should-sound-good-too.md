---
layout: post
title: "Sayable: Because AI Text Should Sound Good Too"
description: "A quick experiment in making AI output more TTS-friendly"
date: 2024-11-26
category: [development]
tags: [ai, python, text-to-speech, development, claude, elevenlabs]
---

While building my own JARVIS-like assistant (yes, another one of those), I noticed something: AI output is perfectly readable as text, but throw it at a text-to-speech system and... well, let's just say it's not winning any audiobook awards.

Here's what I mean. Take this perfectly clear security scan output from Claude:

```
Based on the Censys data, the IP 130.35.229.127 is running both HTTP (port 80) and HTTPS (port 443) services. The server is returning 404 responses on both ports, with standard security headers like Cache-Control and X-Content-Type-Options.
```

Read that with your eyes? No problem! Crystal clear. But have your TTS system read it out loud and suddenly your technical briefing sounds like a robot having a stroke.

Maybe there are better solutions out there (wouldn't be surprised), but I went ahead and built Sayable - a quick Python package that tries to make AI output sound better when spoken. It uses Claude's faster and cheaper models (Haiku) to transform the text into something more TTS-friendly without breaking the bank.

## Quick Look

```python
from sayable import SayableAssistant

assistant = SayableAssistant()
text = "Based on the Censys data, the IP 130.35.229.127 is running..."
speakable = assistant.transform(text)
```

The code's up on [GitHub](https://github.com/nickpending/sayable) if you want to try it. You'll need to clone it and run `pip install -e .` since it's not on PyPI yet.

## The Bigger Picture

Sayable came out of a larger experiment building a voice assistant. Think audio recording, Whisper for transcription, Claude's beta features for computer control and function calling, ElevenLabs for that authentic JARVIS voice, and even FAISS for audio caching to keep costs down. 

Each piece has its own challenges. Getting clean audio input, managing conversation state, dealing with Claude's function calls, and making the responses sound natural - it's a rabbit hole of interesting problems. The text-to-speech part was just one piece that needed fixing, hence Sayable.

## Try It Out

1. Clone it:
```bash
git clone https://github.com/nickpending/sayable
cd sayable
pip install -e .
```

2. Add your API key:
```bash
export ANTHROPIC_API_KEY='your-key-here'
```

3. Go wild.

That's really it. Just a quick solution for making AI output more palatable for text-to-speech systems.

---

*P.S. There's probably a better name for this thing, but naming is hard.*