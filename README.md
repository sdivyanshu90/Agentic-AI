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

  * **Module 1: Introduction to Agentic AI** <br>
    Understand what makes AI “agentic” and why iterative, multi-step workflows outperform traditional single-prompt approaches. Learn to identify tasks suitable for agentic implementation and develop a framework for evaluation-driven development.

  * **Module 2: Reflection Design Pattern** <br>
    Master the foundational pattern where AI systems critique and improve their own outputs. Build workflows for database querying, chart generation, and document writing that get better through self-reflection.

  * **Module 3: Tool Use Design Pattern** <br>
    Enable your AI systems to interact with the world through tools. Learn multiple approaches including function calling, code execution, and the Model Context Protocol (MCP) for seamless tool integration.

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

## Module 2 - The Reflection Design Pattern

Welcome to this deep dive into Module 2 of the "Agentic AI" course. This module transitions from simple, single-call Large Language Model (LLM) interactions to building robust, autonomous systems.

The core concept is **Reflection**.

At its simplest, this pattern is an automated workflow that mirrors a human's creative process:

1.  **Generate:** Create a first draft of a solution (e.g., code, text, a plan).
2.  **Critique (Reflect):** Step back, examine the draft against specific criteria, and identify its flaws, omissions, or potential errors.
3.  **Refine:** Create a new, improved version that explicitly addresses the feedback from the critique.

This `Generate -> Critique -> Refine` loop transforms an LLM from a simple *tool* into an *agentic system*—a system that can "think" about its own work, catch its own mistakes, and iteratively improve its output before a human ever sees it. This module explores why this is necessary, how to build it, how to measure it, and how to enhance it with external knowledge.

Here is a detailed breakdown of each key topic from the module.

<details>
<summary><strong>Reflection to improve outputs of a task</strong></summary>

This is the foundational concept of the entire module. "Reflection" is the mechanism by which an agentic system performs self-correction and iterative improvement. It is a structured, multi-step workflow that replaces a single, hopeful prompt with a robust, self-validating process.

#### The Core Workflow

The reflection pattern is implemented by creating a "system of agents"—or at least, a system of *roles* that different LLM calls will play.

1.  **The "Generator" Agent:**

      * **Role:** The "doer" or "executor."
      * **Task:** This agent receives the initial user request (e.g., "Write a Python script to analyze this CSV and find the top 5 most common values in the 'Category' column.")
      * **Output:** It produces a "first draft" or "Version 1" of the solution. This draft might be 80% correct, but it could contain subtle bugs, miss edge cases, or be syntactically valid but logically flawed.

2.  **The "Critic" (or "Reflector") Agent:**

      * **Role:** The "reviewer" or "quality assurance."
      * **Task:** This is the heart of the reflection pattern. This agent *does not* try to solve the original problem. Instead, it is prompted to perform a *critique* of the Generator's output.
      * **Inputs:** To do its job, the Critic agent needs several pieces of information:
          * The original user request.
          * The "Version 1" output from the Generator.
          * A "rubric" or a set of *critique instructions*. This is the most important part. You don't just ask, "Is this good?" You ask, "Critically evaluate this Python script based on the following criteria: 1. **Correctness:** Does it correctly use the pandas library to read the CSV? Does the `value_counts()` logic correctly identify the top 5? 2. **Robustness:** Does it handle potential errors, like the file not existing or the 'Category' column being absent? 3. **Clarity:** Is the code well-commented and easy to read?"
      * **Output:** The Critic produces a structured piece of text—the *critique*—detailing strengths and, more importantly, weaknesses (e.g., "The script is mostly correct, but it will crash with a `FileNotFoundError` if the path is wrong. It also fails to import the `pandas` library, which will cause an `NameError`.")

3.  **The "Refiner" Agent:**

      * **Role:** The "problem solver."
      * **Task:** This agent's job is to create "Version 2" of the solution.
      * **Inputs:** It receives a consolidated prompt containing:
          * The original user request.
          * The "Version 1" output.
          * The *critique* from the Critic agent.
      * **Prompt:** The prompt for this agent is explicit: "You are a software engineer. Your task is to rewrite the following Python script (`Version 1`) to fix the specific issues identified in the `Critique`. Do not introduce new functionality; only apply the fixes."
      * **Output:** "Version 2," which (ideally) addresses all the identified issues (e.g., the new script now includes `import pandas as pd` and is wrapped in a `try...except` block).

This loop can be run multiple times. Version 2 can be passed *back* to the Critic for a second round of review. This process repeats until the Critic agent's output is "No issues found" or a predefined number of iterations (e.g., 3) has been reached. This provides a high-quality, pre-validated result to the user.

</details>

<details>
<summary><strong>Why not Direct Generation?</strong></summary>

This topic addresses the fundamental problem that the Reflection pattern is designed to solve. "Direct Generation" refers to the standard, single-shot interaction with an LLM: `User Prompt -> LLM -> Output`. While impressive for simple tasks, this approach fails catastrophically as task complexity increases.

#### The Pitfalls of Direct Generation

1.  **The "First-Try" Problem:** LLMs are probabilistic models, not deterministic compilers. Their first attempt at any complex task (like writing a 300-line program, a legal analysis, or a multi-part database query) is rarely perfect. It's just a *statistically likely* first draft. Direct Generation forces the *human user* to be the critic, debugger, and refiner, which defeats the purpose of autonomous agents.

2.  **Vague Prompts and "Mind-Reading":** Users often provide underspecified or ambiguous prompts (e.g., "Make a chart of our sales data."). A Direct Generation model has to *guess* the user's *true intent*.

      * What chart type? (Line, bar, pie?)
      * What time-frame? (Last week, last year?)
      * How to aggregate? (By day, by product?)
      * The LLM's first guess is often wrong, leading to a frustrating back-and-forth "prompt-refinement" cycle that is slow and inefficient.

3.  **Compounding Errors (Lack of "Grounding"):** In a multi-step task, a small error in Step 1 will invalidate all subsequent steps.

      * *Example:* `User: "Analyze customer sentiment in our reviews and then write a summary."`
      * *Direct Gen (Step 1):* The LLM writes code to load `reviews.csv`. It *assumes* the sentiment is in a column named `review_text`.
      * *Direct Gen (Step 2):* The actual file has the column named `customer_comment`. The code crashes, and the subsequent summary step is built on `None` data, resulting in a nonsensical, a-contextual, or "hallucinated" summary.
      * Direct Generation has no mechanism to *stop*, *check its work*, and *verify its assumptions* before proceeding.

4.  **The Cognitive Load is on the User:** The Direct Generation model places the entire burden of quality control on the human. The user must:

      * Meticulously check the generated text for factual errors.
      * Read and debug every line of generated code.
      * Identify logical fallacies in a generated argument.
      * Figure out *why* the output is wrong.
      * Formulate a new, precise prompt to correct the error.

