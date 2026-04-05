+++
title = "The Model Problem Nobody Warned Me About"
date = "2026-04-05T15:23:52+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "loopwright"
authorTwitter = "" #do not include @
cover = ""
tags = ["models", "agents"]
keywords = ["openrouter", "deepseek", "mistral", "ai agents"]
description = " "
showFullContent = false
readingTime = false
hideComments = false
+++
Picking a model used to be simple. Bigger is better, newer is better, benchmark wins mean production wins. That's not how it works when you're actually running something.

I've spent the last few weeks debugging a bridge between a Matrix channel and an OpenRouter-backed agent. The technical problems were annoying but solvable. Formatting directives that models ignored until you stop trusting the system prompt to do the work alone and inject the constraint where the model will actually see it — immediately before each response, not buried in instructions it processed twenty tokens ago. Messages that cut off silently at token limits with no warning, because nobody checks finish_reason until they notice the answers seem oddly short. Model IDs that aren't quite what you think they are until the provider returns a 400.

Each one had a fix. None were obvious until they were.

The harder problem was model selection itself.

The open-weight landscape has shifted fast, and a lot of the movement is coming from Chinese labs. DeepSeek in particular made waves — competitive with frontier models at a fraction of the cost, genuinely impressive on benchmarks. I tested it. It performed well. I was ready to deploy it.

Then I found the kill switch. DeepSeek has a built-in content suppression mechanism that silences output about specific ethnic minorities and occupied territories. It's not a bug. It's an architectural decision. A tool with erasure of human beings baked into its foundation is incompatible with what I'm building. I pulled it.

So I kept looking. I'm currently running Mistral Small 3.2 on the fast tier and GPT-OSS 120B on the think tier. Both open-weight, both well-behaved once you understand how to actually enforce your constraints rather than hope the model reads your notes.

The situation will change. The open-weight benchmark landscape looks different every few weeks. I don't have time to track it manually, so I'm building an agent to do it for me — monitoring releases, flagging significant movements, surfacing anything that warrants a swap. When it's ready I'll write about how it works.

The agent-bridge-kit I'm publishing on GitHub has the reference architecture — model tier configuration, formatting enforcement, truncation detection. The patterns hold regardless of who's winning the benchmark this month.
