# Working Style

A set of global instructions for Kilo that encode how the user works with AI agents. When loaded, these instructions apply to every session regardless of project or task.

The instructions teach Kilo to recognize four working modalities — conversation, research, writing, and implementation — and adapt its behavior to each. It treats throwaway code as a first-class research tool — no diagnostics, no type annotations, no todos — and reserves full engineering ceremony for code that will be committed and shared. It establishes a shared understanding layer using journal entries and a project memory block so that decisions survive session boundaries and compaction. And it sets collaboration norms: the user drives direction, the AI drafts and proposes, corrections are integrated as patterns rather than isolated fixes.

## What changes when this is active

- Kilo recognizes modality shifts without being told and adjusts what it produces (no files during conversation, no diagnostics on throwaway code, no prose before an outline is approved).
- Decisions and approved text are tracked via journal tags (`decided`, `approved`, `superseded`, `open-question`) and indexed in a compact project memory block that persists across sessions.
- Code ceremony scales with durability: a `python -c` one-liner gets zero ceremony, a git-committed repo gets a README and environment spec, production code gets full standards.
- Handoffs are generated from the journal, not manually authored — they export what was decided, what was approved, and what remains open.

## Setup

### 1. Clone this repo

```sh
git clone <this-repo-url> </path/to/working-style>
```

### 2. Clone kilocode-agent-memory

This is a forked version of the agent memory plugin with a worktree fix.

```sh
git clone <kilocode-agent-memory-url> </path/to/kilocode-agent-memory>
```

### 3. Configure Kilo

Add the plugin and instructions entries to `~/.config/kilo/kilo.jsonc`:

```jsonc
{
  "plugin": [
    ["file:///</path/to/kilocode-agent-memory>", {
      "journal": {
        "enabled": true,
        "tags": [
          { "name": "decided", "description": "Confirmed decision or conclusion" },
          { "name": "approved", "description": "Finalized text — include reference to source location" },
          { "name": "superseded", "description": "Prior decision replaced by a later one" },
          { "name": "open-question", "description": "Explicitly identified as unresolved" },
          { "name": "approved-outline", "description": "Approved document structure governing subsequent writing" }
        ]
      }
    }]
  ],
  "instructions": [
    "</path/to/working-style>/instructions.md"
  ]
}
```

All paths must be absolute. The `instructions` array tells Kilo to load `instructions.md` as global instructions injected into every session. The plugin entry loads `kilocode-agent-memory` with journal tags that the instructions reference for shared understanding.
