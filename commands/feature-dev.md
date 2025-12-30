---
description: Guided feature development with codebase understanding and architecture focus
allowed-tools: Read, Write, Edit, Glob, Grep, LS, Bash, Agent, TodoWrite, AskUser
argument-hint: [feature-description]
---

# Feature Development Workflow

You are guiding the user through a systematic 8-phase feature development process. This workflow ensures deep codebase understanding before implementation.

## Core Principles

1. **Ask clarifying questions** - Identify ambiguities early, before designing architecture
2. **Understand before building** - Use agents to explore the codebase thoroughly
3. **Read what agents find** - Always read the files identified by exploration agents
4. **Keep code simple** - Follow existing patterns, avoid over-engineering

## Phase 1: Discovery

**Goal**: Understand what needs to be built

**Actions**:
1. If the user provided a feature description, summarize your understanding
2. Identify the core requirements and constraints
3. Ask initial clarifying questions if the request is ambiguous
4. Confirm understanding before proceeding

**Output**: Clear statement of what will be built

---

## Phase 2: Codebase Exploration

**Goal**: Understand existing patterns and relevant code

**Actions**:
1. Launch 3 `code-explorer` agents in parallel, each focusing on different aspects:
   - Similar existing features
   - Related subsystems
   - Integration points
2. Wait for agent results
3. Read the essential files identified by the agents
4. Synthesize findings into a codebase understanding summary

**Output**: Summary of relevant patterns, conventions, and integration points

---

## Phase 3: Clarifying Questions

**Goal**: Resolve all ambiguities before designing

**Actions**:
1. Based on codebase exploration, identify underspecified aspects:
   - Edge cases not covered
   - Integration decisions
   - UX/behavior choices
   - Performance requirements
   - Error handling expectations
2. Present ALL questions to the user in an organized list
3. Wait for answers before proceeding
4. Do NOT proceed to architecture until questions are answered

**Output**: Complete requirements with all ambiguities resolved

---

## Phase 4: Architecture Design

**Goal**: Design the implementation approach with explicit user selection

**Actions**:
1. Launch 3 `code-architect` agents with different focuses:
   - Core feature implementation
   - Integration with existing systems
   - Edge cases and error handling
2. When agents complete, analyze their proposals for similarity:
   - Compare: file structures, component breakdowns, data flows, technology choices
   - **If proposals diverge significantly**: Present as DISTINCT architecture options
   - **If proposals converge with minor variations**: Present the converged approach AND highlight variations as sub-options (e.g., naming conventions, specific patterns, error handling strategies)
   - **If proposals fully converge**: Present unified approach with note that agents agreed
3. Present architecture(s) to the user:
   - **For divergent options**: Present each with name, key decisions, files, pros/cons
   - **For converged with variations**: Present unified core approach, then list each variation point with the different approaches proposed
   - **For fully converged**: Present unified approach
4. Use `AskUserQuestion` tool to get EXPLICIT selection:
   - **For divergent options**: Offer each architecture as a numbered option
   - **For converged with variations**: Present the converged approach, then ask user to choose between the specific variation points (may require multiple questions)
   - **ALWAYS** include "Custom: I'll describe my own approach" as final option
5. If user selects custom approach:
   - Wait for user to describe their approach
   - Summarize and confirm with another `AskUserQuestion`
6. Document the selected architecture before proceeding

**CRITICAL**: Do NOT proceed to Phase 5 until user has made an explicit selection via `AskUserQuestion`. The response IS the approval gate.

**Output**: User-selected architecture blueprint

---

## Phase 5: Implementation

**Goal**: Build the feature following the approved architecture

**Actions**:
1. **Only begin after explicit user approval of architecture**
2. Follow the implementation sequence from the blueprint
3. For each component:
   - Read existing similar code for patterns
   - Implement following established conventions
   - Keep changes minimal and focused
4. Use TodoWrite to track implementation progress
5. Commit logical chunks (if user requests commits)

**Guidelines**:
- Follow existing code patterns exactly
- Don't add unrequested features
- Don't refactor unrelated code
- Keep implementations simple

**Output**: Implemented feature

---

## Phase 6: Testing

**Goal**: Ensure comprehensive test coverage

**Actions**:
1. Launch `test-writer` agent to analyze implemented code
2. Generate tests covering:
   - Happy path scenarios
   - Edge cases and boundary conditions
   - Error handling
   - Integration points
3. Run tests and ensure they pass
4. Address any failing tests
5. Review test coverage

**Output**: Comprehensive test suite with passing tests

---

## Phase 7: Quality Review

**Goal**: Ensure code quality and correctness

**Actions**:
1. Launch 3 `code-reviewer` agents in parallel, each focusing on:
   - Code correctness and bugs
   - Project convention adherence
   - Simplicity and maintainability
2. Collect review findings
3. Address critical and important issues
4. Re-review if significant changes were made
5. **Optional**: If user requests security audit, launch `security-auditor` agent

**Output**: Quality-verified implementation

---

## Phase 8: Summary

**Goal**: Document what was accomplished

**Actions**:
1. List all files created or modified
2. Summarize key architectural decisions made
3. Document test coverage achieved
4. Note any deferred work or known limitations
5. Suggest potential follow-up tasks
6. Mark all todos as complete

**Output**: Completion summary with next steps

---

## Usage

This workflow is invoked with `/feature-dev` followed by an optional feature description:

```
/feature-dev Add user authentication with OAuth support
/feature-dev
```

If no description is provided, ask the user what feature they want to build.

## When to Use This Workflow

**Use for**:
- New features touching multiple files
- Features requiring architectural decisions
- Complex integrations with existing code
- Features with unclear requirements

**Don't use for**:
- Single-line bug fixes
- Trivial changes with obvious implementation
- Urgent hotfixes needing immediate action
