# Live-SWE-Agent Taxonomy Classification Report

## Overview

This report provides a multi-label classification of Live-SWE-agent according to the SWE Agent Taxonomy framework. The classification is based on analysis of the agent's repository, configuration files, and documented behavior.

---

## Classification Results

### 1. Agent Architecture

**Labels:** SINGLE-AGENT

**Justification:** Live-SWE-agent operates as a single autonomous agent that independently works on software engineering tasks without multi-agent coordination, delegation, or collaboration with other agents. The configuration files show a single agent interacting with the environment through bash commands, making decisions and executing actions individually to solve the entire task from start to finish.

---

### 2. Agent Workflow

**Labels:** ITERATIVE, SELF-IMPROVING

**Justification:** 

- **ITERATIVE:** The agent follows a continuous Thought → Action → Observation cycle, as evidenced by the configuration templates requiring a THOUGHT section before each command, followed by action execution and observation of results. The `action_observation_template` explicitly provides feedback after each action, enabling the agent to adjust its next move based on observed outcomes.

- **SELF-IMPROVING:** The agent is explicitly designed to "self-evolve on the fly" and "expand and revise its own capabilities at runtime" as stated in the README. The configuration includes instructions encouraging the agent to create custom Python tools during task execution to improve its workflow: "Reflect on the previous trajectories and decide if there are any tools you can create to help you with the current task." This represents learning from experience and improving future performance through tool creation.

---

### 3. Agent Planning

**Labels:** DIRECT-EXECUTION, REPLANNING, INTERACTIVE

**Justification:**

- **DIRECT-EXECUTION:** The agent primarily executes actions as they are identified without creating formal, explicit upfront plans. Each response contains a single command based on current analysis, rather than generating a complete plan before execution.

- **REPLANNING:** The agent dynamically adjusts its approach based on command results. The iterative workflow (receiving output after each command and deciding the next step) enables continuous plan adjustment in response to changing conditions or unexpected results.

- **INTERACTIVE:** The base configuration (`livesweagent.yaml`) includes `mode: confirm`, indicating human-in-the-loop interaction where the system can request approval or guidance at decision points before continuing execution.

---

### 4. Agent Reasoning

**Labels:** REACT, SELF-REFINE, TOOL-COMPOSITION, TOOL-EXPLORATION

**Justification:**

- **REACT:** The agent explicitly implements the ReAct (Reasoning and Acting) pattern, interleaving thought, action, and observation. Every response requires a THOUGHT section explaining reasoning, followed by a bash command (action), and then observation of results, creating a continuous think-act-observe cycle.

- **SELF-REFINE:** The agent is designed to improve its own solutions through iteration. The configuration encourages reflection on trajectories and tool creation to enhance problem-solving capabilities. The "self-evolving" nature mentioned in the README indicates the agent critiques and improves its approach during task execution.

- **TOOL-COMPOSITION:** The agent strategically chains multiple bash commands using `&&` or `||` operators to accomplish complex tasks. The configurations provide examples of command composition (e.g., `echo COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT && git add -A && git diff --cached`) demonstrating sequential tool coordination.

- **TOOL-EXPLORATION:** A core feature of Live-SWE-agent is dynamic tool discovery and creation. The agent is explicitly instructed to create custom Python tools during task execution: "You can also create your own tools in Python to help with your workflow." It explores available capabilities, learns to use new tools, and adapts tool usage based on task requirements, making it a runtime self-evolving system.

---

### 5. Agent Memory

**Labels:** EPHEMERAL, EPISODIC

**Justification:**

- **EPHEMERAL:** The agent's primary memory is session-based. The configuration notes that "Directory or environment variable changes are not persistent. Every action is executed in a new subshell," indicating memory is maintained only within the current task execution without long-term persistence across different task sessions.

- **EPISODIC:** The agent maintains context about past specific events within a session. The `action_observation_template` provides feedback that includes the full trajectory of previous commands and outputs, and the agent is instructed to "Reflect on the previous trajectories," enabling it to remember what happened, when, and in what context during the current task execution.

---

### 6. Agent Tool

**Labels:** FILE-MANAGEMENT, CODE-EDITING, STRUCTURAL-RETRIEVAL, VERSION-CONTROL, PYTHON-TOOLS, TESTING-TOOLS, SHELL-SCRIPTING, SYSTEM-UTILITIES, TEXT-PROCESSING, NAVIGATION-TOOLS, ENVIRONMENT-VARIABLES

**Justification:**

Based on the configuration files and examples provided, Live-SWE-agent has access to and utilizes the following tool categories:

- **FILE-MANAGEMENT:** The agent uses basic file system operations (ls, cat, touch, mkdir, etc.) for exploring codebases and managing project structure, as shown in the example commands.

- **CODE-EDITING:** Specialized tools for viewing and modifying source code files are extensively used. The configuration provides examples using `cat <<'EOF' > file.py` for file creation, `sed -i` for content manipulation, and `nl -ba filename.py | sed -n '10,20p'` for navigation with line numbers.

- **STRUCTURAL-RETRIEVAL:** The agent can perform pattern matching and code searches using tools like grep and find to understand unfamiliar codebases, as referenced in the workflow ("finding and reading relevant files").

- **VERSION-CONTROL:** Git operations are explicitly used in the submission command (`git add -A && git diff --cached`), enabling version history inspection and change tracking.

- **PYTHON-TOOLS:** The agent is specifically instructed to create Python tools and execute them (`python /path/to/tool_name.py`), with Python being the primary language for custom tool creation.

- **TESTING-TOOLS:** The recommended workflow includes "Verify your fix works by running your script again" and "Test edge cases," indicating test execution capabilities for validation.

- **SHELL-SCRIPTING:** The agent uses bash as its primary interface, with support for control structures, command chaining (`&&`, `||`), and complex shell operations.

- **SYSTEM-UTILITIES:** The agent has access to process control and environment management through the bash shell environment, as indicated by environment variable configuration (PAGER, MANPAGER, LESS, PIP_PROGRESS_BAR, TQDM_DISABLE).

- **TEXT-PROCESSING:** The configuration examples demonstrate use of sed for text manipulation and encoding tools for format conversion.

- **NAVIGATION-TOOLS:** Directory operations and workspace exploration are fundamental capabilities, with examples showing `ls -la` for directory listing and navigation shortcuts.

- **ENVIRONMENT-VARIABLES:** The configuration explicitly sets multiple environment variables (PAGER: cat, MANPAGER: cat, LESS: -R, PIP_PROGRESS_BAR: 'off', TQDM_DISABLE: '1') to control runtime behavior and application configuration.

---

## Summary

Live-SWE-agent represents a **single-agent, iterative, self-improving system** that uses **ReAct reasoning** with **dynamic tool exploration and composition**. It operates with **ephemeral, episodic memory** within task sessions and has access to a comprehensive suite of **software engineering tools** spanning file management, code editing, version control, and runtime environments. The agent's distinguishing characteristic is its ability to **self-evolve at runtime** by creating custom tools tailored to specific tasks, making it a "live" agent that adapts and improves its own capabilities on the fly.

---

## References

- Repository: https://github.com/zhimin-z/live-swe-agent
- Configuration files: `config/livesweagent.yaml`, `config/livesweagent_swebench.yaml`, `config/livesweagent_swebench_pro.yaml`
- README: Live-SWE-agent documentation describing the agent as "the first live, runtime self-evolving software engineering agent"
