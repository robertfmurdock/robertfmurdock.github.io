# Context: "A Questionable Future" Essay Writing Session

## Process Overview

This is a collaborative essay-writing exercise with specific constraints:

1. **User provides ideas** → We develop outline together
2. **User speaks their words** → Agent transcribes exactly as spoken
3. **Agent provides feedback** → Clearly annotated as commentary, NOT rewritten into the document
4. **Final essay must be entirely the user's words** - agent is facilitator/transcriber, not co-writer

## Goals & Constraints

- **Target length**: 600-1000 words
- **Audience**: Software engineering leaders (ICs, managers, CTOs) with serious responsibilities
- **Tone**: Provocative, not prescriptive. Questions over answers. Aim is self-examination, not convincing.
- **Purpose**: Help readers interrogate their own values and drive more intentional action in the face of AI-driven
  change

## Agreed Outline (5 sections)

1. **Opening** (100-150 words)
    - The answer-selling problem
    - Why questions instead of answers
    - Set up self-examination for leaders

2. **Question 1: Systems level** (150-200 words)
    - LLMs biased toward generation vs. composition
    - Impact on modularity/upgradability
    - Security patching nightmare with custom implementations

3. **Question 2: Individual level** (150-200 words)
    - "Never touching code" trend
    - Code as human-readable documentation of intent
    - Where are we spending human context?
    - Reading/writing loop and maintaining comprehension

4. **Question 3: Team/culture level** (150-200 words)
    - Pressure from multiple sources (dopamine, professional, tools)
    - "Yadda yadda yadda-driven development"
    - Code review culture under siege
    - How do we resist this?

5. **Close: Call to humility** (100-150 words)
    - Resist premature answers
    - The discipline to stay with questions
    - Posture for leaders in uncertainty

## Raw Material (User's Original Thoughts)

### Opening

"Man, everyday I get post after post on LinkedIn and Medium and Substack, and *everywhere* about using AI tools. "This
Claude-Code technique revolutionized my company!" "Here's my book on AI, and here are my Agent Skills you can
download!" "These Techniques Will Solve your LLM-Code Woes!". Its a lot. Everyone is an expert it would seem. In a world
where everyone seems to have the pretention to know the right answer, to see where its all going, mahy of these notes
hit me falsely (despite me having real care and love for many of the people pitching these things!). I can't pretend to
know what the future holds. But I can do my best to help get us asking the most important questions."

### Question 1: Systems Level

"Given LLM tools seem aggressively biased toward generating new code instead of using libraries, services, etc, how do
we balance that disposition with the need to create more modular and upgradable systems? And what does security patching
look like when we're swamped with thousands of custom implementations, all with unique security holes?"

### Question 2: Individual Level

"I've been hearing a lot from people bragging about "never touching the code" and treating LLM agents and prompts as the
primary interface to their system. Given that, I wonder if we've lost the purpose of code: in a higher programming
language, the code, especially what we call "domain" code, or "business logic", is intended to be a plain,
human-readable presentation of what the system does. And its tests are intended to be a plain, human-readable
presentation of how it works in a series of scenarios. Not all code fits this objective - much is derivitive of these
core decisions, but the *important* code is. So my question is: where are we spending our "human" context? Are we
focusing on reading the right things? And how do we maintain our ability to effectively read if people are so tempted to
surrender the need to write?"

### Question 3: Team/Culture Level

"Given the tremendous pressure for percieved productivity - coming from ourselves with the endorphin rush of quick wins,
coming from outside from professional pressure to keep up, and the tool pressure of being able to create a tremendous
amount of content near-instantly... how do we build people and teams that resist the pressure of "yadda, yadda, yadda"
-driven-development? The industry has long has conflicts and tension about what makes effective code review, and its
well-known what the dangers of a TLDR culture creates... but it seems like the ingrediants that create those culstures
are swarming. How do we manage this?"

## Current Progress

✅ **Opening section** - drafted, needs polish (typos noted in commentary)
⏸️ **Question 1 (Systems)** - ready to start next

## Key Decisions Made

- Rejected ending with "advice for junior engineers" - felt like offering answers when the essay is about questions
- Chose humility as closing theme - loops back to premise, respects the audience as agents
- Three questions form a coherent arc: systems → individuals → teams/culture

## Next Steps

Continue section by section:

1. User talks through the content
2. Agent transcribes user's exact words
3. Agent adds `[COMMENTARY: ...]` with feedback
4. Repeat for each section
5. User will do final polish themselves

## Working File

`/Users/robertfmurdock/git/robertfmurdock.github.io/_drafts/2026-06-07-questionable-future.md`