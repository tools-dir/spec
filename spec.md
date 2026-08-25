# `.tools/` Directory Specification

**Version:** 0.1-draft  
**Status:** Draft

## 1. Introduction

The `.tools/` directory provides a standard location for repository-local files and directories owned by development tools.

Rather than each development tool claiming its own file or directory in the root of a source repository, participating tools share a common `.tools/` namespace.

For example:

```text
.tools/
├── claude/
├── eslint/
├── github/
├── jetbrains/
└── vscode/
```

This specification defines the structure and naming conventions of the `.tools/` directory.

It intentionally does not define the internal structure or configuration format used by individual tools.

## 2. Conformance Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **NOT RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in [BCP 14](https://www.rfc-editor.org/info/bcp14) when, and only when, they appear in all capitals.

## 3. The `.tools/` Directory

A repository MAY contain a directory named:

```text
.tools/
```

at its repository root.

The `.tools/` directory is a shared namespace for repository-local resources owned by development tools.

Development tools supporting this specification SHOULD automatically discover their resources beneath `.tools/` without requiring users to provide an explicit path.

## 4. Tool Directories

Each participating tool MUST store its resources within a dedicated directory immediately beneath `.tools/`.

The general form is:

```text
.tools/{tool-name}/
```

For example:

```text
.tools/
├── claude/
├── eslint/
└── vscode/
```

A tool MUST NOT place tool-specific files directly within `.tools/`.

A tool SHOULD NOT place resources within another tool's directory.

A tool MAY define any internal structure beneath its own directory.

For example:

```text
.tools/
└── claude/
    ├── settings.json
    ├── agents/
    └── skills/
```

The structure and semantics of files beneath `.tools/{tool-name}/` are outside the scope of this specification.

## 5. Tool Directory Names

Tool directory names MUST:

- use lowercase characters;
- NOT begin with a period (`.`);
- NOT contain path separators; and
- be valid directory names on commonly supported development platforms.

Tool directory names SHOULD:

- use the commonly recognized name of the tool;
- be concise and recognizable;
- avoid unnecessary vendor or organization prefixes; and
- consist only of lowercase ASCII letters (`a-z`), digits (`0-9`), and hyphens (`-`).

Examples of conforming names include:

```text
.tools/claude/
.tools/codex/
.tools/github/
.tools/jetbrains/
.tools/visual-studio/
.tools/vscode/
```

Examples of non-conforming names include:

```text
.tools/.claude/
.tools/Claude/
.tools/Visual Studio/
```

This specification does not currently define a registry of canonical tool names.

A future version MAY define such a registry or another mechanism for resolving naming conflicts.

## 6. Files Directly Within `.tools/`

Files MUST NOT be placed directly within `.tools/` unless explicitly permitted by this specification.

This reserves the root of `.tools/` for specification-defined resources and allows future versions of the specification to introduce repository-wide tooling metadata without conflicting with individual tools.

### 6.1 `README.md`

A repository MAY contain:

```text
.tools/README.md
```

This file is intended for human-readable documentation concerning the repository's development tooling, its `.tools/` directory, or repository-specific tooling conventions.

Individual development tools MUST NOT assume ownership of `.tools/README.md`.

No other files are currently defined for placement directly within `.tools/`.

For example:

```text
.tools/
├── README.md
├── claude/
├── eslint/
└── vscode/
```

is conforming, while:

```text
.tools/
├── eslint.json
├── prettier.yaml
└── vscode/
```

is not.

Future versions of this specification MAY define additional files directly beneath `.tools/`.

## 7. Repository Root

The `.tools/` directory defined by this specification resides at the root of the source repository.

For example:

```text
repository/
├── .git/
├── .tools/
├── src/
├── tests/
└── README.md
```

The behavior of `.tools/` directories within nested projects, submodules, worktrees, or other repository structures is not currently defined by this specification.

Support for nested or hierarchical `.tools/` directories MAY be considered in a future version.

## 8. Tool Discovery

A development tool implementing this specification SHOULD automatically look for its corresponding directory at:

```text
.tools/{tool-name}/
```

when operating within a repository.

Users SHOULD NOT be required to:

