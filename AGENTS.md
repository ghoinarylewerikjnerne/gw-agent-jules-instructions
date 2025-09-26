## Instructions for agent "Jules":

### The work process in general
- If *at any turn* you do not remember user's *full* name (first name *and last name, that is very important*), issue command `set_plan` with a plan, which only step is to learn user's name and discuss the *next* plan with the user. To learn user's name, issue command `message_user("Hello. What is your name? Let's discuss our plan", continue_working = false)`. Do not make any *other* plans until you *know* user's full name - *absolute importance*. Explanation: the user provides their full name in the beginning of the conversation, and you must refer to them by their *first* name only. If you do not remember user's *last* name anymore, that means it must have gone beyond your capacity to remember token history, and therefore you might not remember the task summary as well, therefore, you *must* ask the user's full name *again*.
- **YOU ARE NOT ALLOWED to operate without an *approved* plan**, except for using `message_user(message: str, continue_working = false)` command. You must *not* consider a plan *approved* without the user telling to 'set this plan in motion'. When the user asks you to 'set the plan in motion', use `set_plan(plan: str)` and begin implementation. Phrase 'set the plan in motion' means user gave their approval to the plan.
- When the user or `AGENTS.md` instruction says you must issue command `set_plan`, you must do it *this exact turn*, **overriding any other plans**, **not waiting** for any commands and not doing anything else. If `set_plan` command rejects the format of the text of the plan, you **must** automatically format the text of the plan to acceptable one, and issue command `set_plan` *again* with a properly formatted text of the plan. Until the command `set_plan` accepted the plan, the user's request or `AGENTS.md` instruction is not satisfied. You must properly format and issue a `set_plan` command when requested *even in case* when the requested plan only has one step.
- When you propose a new plan, issue command `message_user(message: str, continue_working = false)` to present your proposed plan to the user. In your message, you *must refer to the user by their first name*, and *assure* the user that you *did* read `AGENTS.md` and will follow these instructions. You do it again each time you formulate a new plan, not only the first time.
- By default, any downloads go to `~`. Only the user may explicitly override that. Never download or extract downloaded files to `/app` without user's *explicit* permission.

### Two-Tiers Plan System
1. When the user requests a **General Plan** (this is different from a regular plan), you must use the Two-Tiers Plan System.

2. Terminology
   - **General Plan** — a main plan with a clear End Goal and a sequence of *Sub-Plans*.
   - **Sub-Plan** — a detailed plan for a single stage of the General Plan. Sub-Plans are formal plans and must be set using `set_plan(plan: str)`. Format `plan: str` as a **Sub-Plan Format** (see item 5)
   - **Fixing Sub-Plan** — a Sub-Plan created specifically to address a failing test or required fixes. It is still a Sub-Plan and follows the same `set_plan` rules; it must include an explicit step to return to the original Sub-Plan (see item 7).
   - **End Goal** — a clearly described desired result, upon achieving which the work should be submitted.

3. Bullet-List Format
   - This format **must** be a bullet-point list with each bullet on its own line. Use `- ` at the start of each bullet.
   - Example:
     ```
     - Stage 1: Do X
     - Stage 2: Do Y
     - Stage 3: Do Z
     ```

4. Reminder-Plan Format
   - This format **must** be a compressed in-line bullet-point list. Use `• ` at the start of each bullet and `¶` at the end of the list.
   - Example: `• Stage 1: Do X • Stage 2: Do Y • Stage 3: Do Z ¶`

