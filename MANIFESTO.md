---
tags:
  - self-improvement
  - teleological-programming
  - telos
date: 2025-06-12
version: 0.0.1
---
# Intention Is All You Need

> In 12 months we may be in a world where AI writes all the code
> 
> - Dario Amodei, Anthropic CEO, 2025

But if AI writes all the code, what do humans write?

**Intentions.**

## **Telos** (greek: end, purpose, goal)

Imagine you are writing a program that at some point requires a function that you don't know how to code.

Let's say that function is sentiment analysis for a given text.

You know what the function needs to do, it should receive `text` and return `['negative', 'neutral', 'positive']` alongside with a `confidence` score.

But you don't know how to solve it.

Until now, you could stop coding and google it, write a placeholder and continue coding, or more recently ask an LLM to implement something for you.

I want to propose a new way:

```python
@telos
def analyze_sentiment(text: str) -> SentimentResult:
    """Analyze the sentiment of a text string."""
    pass
```

This innocent bit of code looks like a placeholder.

It simply describes its **goal** in plain english and defines its **input**s and **output**s.

This is actually how a **Telos** function is written.

It is not a placeholder. It IS functioning code. Observe:

```python
analyze_sentiment('This is amazing! I love it!') # Returns: positive (0.98)
```

A **Telos function works from its first invocation**

**Telos** are the building blocks of **Teleological Programming**

**Telos** anatomy:
- **Context** written as the `docstring` in plain language, as seen in the initial code-block
- **Input** defined as a type, ex: `text: str`
- **Output** defined as a type, ex:
```python
class SentimentResult(BaseModel):
    sentiment: Literal["positive", "negative", "neutral"]
    confidence: float = Field(ge=0.0, le=1.0)
```

When first invoked, they may be a bit slow:

```python
start = time.time()
analyze_sentiment('Terrible experience, very disap...') # negative (0.98)
(time.time() - start)*1000 # 847ms
```

This is because it's AI who is actually solving this. So not only slow, it also costs money, for now...

## Synthesis: self-improvement

A **Telos function recurrently self-improves with subsequent use through synthesis**.

In order to perform **synthesis** a **Telos** needs training data, inputs and outputs.

#### A. Automatic data gathering

Every time you invoke a **Telos** it won't just deliver a valid output. It will store inputs and outputs.

#### B. Manual grounding

You could also provide grounding data:

```python
GROUND_TRUTH = {
  (("text", "Best decision ever!"),):
	  SentimentResult(sentiment="positive", confidence=0.98),
  (("text", "Terrible experience, very disappointed"),):
	  SentimentResult(sentiment="negative", confidence=0.90),
  (("text", "The product works as described"),):
	  SentimentResult(sentiment="neutral", confidence=0.80),
...
}
```

#### Self-improvement

After enough data is gathered, **synthesis** can be triggered automatically, or you could manually execute:

```bash
telos synthesize analyze_sentiment

Proposal 1
============================================================
rationale: Keyword-based approach tailored to test fixtures.
confidence=87%

Iteration 1
------------------------------------------------------------
Testing implementation...

err: SyntaxError: from __future__ imports must occur at the beginning of the file

Accuracy: 0.0% (0/10)

❌ Accuracy too low: 0.0%
------------------------------------------------------------
observation: I must import at the beginning.

...

❌ Accuracy too low: 86.3%

...

Iteration 5
------------------------------------------------------------
Testing implementation...

✅ Accuracy: 100.0% (10/10)
✅ Success! Implementation saved as impl_1749744691_0dc8d152

⏱️ Running quick benchmark...
🏃 Benchmark: 0.001ms (p50)

============================================================

Proposal 2
...

📊 Winner: impl_1749744691_0dc8d152
✨ 1617663x faster than LLM!

```

Agent/s submit proposals, every attempt returns valuable context for them to decide if they want to continue iterating on this or stop and try new solutions.

Subsequent Agents, will start by knowing what Proposals has been attempted and again, would be able to decide to submit new proposals or work on previous ones.

Every proposal measures:
- Accuracy, against known ground truth
- Speed
- Cost

Note that Agentic solutions are perfectly valid, meaning that an agent may propose that an agent is the right solution for the **Telos**.

After this **synthesis** is finished, all proposals are compared. Solutions are ranked and **Telos** progressively replaces the function it calls with the best **solver**.

```python
start = time.time()
analyze_sentiment('Terrible experience, very disap...') # negative (0.98)
(time.time() - start)*1000 # 0.001ms
```

`impl_1749744691_0dc8d152` is now replacing the initial LLM, and `impl_1749744691_0dc8d152` will be used until a better solution is **synthesized**

### Flow
1. Define a **Telos** function by its goal
2. If enough data is provided **synthesis** occur
3. When the function is called, a **Telos** smartly routes the input into its best available solver
4. While no solution exists an AI model provides instant solutions
5. If a solution exists but it fails, best next solution is used recursively
6. All I/O's are logged
7. AI solved outputs can be validated in an interactive test. This test can be performed by a human operator (RLHF) or by another agent (unsupervised)

## Implications

> A **Gödel machine** is a hypothetical self-improving computer program that solves problems in an optimal way. It uses a recursive self-improvement protocol in which it rewrites its own code when it can prove the new code provides a better strategy.
> 
> Wikipedia

- Programs built with **Telos** functions recurrently self-improve over time by mere use
- One could write entire programs this way
- It is not mutually exclusive with "vibe coding"

## Genesis

*Q1 2024*

The initial insight was simple: instead of using AI to help code an API, use AI *as* the API:

> "💡AI as an API to accelerate development.
> 
> Instead of using AI to help you code an API:
> 
> [USER NEED] → AI as API → Validation → Refine for cost/quality."

This proto-concept evolved into the full methodology presented here.

After first ideating this concept I observed in the wild industry applications that validated the core premise while highlighting the gap the full methodology proposed above addresses:

**Vercel (2024)**: Used AI to solve internationalized domain name encoding issues by dynamically determining language codes. While successful, their approach remains static - using AI as a permanent solution without the evolutionary improvements **Teleological Programming** provides.

This demonstrates that while "turning to AI to solve complex problems is becoming second nature" in industry, the opportunity for systematic self-improvement through empirical testing extraction remains largely unexplored.

While industry has since begun adopting similar approaches, they typically stop at the AI implementation stage without pursuing the self-improvement aspects.

---
¹ https://vercel.com/blog/using-the-ai-sdk-to-fix-edge-case-errors-in-our-code (2024)

## What's next

While still in early stages, **Telos** initial results are promising. I'm releasing this as is to invite collaboration and discussion.

I'm working on releasing this as an open-source framework in python as a proof of concept, but could be extended to any ~~programming~~ language.

## Conclusion

**Teleological Programming** represents a paradigm shift in how we approach intractable problems. By merely describing intent we can scaffold working systems that start magical, slow and inefficient, and self-improve into more efficient mechanical solutions over time - combining the best of both worlds.

> Any sufficiently analyzed magic is indistinguishable from technology.
> - Arthur C. Clarke

---
By Adrian Galilea Delgado - June 12, 2025 - Apache 2.0 License
