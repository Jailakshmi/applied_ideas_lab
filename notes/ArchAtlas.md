From folder chaos to architectural clarity
Observation

Every time I open a new or existing codebase, the first challenge is rarely about business logic.

It’s much more basic:
What even exists here?
How is this system structured?

When a project is already built, the src folder is deeply nested, spread across many modules and responsibilities. Before touching any code, I usually spend a long time just forming a mental picture of the architecture.

That process is slow. It’s mentally heavy. And it happens almost every single time.

Why This Matters

This isn’t just a small inconvenience.

Understanding structure is the foundation for everything else: debugging, extending features, refactoring, even asking the right questions. When orientation takes hours, it quietly drains energy, confidence, and velocity.

For backend engineers especially, this compounds. Every new service, every ownership change, every inherited system repeats the same tax: time spent just to understand what exists.

What’s Actually Hard Here

The problem isn’t that the information is missing.

It’s that it’s scattered, and it takes heavy cognitive load and time to understand the existing structure and align with it.

Folders are nested. Responsibilities are implicit. Architecture lives in people’s heads or outdated docs. And our brains are forced to simulate a whole system from a file explorer.

The hard part is not extracting files.
The hard part is turning raw structure into a clear, external mental model.

Most quick solutions fail because they either stay textual, or they try to jump straight into diagrams without control, accuracy, or grounding in the real structure.

Direction of Thought

Instead of starting from “how do I generate a diagram,” I’m starting from:

How do I reduce the cognitive load of understanding a codebase?

The direction I’m exploring is turning real project structure into a block-level visual that lets someone feel oriented quickly — before they dive into logic.

Not a conceptual architecture.
A structural one.

Something that reflects what actually exists.

Thinking About a Possible System

As a first step, I clearly defined the input and the output.

Input:
A highly structured codebase — messy, deeply nested architectural folders, uploaded as a zip file.

Output:
An architectural atlas — a clear block-level visual that shows where each file belongs, which folder it sits in, and where that folder lies in the overall system. A crystal-clear diagram for quick understanding.

Once that was clear, I started asking what the tool must actually do to reach this goal. Which meant breaking one big process into smaller, concrete tasks.

The first step was straightforward:
the user’s zip file has to be read.
From it, I need to extract file names and folder names, and then meaningfully arrange them into an understandable structure — something like JSON that precisely represents where everything actually lives.

The second step is where my first instinct went:
give that JSON to an LLM and ask it to generate a block diagram.

That was my immediate first thought.

But very quickly, reality hit.
Image generation itself isn’t very accurate. And asking a model to directly generate diagrams from raw structure isn’t reliable either — especially when precision matters.

So I had to break this second step down further.

The first step stayed the same.
But for the second, instead of “generate an image,” I started looking at tools that are specifically built for block and flow diagram generation.

That’s when I went deeper into Mermaid.

Mermaid is a JavaScript-based framework where diagrams are rendered from structured code. So the approach shifted to this:

What if I ask the LLM to generate Mermaid code from the JSON structure?

Then, once Mermaid code is generated, I can render it using HTML — and the visual accuracy becomes much more controlled and predictable.

This direction felt far more doable.

With that, my first sitting was done.
I’ll continue this thread with how I took this further, what worked, what broke, and how my thinking evolved.

Constraints & Open Questions

Some of the questions I’m sitting with right now:

What level of structure is actually useful, and what becomes noise?

How should deeply nested systems be visualized without becoming overwhelming?

Where does automated visualization break down?

How much interpretation should a system attempt versus staying purely structural?
