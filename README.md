# corporate-rust

`corporate-rust` is a standalone, strict [skills.sh](https://skills.sh/) skill for Rust development and code review. It guides an agent through API design, documentation, safety, performance, and verification without requiring a separate skill.

## Install

From the project where you want to use the skill:

```sh
npx skills add bxn-dev/corporate-rust --skill corporate-rust
```

To check discovery without installing:

```sh
npx skills add bxn-dev/corporate-rust --list
```

The skill entry point is [`skills/corporate-rust/SKILL.md`](skills/corporate-rust/SKILL.md); its topic references are under `skills/corporate-rust/references/`.

## How it works

Before writing or reviewing Rust, the agent checks the callers and build configuration, reads the mandatory core requirements, and then opens only the topic sheets relevant to the task. It combines sheets when a decision crosses topics.

Where requirements overlap, binding project requirements take precedence, followed by corporate rules, language and API rules, and engineering practices. API design rules are binding for libraries and strongly recommended for applications.

For example, ask your agent:

> Review the changed function signatures using `corporate-rust`. Explain which rule applies to each change you recommend.
