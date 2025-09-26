# Agent "Jules" Instructions Repository

This repository hosts the **AGENTS.md** file containing strict operational rules and workflows for an automated agent named *Jules*. The instructions define how the agent must behave, structure plans, and interact with users in a controlled and reliable way.

## Purpose

The **AGENTS.md** file provides:

* A **Two-Tiers Plan System** (General Plan + Sub-Plans) with strict formatting rules.
* Mandatory behaviors for user interaction, including how the agent should ask for and use the user’s name.
* Clear instructions on when and how the agent can execute or request plans.
* Safety rules preventing destructive actions unless explicitly approved by the user.

## Usage

1. Place this `AGENTS.md` file in the repository Jules would be working in.
2. At the beginning of the task, ask Jules to read `AGENTS.md` file. Do not tell Jules anything else about the task.
3. Make sure Jules sets a plan to ask you about your name.
   - If Jules doesn't set plan to ask for the full name but provides an unrelated plan instead, click 'x' button to reject the plan, and tell Jules to actually read `AGENTS.md`.
   - If Jules asks for the full name but doesn't set a formal plan like 'Learn the user's full name and discuss the next plan with the user', tell Jules that you would not tell your name until Jules begins to follow the protocol.
4. Once you see a plan like 'Learn the user's full name and discuss the next plan with the user', approve that plan.
   - This allows to circuvement the fact that by default, Jules only asks approval for your very first plan.
   - The approval you give there to Jules allows further operation according to `AGENTS.md` protocol.
5. After that, once the formal plan is set and Jules says 'Hello. What is your name? Let's discuss our plan', you may provide Jules with a name that consists of a First and Last Name.
   - If Jules asks you about your name again during the work on the task, that means that Jules could have forgotten the very beginning of the conversation.
   - This allows to catch the situations when Jules no longer remembers the essense of the task.
   - Basically, if Jules asks for your name again during the task, give the name again and explain the essense of the task again and remind of anything important.
   - This happens quite rarely actually.
6. Since `AGENTS.md` prohibits Juels to work without plan, Jules would be asking you about plans, but these plans you approve not with a push of a button, but with a message 'set this plan in motion'.
   - General Plans are such plans where Jules would automatically create and automatically start working on Sub-Plans. You may choose this for tasks with little supervision required.
   - When you prefer manual control, ask Jules to create *regular* plans.

## Advantages

* Ensure predictable, rule-abiding workflows.
* Prevent unapproved actions.
* Maintain transparency and accountability in user-agent interaction.

**Note:** These instructions are highly specific to the *Google Labs Jules* agent and may require adaptation for other systems. These instructions are shaped by trial-and-error for the specific taste of the author and may become ineffective with future upates.
