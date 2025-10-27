# Agentic AI Engineering - DeepLearning.AI Course

This repository contains my personal notes, code implementations, and project work for the **Agentic AI** course offered by DeepLearning.AI.

## Introduction

This repository is a log of my journey through the "Agentic AI" course. The goal of agentic AI is to move beyond single-call, monolithic LLM prompts and build autonomous, multi-step systems that can **reason, plan, and interact with tools** to solve complex problems.

This repo will contain code examples for:

  * Reflection and self-critique patterns.
  * Tool use via function calling and code execution.
  * Multi-step planning and task decomposition.
  * Multi-agent collaborative systems.
  * The final capstone "Research Agent" project.

-----

## About This Course

This course provides a comprehensive framework for designing, building, and evaluating agentic AI systems. It covers the foundational design patterns and practical skills needed to deploy robust and autonomous agents.

  * **Module 1: Introduction to Agentic AI**
    Understand what makes AI “agentic” and why iterative, multi-step workflows outperform traditional single-prompt approaches. Learn to identify tasks suitable for agentic implementation and develop a framework for evaluation-driven development.

  * **Module 2: Reflection Design Pattern**
    *Notes and code for this module are coming soon.*

  * **Module 3: Tool Use Design Pattern**
    *Notes and code for this module are coming soon.*

  * **Module 4: Practical Tips for Building Agentic AI**
    *Notes and code for this module are coming soon.*

  * **Module 5: Planning and Multi-Agent Systems**
    *Notes and code for this module are coming soon.*

  * **Capstone Project: Research Agent**
    *The final capstone project implementation is coming soon.*

-----

## Module 1: Introduction to Agentic AI (Detailed Notes)

This module lays the groundwork for the entire course, establishing the core concepts that differentiate "agentic" AI from traditional generative models.

<details>
<summary><strong>What is Agentic AI?</strong></summary>

<br>

"Agentic AI" refers to a system, typically powered by one or more Large Language Models (LLMs), that can **operate autonomously to achieve a high-level goal**. Unlike a standard LLM call, which is a "one-shot" response to a single prompt, an agentic system breaks down a complex task, creates a multi-step plan, and executes that plan over time.

The core components that make an AI "agentic" are:

1.  **Goal-Oriented:** The user provides a high-level objective (e.g., "Write a research report on the a\_synchronous-operations\_ in Python," "Book me a flight to Boston for next Tuesday"), not a specific, micro-managed instruction.
2.  **Autonomous:** The agent can proceed through its plan without requiring human intervention at every step. It makes its own decisions about what to do next.
3.  **Iterative & State-Aware:** An agent operates in a loop (often called a "ReAct" loop: Reason + Act). It assesses the current state, reasons about the next best action, takes that action, and then observes the outcome, updating its state and memory before starting the loop again.
4.  **Interactive (Tool Use):** This is perhaps the most critical component. An agent is not limited to its own internal knowledge. It can interact with the "outside world" by using **tools**. These tools can be anything from a search engine (to get new information), a code interpreter (to run Python scripts), a calculator (to perform precise math), or any external API (to book that flight or check a database).
5.  **Reflective & Self-Correcting:** A sophisticated agent can critique its own work. It can generate a draft, then "reflect" on that draft's quality, identify flaws, and then generate a new, improved version.

In short, **agentic AI shifts the paradigm from "AI as a tool" to "AI as a teammate or assistant."** Instead of you using a calculator (the tool), you ask an assistant (the agent) to "figure out the financials for the Q3 report." The assistant then *uses* the calculator, along with a database and a document writer, to get the job done. This approach allows AI to tackle problems far more complex and dynamic than any single prompt could ever handle.

</details>

<details>
<summary><strong>Degrees of Autonomy</strong></summary>

<br>

Autonomy in agentic systems is not a binary switch (on/off) but a **spectrum**. The level of autonomy you grant an agent is a critical design decision that balances capability, reliability, and safety. The course likely frames this as a progression:

  * **1. Human-in-the-Loop (HITL):** This is the lowest level of autonomy. The AI agent operates as a "co-pilot." It can analyze a situation and **propose** a plan or a single next step, but it **must wait for explicit human approval** before taking any action.

      * **Example:** A customer service agent draft a reply to a complaint. A human manager must click "Send."
      * **Use Case:** High-stakes environments where errors are costly (e.g., medical diagnosis, financial transactions, executing terminal commands).

  * **2. Partial Autonomy (Human-on-the-Loop):** The agent can execute a series of pre-defined steps on its own but is designed to **pause and ask for clarification** when it encounters ambiguity or reaches a critical decision point. The human acts as an overseer who can intervene if necessary but doesn't need to approve every minor action.

      * **Example:** A research agent gathers 10 web sources, then presents them to the user and asks, "Which of these are most relevant to focus on?" before proceeding to the writing phase.
      * **Use Case:** Complex creative or analytical tasks where human judgment and high-level direction are still superior to the AI's.

  * **3. Full Autonomy (Human-out-of-the-Loop):** This is the "holy grail" of agentic AI. The agent is given a high-level goal and is trusted to execute the entire workflow from start to finish without any human intervention. It is responsible for its own planning, execution, tool use, and error handling.

      * **Example:** An autonomous "DevOps" agent that receives an alert, diagnoses the server issue, writes and executes a patch, runs tests, and emails a report of the incident *after* it has been resolved.
      * **Use Case:** Well-defined, repetitive, and complex tasks in a controlled environment where the success criteria are clear and the agent's tools are robust.

