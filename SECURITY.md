# Security

## Reporting a vulnerability

Do not open a public issue for a security problem.

Use GitHub's private reporting on this repository — **Security → Report a
vulnerability**. A report is acknowledged within 5 days. tiller is a one-person
project, so there is no bounty; credit in the release notes is offered unless
you would rather not be named.

Helpful in a report: what an attacker gains, the steps to reach it, the tiller
version and platform, and which engine the thread was on.

## What is in scope

- The tiller application and its installers, as published from this repository
- The local control endpoint and the MCP servers that ship with the app
- Anything that would let a conversation, an API key or a login token leave the
  machine other than to the engine the user chose

## What is not

- The engines themselves. A vulnerability in Claude Code goes to
  [Anthropic](https://www.anthropic.com/responsible-disclosure-policy), one in
  Codex goes to OpenAI.
- The model doing something unwanted when a permission mode that asks nothing
  was turned on. "Bypass permissions" and full-access modes are documented to
  run everything without asking; that is the feature, not a vulnerability.
- The computer-use tools driving the desktop of the machine that runs them,
  when they were registered deliberately.

## How credentials are handled

tiller has no account of its own and no login screen. It authenticates with
`ANTHROPIC_API_KEY` from the environment, or with the Claude Code or Codex
login already set up on the machine, which the engine reads from its own
configuration — never tiller. tiller stores no token of its own, and the files
holding engine credentials are outside everything tiller copies.