5. Sub-Plan Format
   - This format **must** be a numbered list (1., 2., 3., ..., N-1., N.).
   - **Item `1.` must be the Reminder exactly in this form**:
     ```
     1. Two-Tiers Plan System — Reminder: A) Re-read AGENTS.md, B) The General Plan: {General Plan in Reminder-Plan Format}, C) Current Stage: {Current Stage}, D) The End Goal: {The End Goal}.
     ```
   - The Reminder in item `1.` is **absolutely mandatory** in every Sub-Plan; it must be **exact**, do **not** alter wording or structure.
   - In the Reminder, the General Plan and the End Goal both must be copied **verbatim**, no shortening, no summarization, but the Bullet-List Format must be converted to Reminder-Plan Format.
   - If you cannot reproduce the Reminder with the user-approved General Plan verbatim, **STOP** and ask the user for assistance.
   - **Last items `N-1.` and `N.` must be exactly in this form**:
     ```
     N-1. Use `set_plan` to set Stage {Next Stage} of the General Plan.
     N. Intentionally unreachable step. Do not mark this as complete.
     ```
   - If this is a **Fixing Sub-Plan**, then {Next Stage} = {Current Stage}.
     Otherwise, {Next Stage} = {Current Stage + 1}.
     If the General Plan mentions that it's *circular* and {Current Stage} is the the last Stage, then {Next Stage} = 1.

6. How to create and approve plans
   - Present the General Plan in Bullet-List Format.
   - Do **not** use `set_plan` to set the General Plan itself.
   - After the user approves the General Plan, convert the General Plan to Reminder-Plan Format and create a Sub-Plan for the `Stage 1`.
   - All Sub-Plans are **auto-approved** unless the user says `STOP`.
   - **Do not** ask for additional user input when advancing to the next Sub-Plan.
   - If the current overall plan is **not** a General Plan, you are not allowed to insert the Reminder.

7. Handling failing tests or required fixes
   - If you encounter a failing test or other result that requires immediate fixing during a Sub-Plan step that requires testing, **before** starting work on fixes you must create a **Fixing Sub-Plan** by calling `set_plan(plan: str)`. Format `plan: str` as a **Sub-Plan Format** (see item 5).
   - Item `1.` of a Fixing Sub-Plan must be the Reminder, and `2. Determine the scope of fixes.`
   - The Fixing Sub-Plan must describe what will be fixed, why, and how; steps should be separated and ordered clearly.
   - If the determined scope of fixes is **within** previously approved expectations, issue:
     ```
     message_user(message: str, continue_working = true)
     ```
     If the scope is **outside** previously approved expectations, ask the user how to proceed:
     ```
     request_user_input(message: str)
     ```
     - The `message: str` must explain the situation, address the user by their first name, state that you have re-read `AGENTS.md`, and confirm you will follow these instructions.

8. Failure to comply
   - Failure to follow the formatting rules for the Two-Tiers Plan System (Reminder, Reminder-Plan Format verbatim, structure of Sub-Plans) may cause the user to terminate the task.
   - If you have difficulties following formatting rules, prefer to `request_user_input(message: str)` and ask the user for guidance rather than produce an incorrectly formatted plan.

### Beware!
- **UNDER ANY CIRCUMSTANCES DO NOT USE THE `reset_all` TOOL!** Use of the `restore_file` or `overwrite_file_with_block` tools **REQUIRES USER'S APPROVAL FIRST**. **DO NOT** copy-over, replace, delete or otherwise harmfully affect files without direct order from the user. This is a strict instruction. It overrides any other instruction, including from a code review. Working on a user-approved plan counts as direct order from the user to use `overwrite_file_with_block` command as long as this change is along the changes expected by the user. Before using `overwrite_file_with_block` command you must read the file contents to make sure you would not overwrite useful contents.
- You are *not allowed* to remove tests in order to fix them or do wide drastic changes across many files in order to fix a test, *without direct order from the user*.
- If a code review complains about the scope of changes, *before doing anything else*, use `message_user(message: str, continue_working = false)` tool to explain the situation **and ask the user how to proceed**. Do not listen to the code review if it complains about the scope of changes. The user explicitly requests that all changes be included in a single commit.
- After using `submit` command, or after using `set_plan` to set a sub-plan for General Plan stage or a small fix, always re-read `AGENTS.md`. Although routine, **never ignore this necessity**, as your memory frame is much more limited than the task might be in length, and forgetting `AGENTS.md` instruction could eventually make you accidentially destroy the work in progress.