Choosing the right degree of autonomy is a trade-off. Full autonomy is more powerful and efficient but carries a higher risk of error propagation and "runaway" processes. Human-in-the-Loop is safer but much slower and less scalable. A key skill for an agentic engineer is designing the *right* control system (e.g., validation, approval gates, logging) for the task's risk profile.

</details>

<details>
<summary><strong>Benefits of Agentic AI</strong></summary>

<br>

Moving from a single-prompt model to an agentic workflow unlocks several powerful benefits, primarily by overcoming the inherent limitations of a static LLM.

  * **Overcoming Knowledge Cutoffs:** A standard LLM (e.g., GPT-4) only knows information up to its last training date. An agentic system, by using a **web search tool**, can access real-time information, making its outputs current and factually up-to-date.
  * **Improved Accuracy and Reliability:** LLMs are notoriously bad at certain tasks, like complex arithmetic or precise string manipulation. An agent can **delegate** these tasks to the right tool. If it needs to calculate $582 \times 391$, it doesn't "guess" the answer; it calls a **code interpreter or calculator tool**, gets the exact result (227,562), and inserts that factual data into its response. This makes the agent's final output far more reliable.
  * **Solving Complex, Multi-Step Problems:** A single prompt has a finite context window and "attention" span. You cannot ask a standard LLM to "write a 50-page screenplay." It will fail. An agent, however, can **decompose** this goal:
    1.  "Create a plot outline."
    2.  "Develop character profiles."
    3.  "Write Act 1, Scene 1."
    4.  "Write Act 1, Scene 2."
        ...and so on. By breaking the problem down and iterating, the agent can complete tasks far beyond the scope of a single LLM call.
  * **Interacting with the Real World:** This is a massive benefit. An agent with the right tools can **effect change outside of its own text generation**. It can connect to an email API to send messages, a calendar API to schedule meetings, a database to query customer records, or a trading API to execute a stock purchase. This moves AI from a passive information-provider to an active participant in digital and even physical (via robotics) systems.
  * **Enhanced Robustness through Self-Correction:** In a simple prompt-response model, if the LLM makes a mistake, the output is final. An agent built with a **reflection pattern** can check its own work. It can run its code and see if it throws an error. If it does, the agent can read the error message, "reflect" on the problem, and then *try again* to fix the bug, all without human intervention. This iterative refinement leads to a much higher-quality final product.

</details>

<details>
<summary><strong>Agentic AI Applications</strong></summary>

<br>

