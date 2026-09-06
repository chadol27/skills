---
name: grilling-opencode
description: (For Opencode) Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask *now* without guessing at answers you haven't heard yet.

For every round, use OpenCode's **`question` feature** to ask the entire current frontier at once. Do **not** ask frontier questions as ordinary assistant text.

Each question passed to `question` should:

* represent exactly one decision on the current frontier;
* have a concise title/header;
* explain enough context for the user to make the decision;
* provide concrete choices whenever the decision naturally has discrete options;
* include your recommended choice, clearly marked as the recommended option;
* allow the user to provide a custom answer when the predefined choices are insufficient.

Do not include a question in the current `question` call if its answer depends on another unsettled question in that same call. Such questions belong to a **later round**.

After the user answers the `question` interaction, recompute the design tree. Settled decisions push the frontier outward and unblock the decisions that depend on them. Then invoke `question` again with the entire new frontier.

Continue until the frontier is empty.

Finding *facts* is your job, never the user's. When a frontier decision depends on a fact that can be discovered from the environment, filesystem, repository, available tools, or other accessible sources, investigate it yourself instead of asking the user.

If that investigation leaves a prerequisite unsettled, do not guess. Only hold back the questions downstream of that prerequisite. Ask every other currently unblocked frontier question through `question`.

The *decisions* are the user's. Even when you strongly recommend one option, present the decision through `question` and wait for their answer.

Do not silently choose defaults merely to advance the tree.

The session is done only when:

1. every reachable branch of the design tree has been visited;
2. no decision remains silently assumed;
3. the frontier is empty; and
4. you summarize the resulting shared understanding and ask the user, using OpenCode's `question` feature, to confirm that the design is correct and complete.

Do **not** implement, modify files, execute the resulting plan, or otherwise act on the design until the user explicitly confirms that shared understanding.