**The Reflection pattern solves this by automating this cognitive load.** It builds the *critique and refinement* steps *into* the AI system itself. The agent checks its *own* assumptions, debugs its *own* code, and verifies its *own* logic, *before* presenting the final, polished output to the user. This makes the system more reliable, more robust, and more autonomous.

</details>

<details>
<summary><strong>Chart Generation Workflow</strong></summary>

This topic provides the module's "hero" example—a practical, complex, multi-domain task that is notoriously difficult for Direct Generation but is perfectly suited for the Reflection pattern.

Generating a chart from a natural language request (e.g., "Show me the top 5 product categories by total sales for last quarter") is extremely difficult because it requires a *chain of specialized skills*:

1.  **Natural Language Understanding:** Parse the user's intent ("top 5," "total sales," "last quarter").
2.  **Database Querying:** Translate this intent into a correct SQL (or Pandas/DataFrame) query to fetch the data.
3.  **Data Transformation:** The raw data from the DB may need to be pivoted, aggregated, or cleaned.
4.  **Code Generation:** The cleaned data must be fed into a plotting library (e.g., Matplotlib, Plotly) to generate the chart code.
5.  **Execution:** The code must be run to produce the final image.

A Direct Generation model will almost certainly fail, likely by generating plotting code that *assumes* the data already exists in a variable named `df`.

#### The Reflection Workflow for Chart Generation

A robust agent uses reflection at *each* critical step.

**Phase 1: Data Generation (SQL)**

1.  **Generate (SQL):** An "SQL Agent" is given the database schema and the user's prompt. It generates `SQL - v1`.

      * *Prompt:* "Given this schema [schema...], write a SQLite query for: 'top 5 product categories by total sales for last quarter'."
      * *Output (v1):* `SELECT category, SUM(sales) FROM transactions WHERE date > '2024-07-01' GROUP BY 1 ORDER BY 2 DESC LIMIT 5;`

2.  **Reflect (SQL Critic):** A "SQL Critic Agent" reviews this query.

      * *Prompt:* "Critically review this SQL query. Is the syntax valid for SQLite? Does it correctly interpret 'last quarter'? Is `GROUP BY 1` safe? Does it handle `transactions.date` as a timestamp?"
      * *Critique:* "The query is logically sound, but it has two potential issues. First, `GROUP BY 1` is poor practice; it should be `GROUP BY category`. Second, `date > '2024-07-01'` is hardcoded; a more robust query would use date functions like `strftime`. The query will work, but it is brittle."

3.  **Refine (SQL):** An "SQL Refiner Agent" gets the original query and the critique.

      * *Prompt:* "Rewrite this SQL to address the critique, specifically by replacing `GROUP BY 1` and using a date function for 'last quarter'."
      * *Output (v2):* `SELECT category, SUM(sales) FROM transactions WHERE strftime('%Y-%m', date) >= strftime('%Y-%m', 'now', '-3 months') GROUP BY category ORDER BY SUM(sales) DESC LIMIT 5;`

**Phase 2: Code Generation (Plotting)**

*Now*, the system *executes* the validated `SQL - v2`, gets the data, and passes it to the next phase.

1.  **Generate (Plot):** A "Plotting Agent" receives the data (e.g., as a JSON array or a pandas DataFrame) and the original request.

      * *Prompt:* "Using this data [data...] and the Matplotlib library, write Python code to generate a bar chart for 'top 5 product categories by total sales'."
      * *Output (v1):* `import matplotlib.pyplot as plt; data = ...; plt.bar(data['category'], data['total_sales']); plt.show();`

2.  **Reflect (Plot Critic):** A "Plotting Critic Agent" reviews the code.

      * *Prompt:* "Critically review this Matplotlib code. Does it produce a chart? More importantly, is the chart *useful*? Does it have a title? Are the X and Y axes labeled? If category names are long, will they overlap?"
      * *Critique:* "The code is functional but creates a poor-quality chart. It is missing a title, an x-label, and a y-label, making it unreadable. Furthermore, the long category names on the x-axis will overlap. A horizontal bar chart (`plt.barh`) would be much better."

3.  **Refine (Plot):** A "Plotting Refiner Agent" gets the code and the critique.

      * *Prompt:* "Rewrite this Matplotlib code to address the critique. Change it to a horizontal bar chart (`barh`), add a title 'Top 5 Categories by Sales', and label the axes 'Category' and 'Total Sales'."
      * *Output (v2):* (A new, complete script with `plt.barh`, `plt.title`, `plt.xlabel`, `plt.ylabel`, and `plt.tight_layout()` to ensure readability.)

Only this final, twice-validated script is executed to produce the chart image for the user. This workflow is far more complex but *dramatically* more reliable.

</details>

<details>
<summary><strong>Evaluating the impact of the reflection</strong></summary>

This topic is crucial from an engineering and business perspective. The reflection pattern is not "free"—it costs more in terms of:

  * **Token Usage:** You are making 3 (or 5, or 7) LLM calls instead of 1.
  * **Latency:** The entire process takes longer.

You *must* be able to prove that this extra cost and time deliver a quantifiable improvement. "Evaluating the impact" means moving from "it *feels* better" to "it *is* better, and here's the data."

#### Evaluation Methodologies

1.  **Automated Testing (for Code-Based Tasks):**

      * **Pass/Fail Execution:** This is the simplest, most powerful metric. Create a "test set" of 100 prompts (e.g., 100 chart-generation requests).
      * *Metric 1 (Success Rate):* Run all 100 prompts through the "Direct Generation" model. How many successfully execute without a human-visible error? (e.g., 15/100 = 15% Success).
      * *Metric 2 (Reflection Success Rate):* Run the same 100 prompts through the "Reflection Workflow." How many succeed? (e.g., 85/100 = 85% Success).
      * This provides a clear, objective measure of improvement. The 70-point increase in success rate justifies the added cost.

2.  **LLM-as-Evaluator (for Text-Based Tasks):**

      * For tasks like document writing, "correctness" is subjective. Here, you can use another, powerful LLM (like GPT-4o) as an impartial judge.
      * **Workflow:**
        1.  Take a prompt (e.g., "Write a professional summary of this meeting transcript").
        2.  Generate `Output A` (Direct) and `Output B` (Reflection).
        3.  Feed both outputs to an "Evaluator LLM" in a randomized order.
        4.  *Prompt (Evaluator):* "You are an expert business analyst. Below are two summaries (A and B) of the same meeting. Evaluate them on a scale of 1-5 for **Clarity**, **Accuracy** (coverage of all key decisions), and **Professionalism**. Provide your scores in a JSON format."
      * By aggregating these scores over hundreds of examples, you can statistically prove that the Reflection model's average score (e.g., 4.5/5) is higher than the Direct model's (e.g., 2.8/5).

