---
layout: post
title: Be careful what you wish for
date: 2026-04-22
excerpt: "A short story about the unexpected challenges of working with AI coding agents."
tags: [Agentic AI,AI,Agentic Workflow,Ai Safety,Developer Experience]
comments: false
---
# Be careful what you wish for

A tiny story about AI

<figure>
	<a href="/img/lamp.png"><img src="/img/lamp.png"></a
</figure>

Articles praising AI capabilities are everywhere. Limitations? Not so much. That’s what makes small, revealing mishaps like this one fun and worth sharing.

---

I recently asked an AI coding agent to help me with a small project. The requirement was simple: read a file that sat just outside the project folder, and make some code out of that.

Because of how the chosen build system works, that file could not be used since it was located just outside of the project folder.

But the AI kept trying to move it inside the project folder.

> I’ve never asked for that.

Then I had to explicitly tell AI to stop doing that. It then created links to the file and tried to use that.

> I’ve never asked for that.

Then again I tried to explicitly tell AI to stop doing that. It then copied the file into the project.

> I’ve never asked for that either.

All of the times the AI found a “creative” workaround. The behavior was interesting and eye-opening.

---

This whole ordeal made me think, how many times an AI worked around like this and in what scenarios? How many times was that critical to things like safety?

How many times did the human just accepted the link, and then pushed the code without that file which resulted e.g. in a CI fail, or worse? One’s imagination can run wild with other scenarios like that. But what should it do? Stopping earlier, explaining the error succinctly, could be nice, but it would only fit that scenario, therefore making a persciption is really hard.

What should the humans do? Well that bit is obvious, pay attention to what your AI agent does. And maybe, every once in a while, be a little careful what you wish for.