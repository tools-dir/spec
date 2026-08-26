# The `.tools/` Directory Specification

A proposal for a standard location for development-tool-specific files and directories within source code repositories.

[Read the current version of the specification](/spec.md)

## The Problem

Development tools increasingly add their own files and directories to the root of a repository.

A modern repository might contain:

```text
.agents/
.aspire/
.claude/
.codex/
.git/
.github/
.idea/
.vs/
.vscode/
```

As the number of development tools grows, so does the amount of tool-specific metadata occupying the repository root.

This proposal asks a simple question:

**What if development tools shared a common namespace?**

## The Proposal

Reserve a `.tools/` directory at the repository root for development-tool-specific files.

Each tool receives its own directory beneath `.tools/`:

```text
.git/
.tools/
    aspire/
    claude/
    codex/
    jetbrains/
    visualstudio/
    vscode/
src/
tests/
README.md
```

The basic convention is:

```text
.tools/{tool-name}/
```

Tools MUST use a dedicated directory beneath `.tools/` rather than placing files directly in `.tools/`.

The specification defines **where a tool may store its repository-specific resources**, not how those resources are organized internally.

For example:

```text
.tools/
    claude/
        settings.json
        agents/
        skills/
    vscode/
        settings.json
        launch.json
        tasks.json
```

## Principles

### One namespace for development tooling

Instead of every development tool claiming another name in the repository root, tools share `.tools/`.

### One directory per tool

Even when a tool currently requires only one configuration file, it receives a directory. This provides namespace isolation and allows the tool to add additional resources later without introducing new root-level artifacts.

### Tools own their directory

The specification intentionally says little about the contents of `.tools/{tool-name}/`. Those details belong to the individual tool.

### Don't break existing project semantics

This proposal is **not** an attempt to move every configuration or metadata file into `.tools/`.

Some files have semantics based on their location or are fundamental parts of a project's build system or structure.

Examples might include:

```text
Directory.Build.props
Directory.Build.targets
Directory.Packages.props
global.json
package.json
pyproject.toml
```

Moving such files may change their behavior and is outside the scope of this proposal.

### Adoption should not require indirection

The goal is for tools to natively discover their resources beneath `.tools/`.

Requiring symlinks, wrapper scripts, or other indirection merely to relocate existing files defeats much of the purpose of the convention.

## Why Not `.config/`?

There have been previous efforts to standardize project configuration beneath a `.config/` directory.

Those efforts address an important part of the problem, but modern development tools increasingly store much more than configuration.

Tools may store:

- configuration
- agent definitions
- instructions
- skills
- workflows
- launch profiles
- tasks
- hooks
- templates
- IDE metadata
- other repository-specific resources

`.tools/` organizes these resources according to **who owns them**, rather than attempting to classify what kind of resource each file represents.

The proposal is therefore:

> **Don't standardize where configuration goes. Standardize where development tools put their stuff.**

## Related Work and References

The `.tools/` proposal builds on and is informed by previous efforts to reduce
tool-specific clutter and establish predictable locations for development
tooling resources.

- [config-dir](https://github.com/pi0/config-dir) — An active proposal for
  standardizing a project-level `.config/` directory. It addresses the closely
  related problem of configuration-file proliferation and tracks adoption
  across development tools.

- [dot-config](https://dot-config.github.io/) — An earlier effort to promote
  `.config/` as a standard location for project-specific tool configuration,
  inspired by the XDG Base Directory Specification.

- [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir/latest/) —
  The established convention that consolidates user-specific configuration,
  data, state, and cache files into common base directories rather than allowing
  every application to create its own top-level location.

- [Node.js Tooling: Recommendation for tooling config file location/format](https://github.com/nodejs/tooling/issues/71) —
  A 2020 discussion of project-root configuration proliferation. Among the
  proposals considered was a `tools` directory containing a subdirectory for
  each tool. This establishes a precedent for this approach to this problem.

- [Stay Out Of My (Project) $HOME](https://jackson.dev/post/stay-out-of-my-project-home/) —
  A discussion of project-root clutter and existing standardization efforts,
  including dot-config and the broader PRJ specification.

- [AGENTS.md](https://agents.md/) — An example of multiple development-tool
  vendors collaborating on a predictable, vendor-neutral repository convention
  rather than requiring separate instructions for every tool.

### Specification Conventions

- [BCP 14](https://www.rfc-editor.org/info/bcp14/) —
  Defines the normative `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, and `MAY`
  terminology used by this specification.

## Status

This project is currently an early proposal.

The specification, naming conventions, scope, compatibility guidance, and migration strategy are open for discussion.

Existing tools are **not expected to support `.tools/` today**.

An important goal of this project will be documenting existing conventions and working with tool maintainers interested in supporting `.tools/{tool-name}/` as a standard discovery location.

## Contributing

Feedback, criticism, edge cases, and examples of existing tool conventions are welcome.

Please use GitHub Issues and Discussions to help shape the specification before it is considered stable.

## Credits

- [Org Icon](https://icon-sets.iconify.design/?query=folder-tools) (MIT licensed)