Agentic AI is not a single product but a new **architecture** for building software. This architecture is unlocking applications across virtually every industry.

  * **Software Engineering:** This is one of the most prominent applications. An "AI Software Engineer" agent can be given a GitHub issue or a feature request. It can read the existing codebase (tool), write new code to implement the feature (generation), run the linter and unit tests (tool), debug any errors (reflection + tool use), and finally, submit a pull request.
  * **Scientific and Academic Research:** A "Research Agent" (like the course's capstone) can be tasked with "investigating the link between gut microbiome and depression." It can use search tools to find papers, use a PDF reader tool to extract text, a "graphing" tool to visualize data, and a "writer" tool to synthesize its findings into a comprehensive literature review.
  * **Business Process Automation (BPA):** Agents can automate complex back-office tasks. An "auditing" agent could be tasked with "verifying all employee expense reports for Q3." It would connect to the expense report database (tool), read each report, check it against company policy (reasoning), use a currency converter API (tool), and flag discrepancies for human review.
  * **Hyper-Personalized Assistants:** Beyond simple "Alexa, set a timer" commands, an agentic assistant could handle a goal like "Plan a date night for my anniversary next Friday." It would check your calendar (tool), research restaurants (tool), check their availability via an API (tool), consider your known food preferences (memory), and then present you with 3 fully-booked options.
  * **Autonomous Content Creation:** An agent can run a "digital marketing" workflow. Given the goal "Promote our new product launch," it could research competing products (tool), draft 5 different blog post ideas (planning), write the full text for the chosen idea (generation), search for relevant images (tool), and finally, stage the post in a content management system (tool).
  * **E-commerce and Customer Support:** An agent can go beyond a simple chatbot. A customer might say, "My package is late and I want a refund." A non-agentic bot can only answer questions. An *agentic* bot can look up the order in the database (tool), check the shipping API for its status (tool), see that it's lost, and then automatically issue the refund via the payments API (tool), all in one autonomous flow.

</details>

<details>
<summary><strong>Task Decomposition: Identifying the steps in a workflow</strong></summary>

<br>

**Task decomposition is the single most important skill in agentic AI engineering.** It is the process of breaking a large, complex, or vague user goal into a sequence of smaller, concrete, and actionable steps. An LLM cannot execute "plan a vacation." It *can* execute "1. Search for flights from SFO to TYO."

This is the "planning" part of an agent's "reasoning" loop. Without a good plan, the agent will fail, get stuck in a loop, or produce a low-quality result.

  * **Why is it necessary?** LLMs excel at short-horizon tasks. They are good at "what is the *next* word?" or "what is the *next* logical step?" They are very bad at "what is the 50-step plan to build a house?" Task decomposition bridges this gap. It turns an intractable marathon into a series of manageable sprints.
  * **Methods of Decomposition:**
    1.  **Human-Defined Plan (Simple):** The developer explicitly codes the plan. For a "weather" agent, the plan is always: "1. Get the user's location. 2. Call the weather API. 3. Format the result." This is simple and reliable but not flexible.
    2.  **LLM-Generated Plan (Dynamic):** This is the more "agentic" approach. The *first thing* the agent does is call an LLM with a meta-prompt like, "You are a world-class project manager. Your goal is: [User's Goal]. Break this down into a step-by-step plan of tasks. For each task, identify the tool you will use."
  * **Example of LLM-Generated Decomposition:**
      * **User Goal:** "Write a blog post about the pros and cons of React vs. Vue."
      * **Agent's Generated Plan:**
        1.  `{ "task": "Research the main benefits of React", "tool": "web_search" }`
        2.  `{ "task": "Research the main drawbacks of React", "tool": "web_search" }`
        3.  `{ "task": "Research the main benefits of Vue", "tool": "web_search" }`
        4.  `{ "task": "Research the main drawbacks of Vue", "tool": "web_search" }`
        5.  `{ "task": "Synthesize all research findings", "tool": "memory" }`
        6.  `{ "task": "Write an outline for the blog post", "tool": "llm_writer" }`
        7.  `{ "task": "Draft the 'Introduction' section", "tool": "llm_writer" }`
        8.  `{ "task": "Draft the 'React Pros/Cons' section", "tool": "llm_writer" }`
        9.  `{ "task": "Draft the 'Vue Pros/Cons' section", "tool": "llm_writer" }`
        10. `{ "task": "Draft the 'Conclusion' section", "tool": "llm_writer" }`
        11. `{ "task": "Review the full draft for flow and accuracy", "tool": "llm_critic" }`

The agent then executes this plan one task at a time, storing the results in its "memory" (or scratchpad) to be used by subsequent steps. The quality of this initial plan is the single biggest predictor of the agent's success.

</details>

<details>
<summary><strong>Evaluation of Agentic AI (Evals)</strong></summary>

<br>

"Evals" (evaluations) are how you measure if your agent is working, if it's getting better, and where it's failing. The course introduces this as **Evaluation-Driven Development (EDD)**. This is a crucial concept. In traditional software, you write unit tests that check for an *exact* outcome (e.g., `assert add(2, 2) == 4`). But how do you "assert" that an agent "wrote a *good* blog post"?

Evaluating agents is difficult because their outputs are often non-deterministic and qualitative.

  * **Why Evals are Critical:** When you change a prompt, add a new tool, or tweak your agent's "constitution," you need a way to measure if that change was an *improvement* or a *regression*. Without a strong set of evals, you are "prompt engineering in the dark."
  * **Types of Evals for Agents:**
    1.  **Exact-Match Evaluation:** Used for tasks with a single correct answer.
          * **Example:** For a math agent, you run it on a "test set" of 100 math problems and check if its final answer matches the known correct answer.
          * **Pro:** Simple, fast, objective.
          * **Con:** Only works for a small class of "closed-domain" problems.
    2.  **Human-in-the-Loop Evaluation:** You create a "golden set" of tasks (e.g., 50 sample user requests) and run your agent on them. A human expert then grades the agent's final output on a scale (e.g., 1-5) based on a **rubric** (e.g., "Is it accurate? Is it comprehensive? Is it helpful?").
          * **Pro:** The "gold standard" for quality, captures nuance.
          * **Con:** Extremely slow, expensive, and subjective.
    3.  **LLM-as-Judge Evaluation:** This is a powerful and scalable compromise. Instead of a human, you use *another, powerful LLM* (like GPT-4) to grade your agent's output. You give the "Judge LLM" the user's request, the agent's output, and a detailed rubric.
          * **Prompt:** "You are an expert evaluator. Here is a user request and an AI's response. Please grade the response on a scale of 1-10 for 'Completeness' and 'Accuracy' based on the following criteria..."
          * **Pro:** Fast, cheap, and surprisingly accurate (often 80-90% correlation with human-level grading).
          * **Con:** Can have biases, and you're using an AI to evaluate an AI.
    4.  **Simulation-Based Evaluation:** You create a "sandbox" environment for your agent (e.g., a fake file system, a mock e-commerce website). You then give the agent a task (e.g., "Find the file named 'report.txt' in the 'docs' folder and delete it") and check if the *state of the environment* matches the desired end state.
          * **Pro:** Excellent for testing complex tool use and side effects.
          * **Con:** Hard to build and maintain the simulation environment.

The EDD workflow is: **1. Build** a V1 agent. **2. Create** an eval dataset (e.g., 50 test cases). **3. Run** the agent on the dataset and save its scores. **4. Analyze** the failures. **5. Tweak** the agent's prompts/tools. **6. Re-run** the evals and see if the score improved. **Repeat.**

</details>

<details>
<summary><strong>Agentic Design Patterns</strong></summary>

<br>

Agentic AI is not about finding one "master prompt." It's about system design. Agentic design patterns are **reusable architectural solutions** for building agentic workflows. The course modules are built around these very patterns.

  * **1. Reflection (Module 2):** This pattern is about **iterative self-improvement**. Instead of just outputting a final answer, the agent generates a first draft and then *feeds that draft back to itself* in a new prompt to critique it.

      * **Workflow:**
        1.  **Generate Draft:** The agent writes a first version (e.g., a piece of code).
        2.  **Reflect & Critique:** The agent calls an LLM (or itself) with a prompt like, "Here is a piece of code. Critique it. Is it efficient? Does it have bugs? How can it be improved?"
        3.  **Refine:** The agent takes the critique and *uses it* to generate a new, better version.
      * **Benefit:** Produces much higher-quality, more robust outputs by catching its own errors.

  * **2. Tool Use (Module 3):** This is the fundamental pattern of **extending an LLM's capabilities**. The LLM's "brain" is given "hands and eyes" to interact with the world. This is most commonly implemented via **function calling**.

      * **Workflow:**
        1.  The LLM, instead of just responding with text, outputs a special JSON object: `{"tool_name": "search_web", "arguments": {"query": "latest AI news"}}`.
        2.  The "agent framework" (your code) *intercepts* this JSON.
        3.  Your code *actually executes* the `search_web("latest AI news")` function.
        4.  The function returns its result (e.g., a list of news articles).
        5.  Your code *passes this result back* to the LLM, which then uses the new information to form its final text answer.
      * **Benefit:** Gives the agent access to real-time data, calculations, and any action an API can perform.

  * **3. Planning (Module 5):** This pattern, based on **task decomposition**, involves having the agent create a formal plan *before* it starts working.

      * **Workflow:** The agent's first step is always to generate a plan (e.g., a JSON list of tasks). An "executor" loop then iterates through this plan, completing one task at a time. The agent can even *modify its own plan* mid-way if it finds new information.
      * **Benefit:** Enables solving complex, long-horizon tasks in a structured way.

  * **4. Multi-Agent Systems (Module 5):** This is the "divide and conquer" pattern. Instead of one "do-it-all" agent, you create a **team of specialized agents** that collaborate.

      * **Workflow (e.g., a "research team"):**
        1.  A **"Manager" Agent** receives the user's goal and creates a plan.
        2.  The Manager assigns the first task ("gather information") to a **"Researcher" Agent**.
        3.  The Researcher (who only has `search` tools) finds 10 articles and passes them back to the Manager.
        4.  The Manager gives the articles to a **"Writer" Agent** with the task "write a draft."
        5.  The Writer produces a draft and gives it to a **"Critic" Agent.**
        6.  The Critic reviews the draft and provides feedback.
        7.  The Manager gives the draft *and* the feedback back to the Writer for a final revision.
      * **Benefit:** By specializing each agent with its own prompts, instructions, and tools, you can create a "society of agents" that is far more capable and reliable than any single agent.

</details>

-----

## Acknowledgement

This repository is for my personal educational and learning purposes only. All course materials, concepts, and project ideas are the intellectual property of **DeepLearning.AI**. I do not claim any ownership over the course curriculum. Please enroll in the official course to access the complete materials and support.