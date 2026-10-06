# opencode-profile-isolation

A small wrapper script for launching [OpenCode](https://opencode.ai/) in isolated, named profile environments.

## Why?

OpenCode already has project config, which is great when a setup belongs to one repo. But sometimes the setup belongs to a *type* of work instead.

I wanted a heavy profile for serious/big projects, with things like `oh-my-opencode`, extra MCP servers, custom config, and whatever else. But dragging that into every small hobby repo felt like overkill, and editing the same config back and forth did not sound fun.

It is also handy for plugin development: spin up a clean profile, test things in isolation, break stuff freely, then throw the profile away if needed.

So this script gives me simple named OpenCode profiles:

```bash
op work .
op hobby .
op throwaway .
```

Each profile gets its own config, cache, state, data, and optional `.env`, while still letting the actual project stay clean.

## What it does

`op` creates and runs OpenCode with a separate profile directory under:

```text
~/opencode-profiles/<profile-name>
```

Each profile gets its own `HOME` and XDG directories, so OpenCode config, cache, state, and data are isolated from other profiles and from your normal user environment.

## Features

- Prompts before creating a new profile directory.
- Keeps OpenCode-installed plugins, skills, and other customizations inside new profiles.
- Loads `~/opencode-profiles/<profile-name>/.env` into the OpenCode process if present.
- Keeps `.env` variables scoped to the launched process.
- Disables OpenCode external skills with `OPENCODE_DISABLE_EXTERNAL_SKILLS=1`.
- Persists OpenCode service port per profile and protects concurrent launches with a lock.
- Passes all arguments after the profile name through to `opencode`.

## Installing OpenCode customizations inside a profile

When `op` creates a profile, it also creates:

```text
~/opencode-profiles/<profile-name>/.config/opencode/AGENTS.md
```

These starter instructions apply when you ask OpenCode running inside an
isolated profile to install or configure something for itself — for example, a
plugin, skill, agent, command, MCP server, provider, model, theme, or keybinding.
They tell the agent to put that profile-global customization inside the active
isolated profile rather than the host's normal OpenCode directories.

The file is only created with a new profile. After that it is entirely yours:
`op` never rewrites or restores it, so you can edit or remove the instructions.
Existing profiles are not changed; copy the starter guidance from the `op`
script into their `.config/opencode/AGENTS.md` if you want the same behavior.

## Usage

```bash
./op <profile-name> [opencode args...]
```

Example:

```bash
./op work .
```

## Install

Copy the script somewhere on your `PATH`, for example:

```bash
install -m 0755 op ~/bin/op
```

## Requirements

- Bash
- `flock`
- Python 3
- OpenCode CLI available as `opencode`