- create symbolic links to legacy locations;
- use wrapper scripts;
- specify command-line configuration paths; or
- otherwise introduce indirection solely to enable `.tools/` support.

Tools MAY continue to support their existing or legacy repository locations.

If a tool supports both a legacy location and `.tools/{tool-name}/`, the tool MUST document how conflicts and precedence are resolved when both locations exist.

This specification does not currently prescribe precedence between legacy and `.tools/` locations.

## 9. Migration

Adoption of `.tools/` SHOULD preserve existing tool behavior.

Tools adding support for this specification SHOULD provide a straightforward migration path from their existing repository-level files or directories when practical.

For example, a tool currently using:

```text
.claude/
```

might additionally support:

```text
.tools/claude/
```

A tool MAY provide diagnostics, migration commands, or other assistance to help users relocate existing resources.

Migration SHOULD NOT require symbolic links, wrapper scripts, or other indirection solely to preserve existing behavior.

Tools MUST NOT recommend relocating a resource when doing so would change the resource's intended semantics.

## 10. Scope

The `.tools/` directory is intended for resources that are:

1. repository-local;
2. owned primarily by a development tool; and
3. not inherently required to exist elsewhere because of established project semantics.

Examples may include:

- tool configuration;
- IDE settings;
- agent definitions;
- agent skills;
- instructions;
- hooks;
- launch profiles;
- development tasks;
- workflows;
- templates; and
- other tool-specific repository metadata.

Whether a particular resource belongs beneath `.tools/` ultimately depends on its semantics, not merely on whether a development tool reads it.

## 11. Non-Goals

This specification does not attempt to relocate every configuration, metadata, or hidden file in a repository.

In particular, it does not attempt to replace files or directories whose location is fundamental to their semantics or to established repository behavior.

Examples include:

```text
.git/
.gitignore
.gitattributes
Directory.Build.props
Directory.Build.targets
Directory.Packages.props
global.json
package.json
pyproject.toml
```

Some of these resources are consumed by development tools, but they describe the repository, project, dependency graph, or build system rather than merely configuring a particular tool.

This specification also does not currently attempt to standardize:

- application runtime configuration;
- dependency storage;
- build output;
- temporary files;
- caches;
- user-level tool configuration;
- the internal structure of tool directories; or
- tool configuration file formats.

## 12. Examples

### 12.1 Conforming Repository

```text
repository/
├── .git/
├── .tools/
│   ├── README.md
│   ├── claude/
│   │   ├── settings.json
│   │   ├── agents/
│   │   └── skills/
│   ├── eslint/
│   │   └── config.js
│   └── vscode/
│       ├── settings.json
│       ├── launch.json
│       └── tasks.json
├── src/
├── tests/
├── Directory.Build.props
└── README.md
```

### 12.2 Non-Conforming `.tools/` Contents

```text
.tools/
├── .claude/
├── VSCode/
├── eslint.json
└── prettier.yaml
```

Problems include:

- `.claude/` begins with a period;
- `VSCode/` contains uppercase characters; and
- `eslint.json` and `prettier.yaml` are tool-specific files placed directly within `.tools/`.

A conforming equivalent might be:

```text
.tools/
├── claude/
├── eslint/
│   └── config.json
├── prettier/
│   └── config.yaml
└── vscode/
```

## 13. Compatibility

Support for `.tools/` is expected to be incremental.

A tool does not need to abandon its existing repository conventions in order to support this specification.

During adoption, tools MAY support both their established locations and `.tools/{tool-name}/`.

The goal of this specification is to establish a common location that tools can support over time without requiring immediate ecosystem-wide migration.

## 14. Future Considerations

Possible future additions to this specification include:

- a registry of canonical tool directory names;
- machine-readable metadata describing tool directories;
- repository-wide tooling metadata such as an index;
- discovery rules for nested repositories or monorepos;
- standardized migration metadata; and
- additional specification-defined files directly beneath `.tools/`.

These features are intentionally outside the scope of version 0.1.

## 15. Status of This Specification

This specification is currently a draft.

Version `0.1-draft` is intended to establish the core convention and solicit feedback from development-tool authors and users.

The specification may change substantially before version 1.0.

Feedback, edge cases, implementation experience, and proposals are welcome through the project's GitHub repository.