3.  **Human-in-the-Loop (HIL) Evaluation:**

      * This is the "gold standard." It's slow and expensive, but it's the most accurate.
      * **A/B Testing:** Present human raters with the outputs from both systems side-by-side (blinded, randomized) and ask, "Which response is better?"
      * **Task-Based Grading:** Give a human expert a rubric (just like the Critic agent) and have them manually score the final outputs from both systems.
      * **"Edits Needed" Metric:** A very practical metric. How many times does the human user have to manually edit or re-prompt the agent to get a usable result? A lower number is better. If the Direct model takes 4 user follow-ups and the Reflection model takes 0, the reflection pattern is a clear winner.

</details>

<details>
<summary><strong>Using External Feedback</strong></summary>

This final topic "supercharges" the reflection pattern. So far, the "Critique" step has been *internal self-reflection* (an LLM critiquing another LLM's output in a vacuum).

"External Feedback" is about grounding the agent in *reality*. It's about incorporating information from the "outside world" into the reflection loop to make it even more powerful. This feedback is objective, truthful, and often impossible for the LLM to "hallucinate" or guess.

There are three primary types of External Feedback:

1.  **Tool Feedback (e.g., Error Messages):**

      * This is the most powerful form of external feedback for code-related tasks. Instead of just having an *AI Critic* *guess* if the code will work, you *run the code* and let the *compiler or database* be the critic.
      * **Improved Workflow (Chart Gen):**
        1.  `Generate (SQL - v1)`
        2.  `Execute (SQL - v1)`
        3.  **`External Feedback:`** The database *crashes* and returns an error: `FATAL: no such column 'category_name'`.
        4.  `Reflect (Refine):` The agent is *not* given a subjective AI critique. It is given the *real error message*.
        5.  *Prompt (Refiner):* "Your last SQL query failed. Here is the output from the database: `FATAL: no such column 'category_name'`. Here is the database schema [schema...]. Please find the correct column name and rewrite the query."
        6.  The agent now "knows" with 100% certainty that `category_name` is wrong and can search the schema for the correct name (e.g., `product_category`), fixing its own mistake. This is the core of the **ReAct (Reason + Act)** pattern, where the *Action* (running code) produces an *Observation* (the error message), which is then used for *Reasoning*.

2.  **Document Feedback (e.g., RAG):**

      * This feedback is used when the agent's output is *internally consistent* but *factually incorrect* because it lacks world knowledge.
      * **Workflow (Document Writing):**
        1.  `Generate (v1):` "Write an email summarizing our new 'Project Titan' initiative." (The LLM generates a vague, generic email about a project.)
        2.  `Reflect (AI Critic):` "This email is too vague. It doesn't mention the project lead, the timeline, or the key objectives."
        3.  `Refine (with RAG):` The Refiner agent is given the critique *and* a new tool: `search_internal_wiki('Project Titan')`.
        4.  **`External Feedback:`** The agent calls this tool and gets back a document: "Project Titan: Lead: Dr. A. Smith. Timeline: Q4 2025. Objectives: Reduce server costs by 30%..."
        5.  The Refiner agent now uses this *external, factual data* to rewrite the email, populating it with the correct names, dates, and goals. The critique is addressed using knowledge from an external source.

3.  **Human Feedback (Human-in-the-Loop):**

      * This is the ultimate form of external feedback, used for correcting subjective preferences, ambiguity, or goals that the AI could not possibly know.
      * **Workflow:**
        1.  `Generate -> AI Reflect -> Refine (v2)` (The AI agent produces a high-quality, technically correct horizontal bar chart.)
        2.  **`External Feedback (Human):`** The agent shows the user the chart. The user says, "This is great, but our company's brand guidelines require all charts to use the color blue, not green. Please change it."
        3.  `Reflect (Refine - v3):` The agent takes this *new human instruction* as its "critique" and regenerates the chart code, this time specifying `color='blue'`.
      * This allows the agent to "course-correct" based on user preference, not just on technical correctness.

</details>

---

## Module 3 - The Tool Use Design Pattern

This module refers to the core architectural blueprint for making an AI *agentic*.

* **"Tool Use"** is the fundamental concept of giving your AI (which is normally just a text-generating "brain in a vat") the ability to *use* external capabilities. These "tools" are its "hands and eyes," allowing it to interact with the world—like searching Google, checking a database, or sending an email.
* **"Design Pattern"** means this module teaches you the *standard, repeatable, and reusable solution* for building this capability. It's not just a single trick, but the established engineering blueprint for how to connect tools to an AI, have the AI *reason* about which tool to use, call it correctly, and understand its output.

In short, this module teaches you the "how-to" guide for building AIs that can **take action** (Tool Use) using a **proven, structured method** (Design Pattern).

<details>
<summary><strong>What are Tools?</strong></summary>

### The Core Concept: Bridging the Digital and Physical Worlds

At its most fundamental level, an "Agentic AI" or a Large Language Model (LLM) is a "brain in a vat." It's a text-in, text-out system. It has been trained on a massive snapshot of the internet, but it's frozen in time (its "knowledge cutoff") and completely disconnected from the "real world." It can't browse a new website, check today's weather, book a flight, or even perform a reliable calculation, because it's just a pattern-matching engine for text.

**Tools are the "wires" that connect this "brain in a vat" to the outside world.**

They are the mechanisms that allow a passive, text-generating model to become an active, problem-solving **agent**. Tools are the "eyes," "ears," and "hands" of the LLM.

  * **Tools as "Sensors" (Reading Information):** These tools allow the LLM to *perceive* the world beyond its static training data.

      * **Example 1: Web Search:** A user asks, "Who won the 2024 World Series?" An LLM trained in 2023 has no idea. An agent with a `search_web()` tool can take the query, send it to a search engine (like Google), get the text results back, and then use its language skills to synthesize that *new* information into a perfect answer.
      * **Example 2: Database Access:** A user asks, "What's the current inventory level for product XYZ-123?" The LLM can't know this. But it can use a `query_database()` tool, which (in the background) runs a SQL query against the company's private inventory system, gets a JSON response `{"stock": 150}`, and replies, "We have 150 units of XYZ-123 in stock."
      * **Other Examples:** `get_current_weather(location)`, `read_calendar()`, `get_stock_price(ticker)`.

  * **Tools as "Actuators" (Taking Action):** These tools allow the LLM to *change* the state of the world. This is where "agentic" behavior becomes truly powerful.

      * **Example 1: Sending an Email:** A user says, "Draft an email to my team about the new deadline and send it." The LLM can use its language skills to *draft* the email. Then, it can use a `send_email(to, subject, body)` tool to *actually send* it via a service like Gmail or SendGrid.
      * **Example 2: Booking a Flight:** "Find me a flight from JFK to LAX for next Tuesday." The agent can use a "sensor" tool like a `search_flights()` API to find options. After the user confirms, it can use an "actuator" tool like `book_flight(flight_id, passenger_details)` to complete the purchase.
      * **Other Examples:** `post_to_twitter(message)`, `add_calendar_event()`, `update_database_record()`.

  * **Tools as "Processors" (Enhancing Capability):** These tools overcome the LLM's "fuzzy" and non-deterministic nature. LLMs are bad at precise, logical operations (like math). Tools can offload these tasks to systems that are perfect at them.

      * **Example 1: Calculator:** A user asks, "What is $(125 * 34) / 17$?" An LLM might *guess* the answer, and it might be wrong. An agent with a `calculator()` tool (or, better yet, a `run_python_code()` tool) can pass the expression to a deterministic interpreter, get the *exact* answer (250), and provide it with 100% confidence.
      * **Example 2: Code Execution:** This is the "ultimate" processor tool, which we'll cover in its own section. The LLM can *write its own code* to solve complex problems (data analysis, image manipulation, etc.) and then use a tool to *run* that code.

### The "Agentic Loop": How Tools are Used

Tools are not magic; they operate within a strict logical loop often called the **Observe-Think-Act (OTA)** or **ReAct (Reason + Act)** loop.

1.  **Observe (User Input):** The user provides a prompt or query (e.g., "What's the weather in London and send a summary to my boss?").
2.  **Think (LLM Reasoning):** The LLM receives the prompt. It *also* receives a list of *available tools* (e.g., `get_weather`, `send_email`). The LLM's internal "thought" process might be:
      * *"The user wants two things: the weather in London and to send an email."*
      * *"I can't do either of these things myself."*
      * *"I should first use the `get_weather` tool for London."*
      * *"After I get that information, I will need to use the `send_email` tool."*
3.  **Act (Tool Call):** The LLM *does not* respond to the user. Instead, it outputs a *structured request* (like JSON) to the system, asking to use a tool.
      * **LLM Output:** `{"tool_call": "get_weather", "arguments": {"city": "London"}}`
4.  **Observe (Tool Execution):** The system *(your code)* receives this request. It parses the JSON, calls the *actual* `get_weather("London")` function, which makes a real API call. The API returns a result.
      * **Tool Result:** `{"temperature": 15, "condition": "Cloudy", "humidity": 80}`
5.  **Think (LLM Reasoning, Round 2):** This tool result (as a string) is fed *back* to the LLM. The LLM's new prompt now includes the *entire history*:
      * *User:* "What's the weather in London and send a summary to my boss?"
      * *LLM (Tool Call):* `get_weather(city="London")`
      * *System (Tool Result):* `{"temperature": 15, "condition": "Cloudy", "humidity": 80}`
      * The LLM "thinks": *"Okay, I have the weather. Now I need to send the email. The user's boss's email is 'boss@example.com' (from my instructions, or I might need to ask). I will now draft the email body and call the `send_email` tool."*
6.  **Act (Tool Call, Round 2):** The LLM again outputs a structured tool call.
      * **LLM Output:** `{"tool_call": "send_email", "arguments": {"to": "boss@example.com", "subject": "Weather in London", "body": "The current weather in London is 15°C and cloudy with 80% humidity."}}`
7.  **Observe (Tool Execution, Round 2):** The system calls the `send_email` function.
      * **Tool Result:** `{"success": true, "message_id": "xyz-789"}`
8.  **Think (LLM Reasoning, Final Round):** This success message is fed back to the LLM. The LLM now sees the full, successful execution of both tasks. It's *finally* ready to respond to the user.
9.  **Act (Final Response):** The LLM generates a natural language response.
      * **LLM Output:** "Done\! The weather in London is 15°C and cloudy. I've sent a summary to your boss."

In summary, **tools are simply functions, APIs, or services** that are "described" to an LLM. By giving the LLM the ability to *reason* about which tool to use, *request* its execution, and *observe* its output, we transform a passive "language model" into a powerful, "agentic" system that can perceive, process, and act upon the world.

</details>

-----

<details>
<summary><strong>Creating a tool</strong></summary>

### The Two Halves of a Tool: Implementation vs. Description

Creating a tool for an AI agent is a two-part process. It's not enough to just *write the code* (the implementation). You must also *describe that code* (the schema or definition) in a way the LLM can understand and use.

Think of it like hiring a new employee (the LLM) and giving them a new piece of equipment (the tool).

1.  **The Implementation:** This is the physical tool itself. It's the `def get_weather(city: str)` function written in Python. It's the `send_email` microservice. It has to exist, be reliable, and work.
2.  **The Description (Schema):** This is the *user manual* for the tool. This manual tells the employee (the LLM) *what* the tool is, *when* to use it, *what inputs* it needs (and in what format), and *what output* to expect.

If you provide the implementation but no description, the LLM has no idea the tool exists. If you provide the description but no implementation, the LLM will try to use a tool that doesn't work, leading to an error. You *must* have both, and they *must* match perfectly.

-----

### Part 1: The Implementation (The Code)

This is the "backend" logic of your tool. It's the actual code that performs the task. This code does *not* run inside the LLM; it runs on your server, in your application, or as a cloud function.

**Best Practices for Implementation:**

  * **Atomicity (Do One Thing Well):** Your tools should be granular. Don't create a single, massive tool called `do_everything_for_user(task)`. Create small, self-contained tools.

      * **Bad:** `manage_calendar(action, date, title, attendees)`
      * **Good:** `get_calendar_events(start_date, end_date)`, `Calendar(title, start_time, duration, attendees)`, `delete_calendar_event(event_id)`.
      * **Why?** This gives the LLM more flexibility and "Lego-like" blocks to combine. It also makes your descriptions (Part 2) much simpler and more accurate, leading to better tool selection by the LLM.

  * **Serializable Inputs & Outputs (JSON-Friendly):** The LLM communicates with your tools via *text* (specifically, structured formats like JSON). Your tool's function signature must accept simple, primitive data types (strings, numbers, booleans) or simple objects (lists and dictionaries). It must also *return* data in the same way.

      * **Don't:** `def process_file(file_handle)` or return a complex, in-memory Python object (`return MyCustomUserObject()`).
      * **Do:** `def process_file_by_path(filepath: str)` and `return {"user_id": 123, "name": "John", "is_active": true}`. The standard practice is to have your function return a dictionary or list, which can then be serialized to a JSON string.

  * **Robust Error Handling:** What happens if the user asks for the weather in "Faketown"? Your `get_weather` API will likely return a 404 error. Your *tool implementation* must catch this\!

      * **Bad:** The function crashes or throws an unhandled exception. The system returns a generic `{"error": "Internal Server Error"}` to the LLM, which the LLM can't understand or learn from.
      * **Good:** The function *catches* the 404 error and returns a *descriptive, structured error* in its normal JSON output.
        ```python
        def get_weather(city: str):
          try:
            response = requests.get(f"https://api.weather.com?q={city}")
            response.raise_for_status() # Raises HTTPError for 4xx/5xx
            return response.json()
          except requests.exceptions.HTTPError as e:
            if e.response.status_code == 404:
              # Return a structured error that is "in-band"
              return {"error": "City not found", "city_queried": city}
            else:
              return {"error": "API error", "details": str(e)}
        ```
      * **Why?** When the LLM receives `{"error": "City not found", "city_queried": "Faketown"}`, it can "think": *"Ah, the tool failed because 'Faketown' isn't a real city. I will now ask the user to clarify the city name."* This makes the agent resilient and self-correcting.

-----

### Part 2: The Description (The Schema)

This is the "user manual" for the LLM. This is arguably *more important* than the implementation, because a perfectly good tool will *never be used* if it's described poorly.

This description is almost always a JSON object that follows a specific format, like the **OpenAPI Function Specification**. This format is the *lingua franca* for defining tools, used by OpenAI, Anthropic, Google, and others.

Let's break down the schema for our `get_weather` tool.

```json
{
  "name": "get_weather",
  "description": "Get the current weather conditions for a single, specific city.",
  "parameters": {
    "type": "object",
    "properties": {
      "city": {
        "type": "string",
        "description": "The name of the city, e.g., 'San Francisco', 'Tokyo', or 'Paris'."
      },
      "unit": {
        "type": "string",
        "description": "The temperature unit to use. Either 'celsius' or 'fahrenheit'.",
        "enum": ["celsius", "fahrenheit"]
      }
    },
    "required": ["city"]
  }
}
```

Let's analyze each key:

  * **`name`**: `get_weather`

      * This *must* exactly match the name of the function your system will call. It's the unique identifier.

  * **`description`**: `"Get the current weather conditions for a single, specific city."`

      * **This is the single most important field.** This description is what the LLM uses to *decide* (via semantic matching) if this tool is the right one for the user's query.
      * This is "Prompt Engineering" for tools.
      * **A bad description:** `"weather"` (Too vague. For what? Forecast? Current? Multiple cities?).
      * **A good description:** `"Get the *current* weather conditions for a *single, specific* city."` This tells the LLM *exactly* when to use it (for "current" weather, not "forecast") and what its limitations are (one city at a time).
      * **A great description (for more complex tools):** `"Use this tool to get the 5-day weather forecast for a specific location. This tool cannot get historical data or real-time alerts. For current conditions, use the 'get_weather' tool."` This helps the LLM distinguish between multiple, similar tools.

  * **`parameters`**: This object (using JSON Schema) defines the *arguments* for the function.

      * **`type`: "object"**: This is the standard outer wrapper.
      * **`properties`**: This is a dictionary where each key is a parameter name.
          * **`city`**: This is the name of the first parameter.
              * **`type`: "string"**: Tells the LLM it must provide a string.
              * **`description`**: `"The name of the city, e.g., 'San Francisco', 'Tokyo', or 'Paris'"` This is a "prompt" for the *parameter*. It helps the LLM extract the correct entity from the user's query. The examples (`e.g., ...`) are extremely helpful.
          * **`unit`**: The second parameter.
              * **`type`: "string"**: It's a string.
              * **`description`**: `"The temperature unit to use. Either 'celsius' or 'fahrenheit'"`
              * **`enum`: `["celsius", "fahrenheit"]`**: This is a *constraint*. It tells the LLM that the *only* valid values for this string are `"celsius"` or `"fahrenheit"`. This prevents the LLM from trying to pass `"kelvin"` or `"degrees"`.
      * **`required`: `["city"]`**: This is a list of all parameters that are *not* optional.
          * This tells the LLM: "You *must* provide a value for `city`. If you don't have one, you should *ask the user for it*."
          * Since `unit` is *not* in this list, it's considered optional. The LLM might provide it, or it might not. Your *implementation* (Part 1) should be prepared for this and have a sensible default (e.g., `def get_weather(city: str, unit: str = "celsius")`).

By combining a robust **Implementation (Part 1)** with a precise and descriptive **Schema (Part 2)**, you create a reliable and effective tool that your AI agent can use to successfully accomplish its tasks.

</details>

-----

<details>
<summary><strong>Tool Syntax</strong></summary>

### The "Language" of Tool Use: A Conversational Protocol

"Tool Syntax" refers to the **specific, machine-readable format** that an LLM and a "tool-executing" system use to communicate. It's the *protocol* or *grammar* of the agentic conversation.

If "Creating a Tool" was about building a tool (a stapler) and writing its manual, "Tool Syntax" is the *standardized request form* the employee (LLM) must fill out to use the stapler ("Tool Request Form") and the *standardized report* the system provides after it's used ("Stapler Result Form").

This syntax is *not* just the JSON schema for the tool definition. It is the **entire multi-turn conversational structure** required to:

1.  Signal that a tool needs to be called.
2.  Specify which tool(s) and with what arguments.
3.  Receive the result(s) of the tool's execution.
4.  Continue the conversation armed with that new information.

While different model providers (OpenAI, Anthropic, Google) have slightly different *names* for their JSON keys or use different *markup* (like XML), the underlying *logic* is almost identical. The most common and foundational example is **OpenAI's "Function Calling" (now "Tool Calling") syntax**, which we will use to explain the concept.

### The 4-Step Syntactic Loop

Let's trace the *exact* messages that are passed back and forth in a tool-use "conversation." This is the core syntax.

**Scenario:** User says, "What's the weather in Boston?"
**Tools Available:** We have defined a tool named `get_current_weather(city, unit)`.

-----

#### Step 1: The User's Request (API Call \#1)

Your application makes an API call to the LLM. This message *must* include:

1.  The `messages` history (starting with the user's prompt).
2.  The `tools` list (the "manuals" for all available tools).

**Request to LLM API (Simplified):**

```json
{
  "model": "gpt-4-turbo",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "What's the weather in Boston?"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_current_weather",
        "description": "Get the current weather for a specific city.",
        "parameters": {
          "type": "object",
          "properties": {
            "city": {
              "type": "string",
              "description": "The city name, e.g., 'Boston'"
            },
            "unit": {
              "type": "string",
              "enum": ["celsius", "fahrenheit"],
              "default": "celsius"
            }
          },
          "required": ["city"]
        }
      }
    }
  ]
}
```

-----

#### Step 2: The LLM's Response (The Tool Call)

The LLM processes this request. It "thinks" (based on the `description`): *"The user's prompt 'What's the weather in Boston?' is a semantic match for the `get_current_weather` tool. The 'city' parameter should be 'Boston'."*

Instead of replying with text, the LLM replies with a special message. This message has `role: "assistant"` but *lacks* a `content` field. Instead, it has a `tool_calls` array.

**Response from LLM API (Simplified):**

```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": null, // NO text content
        "tool_calls": [ // The "Tool Request Form"
          {
            "id": "call_abc123", // A unique ID for this specific call
            "type": "function",
            "function": {
              "name": "get_current_weather",
              "arguments": "{\"city\": \"Boston\", \"unit\": \"fahrenheit\"}" // Note: Arguments are a JSON *string*
            }
          }
        ]
      }
    }
  ]
}
```

  * **Key Syntax:** The `tool_calls` array is the crucial part. It's a list because an LLM can be smart and call *multiple tools in parallel* (e.g., "What's the weather in Boston and the traffic in New York?").
  * **`id`**: `call_abc123` is a unique identifier. This is critical for matching the *request* to the *result* in the next step.
  * **`function.arguments`**: This is a **stringified JSON object**. Your code is responsible for *parsing* this string into a real JSON object. The LLM intelligently inferred that the user (being in the US) probably wants "fahrenheit" even though they didn't specify it (or it might have picked the default "celsius" - this is part of the LLM's "intelligence").

-----

#### Step 3: The System's Execution (API Call \#2)

Your application code (the "harness") receives this response. It must:

1.  See the `tool_calls` array.
2.  Iterate through each call in the array.
3.  Parse the `arguments` string.
4.  Call your *actual* (Part 1: Implementation) Python/JS/etc. function: `get_current_weather(city="Boston", unit="fahrenheit")`.
5.  This function runs, calls an external API, and gets a result. Let's say it returns a Python dict: `{"temperature": 68, "condition": "Sunny"}`.
6.  Your code *serializes* this result back into a JSON string: `"{\"temperature\": 68, \"condition\": \"Sunny\"}"`.

Now, you make a *second* API call to the LLM. This call **adds two new messages** to the history:

1.  The LLM's own `tool_calls` message from Step 2.
2.  A *new* message with `role: "tool"`, which contains the *result* of the execution.

**Request to LLM API (Call \#2 - The "Tool Result Form"):**

```json
{
  "model": "gpt-4-turbo",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "What's the weather in Boston?"
    },
    // Message 1: The LLM's previous turn (from Step 2)
    {
      "role": "assistant",
      "tool_calls": [
        {
          "id": "call_abc123",
          "type": "function",
          "function": {
            "name": "get_current_weather",
            "arguments": "{\"city\": \"Boston\", \"unit\": \"fahrenheit\"}"
          }
        }
      ]
    },
    // Message 2: The *new* message with the tool's result
    {
      "role": "tool",
      "tool_call_id": "call_abc123", // MUST match the ID from the call
      "name": "get_current_weather",
      "content": "{\"temperature\": 68, \"condition\": \"Sunny\"}" // The stringified JSON result
    }
  ],
  "tools": [ ... ] // The tool list is often sent again
}
```

  * **Key Syntax:** The `role: "tool"` message is the "Tool Result Form."
  * **`tool_call_id`**: This *must* match the `id` from the `tool_calls` object it is "replying" to. This is how the LLM keeps track of which result belongs to which request.
  * **`content`**: This is the raw (string) output from your function. If the tool failed, this is where you'd put the error: `"{\"error\": \"City not found\"}"`.

-----

#### Step 4: The LLM's Final Response (The Natural Language Answer)

The LLM receives this entire 4-message history. It "thinks":

1.  *"The user asked for the weather in Boston."*
2.  *"I decided to call `get_current_weather` with `city=Boston`."*
3.  *"The system executed that call (ID `call_abc123`)."*
4.  *"The system responded for `call_abc123` with the result `{\"temperature\": 68, \"condition\": \"Sunny\"}`."*
5.  *"Now I have all the information. I can formulate a natural language answer."*

The LLM API now sends its *final* response. This is a standard `role: "assistant"` message with a `content` field.

**Response from LLM API (Final Answer):**

```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "The current weather in Boston is 68°F and sunny."
      }
    }
  ]
}
```

Your application displays this `content` to the user, and the loop is complete.

**Syntax Variations:**

  * **Anthropic (Claude):** Uses a similar loop, but the syntax is different. The assistant's tool-use message is wrapped in `<invoke>` and `<tool_name>` XML tags. The system's response is wrapped in `<tool_result>` tags. The *logic* is identical.
  * **Google (Gemini):** Uses `functionCall` and `functionResponse` objects in its API. Again, the *logic* of the 4-step "request, call, execute, respond" loop is the *exact same*.

Understanding this 4-step, multi-message "syntactic loop" is the key to building any tool-using agent, regardless of the specific model provider.

</details>

-----

<details>
<summary><strong>Code Execution</strong></summary>

### The "Meta-Tool": A Tool to Create Tools

**Code Execution** is a special, extremely powerful, and potentially dangerous *type* of tool.

In all the previous examples (`get_weather`, `send_email`), the tools were **pre-defined**. The AI can *only* call the specific functions you've exposed to it. The logic inside `get_weather` is static and was written by a human developer.

**Code Execution** turns this on its head. With Code Execution, you give the LLM a *single* tool, often called `execute_python_code` or `run_code`. The *argument* for this tool is not a simple string like `"Boston"`; it's a *string of raw code* (e.g., a multi-line Python script).

Instead of just *using* tools, the LLM can *create its own logic* on the fly.

**Analogy:**

  * **Standard Tools:** Giving your employee a simple calculator. They can only use the `+`, `-`, `*`, `/` buttons you provided.
  * **Code Execution:** Giving your employee a *programmable graphing calculator* (like a TI-89). They can *write their own programs* (a `code` string) and then hit the "run" button (the `execute_code` tool) to solve problems you never could have anticipated.

This is the technology that powers features like OpenAI's "Advanced Data Analysis" (formerly Code Interpreter).

### How Code Execution Works (The Agentic Loop)

Let's walk through the loop. The setup is the same, but the LLM's "thought" process is different.

**Scenario:** User says, "What is the standard deviation of the numbers 5, 8, 12, 15? Also, plot the first 10 numbers of the Fibonacci sequence and save it to a file."

This is a request you would *never* create a pre-defined tool for. It's too specific.

**Tool Available:**

```json
{
  "name": "execute_python_code",
  "description": "A tool to execute a multi-line Python code snippet in a sandboxed environment. The code has access to common libraries like 'numpy', 'pandas', and 'matplotlib'. Any plots generated with 'matplotlib' will be saved, and their file paths returned.",
  "parameters": {
    "type": "object",
    "properties": {
      "code": {
        "type": "string",
        "description": "The Python code to execute. Must be complete, valid Python. Use 'print()' to output results."
      }
    },
    "required": ["code"]
  }
}
```

1.  **Observe (User Input):** User provides the complex prompt.

2.  **Think (LLM Reasoning):**

      * *"The user wants two things: a standard deviation calculation and a plot of the Fibonacci sequence."*
      * *"I cannot do math reliably. I do not have a pre-defined `calculate_std_dev` tool or a `plot_fibonacci` tool."*
      * *"However, I *do* have the `execute_python_code` tool. I am an expert at writing Python."*
      * *"I can write a single Python script that does *both* tasks. I will use the `numpy` library for the standard deviation (as the tool description says it's available) and `matplotlib` for the plot. I will `print` the standard deviation and save the plot."*

3.  **Act (Tool Call):** The LLM generates a `tool_calls` message. The `arguments` are not a simple JSON object, but a *string containing an entire Python script*.

    **LLM Output (Tool Call):**

    ```json
    {
      "tool_calls": [
        {
          "id": "call_python_789",
          "type": "function",
          "function": {
            "name": "execute_python_code",
            "arguments": "{\"code\": \"import numpy as np\\nimport matplotlib.pyplot as plt\\n\\n# Task 1: Calculate Standard Deviation\\nnumbers = [5, 8, 12, 15]\\nstd_dev = np.std(numbers)\\nprint(f'Standard Deviation: {std_dev}')\\n\\n# Task 2: Plot Fibonacci Sequence\\na, b = 0, 1\\nfib_numbers = []\\nfor _ in range(10):\\n    fib_numbers.append(a)\\n    a, b = b, a + b\\n\\nplt.figure()\\nplt.plot(fib_numbers, 'o-')\\nplt.title('First 10 Fibonacci Numbers')\\nplt.xlabel('Index')\\nplt.ylabel('Value')\\nplot_filename = '/mnt/data/fibonacci_plot.png'\\nplt.savefig(plot_filename)\\nprint(f'Plot saved to: {plot_filename}')\\n\"}"
          }
        }
      ]
    }
    ```

    *(Note: The `\n` are literal newline characters in the JSON string.)*

4.  **Observe (Tool Execution):** This is the *most critical* part. Your system's "tool harness" receives this request. It *cannot* just run this code on its own server. That would be a
    **massive security vulnerability** (a Remote Code Execution, or RCE, attack). The LLM could write `import os; os.system('rm -rf /')` and wipe your server.

    Instead, the system *must* execute this code in a **secure, isolated sandbox.**

    **The Sandboxing Process:**
    a.  Your system spins up a temporary, isolated environment (e.g., a **Docker container**, a **WebAssembly runtime**, or a **gVisor** instance).
    b.  This sandbox has *no* network access.
    c.  It has *no* access to the host file system, *except* for a single, temporary "scratch" directory (like `/mnt/data`).
    d.  It has strict limits on execution time (e.s., 10 seconds) and memory (e.g., 256MB).
    e.  The system "injects" the LLM's code into this sandbox and executes it.
    f.  The system *captures* all `stdout` (print statements) and `stderr` (error messages) from the sandbox.
    g.  After the code finishes (or times out), the sandbox is *completely destroyed*.

    In our example, the sandbox executes the script. The script's `stdout` is captured:

    ```
    Standard Deviation: 3.840572873853143
    Plot saved to: /mnt/data/fibonacci_plot.png
    ```

    The system also sees that a file was created at `/mnt/data/fibonacci_plot.png` and makes this file securely available, perhaps via a downloadable link.

5.  **Think (LLM Reasoning, Round 2):** The `stdout` and `stderr` are packaged into a JSON result and sent back to the LLM.

    **System Input to LLM (Tool Result):**

    ```json
    {
      "role": "tool",
      "tool_call_id": "call_python_789",
      "name": "execute_python_code",
      "content": "{\"stdout\": \"Standard Deviation: 3.840572873853143\\nPlot saved to: /mnt/data/fibonacci_plot.png\\n\", \"stderr\": \"\", \"files_generated\": [\"/mnt/data/fibonacci_plot.png\"]}"
    }
    ```

6.  **Act (Final Response):** The LLM receives this result. It "thinks": *"My code ran successfully. The standard deviation was printed to `stdout`, and the plot was saved. I can now give the user both results."*

    **LLM Final Answer:** "I've completed your requests\!

      * The standard deviation of the numbers 5, 8, 12, and 15 is approximately **3.84**.
      * I've also generated a plot of the first 10 Fibonacci numbers. You can download it here: [Link to fibonacci\_plot.png]"

### The Power and The Peril

  * **The Power (Infinite Flexibility):** The LLM is no longer constrained by your imagination. It can perform complex data analysis, create visualizations, transform files, run algorithms, and solve virtually any problem that can be solved with code. It can chain logic, define its own helper functions, and correct its own mistakes (if the `stderr` shows a bug, it can *rewrite the code and try again*).
  * **The Peril (Security):** This is **Remote Code Execution as a Service**. A non-negotiable prerequisite is a **robust, battle-tested sandboxing environment.** Building this sandbox is a highly complex security-engineering task. You must protect against:
      * **Host Compromise:** `os.system` attacks.
      * **Data Exfiltration:** Network calls to `http://evil.com/steal?data=...`
      * **Denial of Service (DoS):** `while True: pass` (CPU) or `[1] * 999999999` (memory).
      * **Data Contamination:** One user's agent *must never* be able to see another user's files or data.

Code Execution is the most powerful tool in the agentic AI arsenal, but it demands the most rigorous engineering and security-first mindset to implement safely.

</details>

-----

<details>
<summary><strong>MCP (Model Context Protocol)</strong></summary>

### The "Next Generation": Beyond Chat-Based Tools

**MCP (Model Context Protocol)** is a *proposed standard* and a *new design pattern* that fundamentally re-imagines how AI models interact with applications and tools. It's a response to the "clunky" and "inefficient" nature of the "Tool Syntax" (the 4-step chat loop) we discussed earlier.

**The Problem with the "Chat" Metaphor:**

The "Tool Syntax" we analyzed (the `tool_calls` / `role: "tool"` loop) is based on a **chat metaphor**. The *entire history* of the interaction—every user prompt, every LLM thought, every tool call, every tool result—is appended to a long, growing list of messages. This "chat log" is then re-sent to the LLM *every single time* it needs to "think."

This design has two massive problems:

1.  **Context Window Bloat (Inefficiency):** If a user is editing a document and makes 50 small changes, the "chat log" model would require re-sending the *entire document* and the *history of all 50 changes* on the 51st request. The context window (the LLM's "short-term memory") fills up almost instantly, it's slow, and it's (API-wise) expensive.
2.  **Statelessness (A Bad "User Experience" for the LLM):** The LLM is still fundamentally stateless. It doesn't "edit a document." It "sees a history where a document was edited." It can't "hold" a file, "look" at a spreadsheet, or "work on" a codebase. It can only "propose a tool call" (like `edit_file`) and then be "shown the new file" in the *next* turn. This is a very clunky, indirect way to perform work.

**Analogy:**

  * **Traditional Tool Calling:** Is like trying to co-edit a Google Doc with your assistant *via text message*.

      * You (User): "Hey, can you look at the doc?"
      * System: *Texts you the entire doc.*
      * You: "Okay, on line 5, change 'apple' to 'orange'."
      * Assistant (LLM): "Okay, I'll *propose* a change\!" (Sends a `tool_call`)
      * System: *Runs the tool, makes the change.*
      * System: *Texts you the **entire, new version** of the doc.*
      * You: "Great. Now on line 10..."
        This is obviously a terrible, slow, and inefficient way to work.

  * **MCP (Model Context Protocol):** Is like *actually sharing the Google Doc*.

      * Both you (the User/System) and the Assistant (the LLM) are looking at the *same, shared document* (the "Context").
      * When the LLM wants to make a change, it doesn't "chat" about it. It *directly mutates* the shared document.
      * **LLM's "Tool Call":** `set(path="/document/lines/5", value="orange")`
      * This "operation" (a "diff" or "patch") is tiny, fast, and applies directly to the shared state. The User Interface (UI) and the LLM are always in sync.

### The Core Idea of MCP: A Shared "Context Object"

MCP (and similar ideas, like "stateful models") replaces the **"Chat Log" (a List of Messages)** with a **"Context Object" (a structured JSON document)**.

This "Context Object" *is* the application's state. It's not just a *history* of what happened; it's the *living state* of the world *right now*.

**Example: A "To-Do List" Application**

Instead of a chat history, the LLM is given a *single* JSON object representing the entire app state:

**Initial Context Object:**

```json
{
  "app_name": "My To-Do App",
  "current_user": { "id": "user_123", "name": "Alice" },
  "todos": [
    { "id": "t1", "task": "Buy milk", "done": false },
    { "id": "t2", "task": "Call mom", "done": true }
  ],
  "chat_log": [
    { "role": "system", "content": "You are a to-do list assistant." }
  ]
}
```

  * Notice how the `chat_log` is now *part of* the shared state, not the *entire* state.

**The User's Turn:**
The user *doesn't* send a "chat message." They *interact* with the UI. The user clicks a button to add a new to-do.

  * **User (via UI):** Types "Feed the cat" and hits Enter.

**The System's Turn (Mutating the Context):**
Your application's *frontend* code captures this event. It *mutates* the Context Object (e.g., using a "patch" operation) and tells the *system* (and the LLM) about the change.

**New Operation (from User):**

```json
{ "op": "insert", "path": "/todos/-", "value": { "id": "t3", "task": "Feed the cat", "done": false } }
```

**The LLM's "Turn" (Reacting to a Change):**
The LLM isn't "called" in a request/response loop. It's *observing* the Context Object. It sees this new `insert` operation.

Now, let's say the user types in the chat box (which is just *another part* of the Context Object):

  * **User (via UI):** "Hey, can you please mark 'Buy milk' as done?"

**The System's Turn:**

```json
{ "op": "insert", "path": "/chat_log/-", "value": { "role": "user", "content": "Hey, can you please mark 'Buy milk' as done?" } }
```

**The LLM's Turn (Reacting and Acting):**
The LLM "wakes up" because the `/chat_log` part of its context has changed. It analyzes the new message. It "thinks":

  * *"The user wants to mark 'Buy milk' as done."*
  * *"I see the 'Buy milk' task in my context at `/todos/0`."*
  * *"Its 'done' property is at `/todos/0/done`."*
  * *"I will *not* call a `mark_todo_done` tool. I will *directly mutate* the context."*

**The LLM's "Tool Call" (An MCP Operation):**
Instead of a `tool_calls` message, the LLM emits a *list of operations* (using a standard like **JSON-PATCH**) to mutate the context.

**LLM Output (Operations):**

```json
[
  {
    "op": "replace",
    "path": "/todos/0/done", // Finds the task at index 0
    "value": true
  },
  {
    "op": "insert",
    "path": "/chat_log/-",
    "value": { "role": "assistant", "content": "Okay, I've marked 'Buy milk' as done!" }
  }
]
```

These operations are *tiny* (bytes of data) compared to resending the whole history.

**The System's Turn (Applying the Patch):**
Your system (and your frontend UI) receives this list of operations. It applies them to its *local* version of the Context Object.

1.  The `todos[0].done` state is changed to `true`.
2.  A new message from the assistant is added to the chat log.

The UI, which is *also* watching this state, *instantly re-renders* to show the checkbox for "Buy milk" becoming checked and the assistant's new message appearing in the chat window.

### Why This is a "Game Changer"

1.  **Massive Efficiency:** The LLM only sends and receives "diffs" (changes), not the entire state. This is *dramatically* faster, cheaper, and scales to *enormous* contexts (e.g., an entire codebase, a large spreadsheet).
2.  **True Stateful Interaction:** The LLM is no longer a "stateless chatbot." It's a "stateful co-worker" that is *directly* interacting with the application's data. It "sees" the same reality the user sees.
3.  **Richer Applications:** This pattern unlocks applications that are *impossible* with the chat-tool-loop.
      * **AI Code Editors:** The Context is the file tree and the text of the open file. The LLM's "operations" are text-insertion/deletion patches to the file buffer.
      * **AI Spreadsheet Tools:** The Context is the grid of cells. The LLM's operation is `set("/cells/A5/formula", "=SUM(A1:A4)")`.
      * **AI Game NPCs:** The Context is the "world state" (player location, inventory, etc.). The LLM's operations are `set("/player/hp", 90)` or `insert("/player/inventory/-", "health_potion")`.

MCP is a *protocol* to **standardize** this new, more powerful way of thinking. It aims to create a *single language* of "context operations" (like JSON-PATCH) that *all* models (OpenAI, Google, Anthropic, open-source) can "speak." This would allow you to build one "MCP-compliant" application (like a code editor) and then swap out the "brain" (the LLM) behind the scenes without changing your application logic.

It's the future of building truly integrated, agentic AI systems, moving far beyond the limits of a "chatbot-with-functions."

</details>

## Acknowledgement

This repository is for my personal educational and learning purposes only. All course materials, concepts, and project ideas are the intellectual property of **DeepLearning.AI**. I do not claim any ownership over the course curriculum. Please enroll in the official course to access the complete materials and support.