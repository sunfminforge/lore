# Writing help with agent-help

Help is the only manual (skills/agent-cli.md). It is written with
`agent-help`; its crate docs say how.

## Where it lives

- Help is Markdown, written as doc comments on the type that is each fact's one home: a subcommand's clap struct or variant, a refusal's enum variant, a topic's type. It is assembled at build time; nothing is copied by hand. {#help-is-markdown-written-as-doc}
- The types are the registries: clap's command tree and an enum's variants. A variant without help does not compile; a command without text fails `check`. {#the-types-are-the-registries}
- The answer is the Markdown as written: no rendering, no wrapping, no width. Each paragraph on one line. {#the-answer-is-the-markdown-as}

## What it answers

- `--help` (and `-h`) answers the index: the tool's own text, each command's line under its group, each topic's line. `<command> --help` answers that command whole, `help <topic>` that topic, `help all` the whole manual. {#help-and-h-answers-the-index-the}
- A command's line in the index is the first line of its text; a topic's, its first sentence, so a topic opens with one. The tool's text sends the reader to `<command> --help` before a command's first run. {#a-command-s-line-in-the-index-is}
- The manual says what the tool is, what it needs, what to run first, the commands grouped by who runs them, the rules to know first, and every departure from the output contract. {#the-manual-says-what-the-tool-is}
- A command's help says when to run it, what it reads and writes, what it answers (every key, with an example object), what it may refuse, what trips you up. Who runs it is its group, declared where it is defined. {#a-command-s-help-says-when-to}
- A command's or topic's help is a page of its own: sections are `##` headings, the title left to the manual. {#a-command-s-or-topic-s-help-is-a}
- What belongs to no command (signing in, the errors' shape, a shared concept) is a topic. {#what-belongs-to-no-command}

## What it never does

- A number, an exit code or a code list is written once, in code, and placed in help by a placeholder; a message names a topic by its reference, not its name written out. {#a-number-an-exit-code-or-a-code}
- Help names nothing its reader cannot open: no decision-record numbers, no repository paths, no internal documents. {#help-names-nothing-its-reader}

## Shipping it

`agent_help::check` runs in a test. Every tool ships
`skills/<tool>/SKILL.md`: a description of when to use it and a few lines
sending the agent to `--help`, nothing more. When a command changes, its
help changes in the same commit. Test it as a stranger: from `--help` and
the command's `--help` alone, could an agent that never saw the tool run
it?
