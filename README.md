# opencode-profile-isolation

A small wrapper script for launching [OpenCode](https://opencode.ai/) in isolated, named profile environments.

## What it does

`op` creates and runs OpenCode with a separate profile directory under:

```text
~/opencode-profiles/<profile-name>
```

Each profile gets its own `HOME` and XDG directories, so OpenCode config, cache, state, and data are isolated from other profiles and from your normal user environment.

## Features

- Prompts before creating a new profile directory.
- Loads `~/opencode-profiles/<profile-name>/.env` into the OpenCode process if present.
- Keeps `.env` variables scoped to the launched process.
- Disables OpenCode external skills with `OPENCODE_DISABLE_EXTERNAL_SKILLS=1`.
- Persists OpenCode service port per profile and protects concurrent launches with a lock.
- Passes all arguments after the profile name through to `opencode`.

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
