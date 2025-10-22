---
title: Developer Setup
description: Local setup instructions for TwigBush developers
navigation:
  icon: i-lucide-terminal
---
# Installation

TwigBush is currently in early development with no official releases yet. You'll need to build from source.

## Prerequisites

- [Go](https://golang.org/dl/) 1.22 or later
- Git

## Building from Source

1. Clone the repository:
   ```bash
   git clone https://github.com/your-org/twigbush.git
   cd twigbush
   ```

2. Build the CLI:
   ```bash
   mod download
   go build -o ./dist/twigbush ./cmd/twigbush
   ```

3. Add to your PATH (current session only):
   ```bash
   # On macOS/Linux
   export PATH="$PATH:$(pwd)/dist"
   
   # On Windows (PowerShell)
   $env:PATH += ";$(pwd)\dist"
   ```

## Verify Installation

Test that the installation was successful:
```bash
twigbush --help
```
You should see the TwigBush CLI help output with available commands.
```bash
TwigBush developer CLI for GNAP flows

Usage:
  twigbush [flags]
  twigbush [command]

Available Commands:
  as          Authorization Server helpers
  completion  Generate the autocompletion script for the specified shell
  grant       GNAP grant operations
  help        Show help
  init        Create ~/.twigbush/config.yaml and a default key
  keys        Key management
  run         Start local AS and RS for dev
  sign        Helpers for HTTP Message Signatures and DPoP
  token       Use or inspect tokens

Flags:
      --as-base-url string   Authorization Server base URL (default "http://localhost:8089")
      --config string        config file path (default "/Users/joshfischer/.twigbush/config.yaml")
  -h, --help                 help for twigbush
  -o, --output string        output format: json|yaml|table (default "json")
      --rs-base-url string   Resource Server base URL (default "http://localhost:8089")
      --show-curl            print equivalent curl for networked commands

Use "twigbush [command] --help" for more information about a command.
```

## Initialize Configuration
Start by creating the default configuration:

```bash
twigbush init
```

This creates `~/.twigbush/config.yaml` and generates a default key for development.

## Start Development Server

Launch the local Authorization Server (AS) and the Playground Server (RS):

```shell script
twigbush run
```


This starts both servers on default settings:
```bash
$ twigbush run
./dist/twigbush run
Starting authorization server from: /Users/<username>/TwigBush/dist/as
Starting playground from: /Users/<username>/TwigBush/dist/playground
```

## Basic Commands

### Key Management
** NOTE ** The following commands are still under development. We'd love your help!
```shell script
# List available keys
twigbush keys list

# Generate a new key
twigbush keys new
```
For complete key management documentation, see [Key Operations](/docs/guides/key-operations).

### Authorization Server Operations
** NOTE ** The following commands are still under development. We'd love your help!
```shell script
# Get AS configuration
twigbush as config

# View AS status
twigbush as status
```


### Grant Operations
** NOTE ** The following commands are still under development. We'd love your help!
```shell script
# Create a new grant
twigbush grant create

# List active grants
twigbush grant list
```

## Global Flags

TwigBush supports several global flags that work with any command:

- `--config string` - Specify config file path (default: `~/.twigbush/config.yaml`)
- `--output string` - Output format: `json|yaml|table` (default: `json`)
- `--as-base-url string` - Authorization Server base URL (default: `http://localhost:8089`)
- `--rs-base-url string` - Resource Server base URL (default: `http://localhost:8089`)
- `--show-curl` - Print equivalent curl commands for network operations

## Next Steps

- [Playground Demo](/en/guides/playground) - Try the interactive GNAP grant lifecycle

## Global Flags

Available with any command:

| Flag | Description | Default |
|------|-------------|---------|
| `--config` | Config file path | `~/.twigbush/config.yaml` |
| `--output` | Output format (json\|yaml\|table) | `json` |
| `--as-base-url` | Authorization Server base URL | `http://localhost:8089` |
| `--rs-base-url` | Resource Server base URL | `http://localhost:8089` |
| `--show-curl` | Print equivalent curl commands | `false` |
| `-h, --help` | Show help | |

## Available Commands
All commands are available via the `twigbush` CLI and are still under development.  

Our [GitHub Discussions](https://github.com/orgs/TwigBush/discussions) page is a great place to ask questions and give constructive feedback. 

Use `twigbush [command] --help` for detailed information about any command.
```
<llm-snippet-file>content/docs/3.development/1.contributing.md</llm-snippet-file>
```
markdown
# Contributing to TwigBush

We welcome contributions to TwigBush! This guide will help you get started.

## Development Setup

### Prerequisites
- Go 1.22 or later
- Git

### Local Development

1. Fork and clone the repository:
```shell script
git clone https://github.com/your-username/twigbush.git
   cd twigbush
```


2. Build the development version:
```shell script
go build -o ./dist/twigbush ./cmd/twigbush
```


3. Run tests:
```shell script
go test ./...
```


4. Add the binary to your PATH for testing:
```shell script
export PATH="$PATH:$(pwd)/dist"
```


## Project Philosophy

TwigBush is designed around the principle of **CLI-first lifecycle management**. Our goal is to provide a comprehensive command-line interface that can manage the entire lifecycle and maintenance of TwigBush servers and their resources.

Key principles:
- All server operations should be accessible via CLI
- Configuration should be code-driven when possible
- Development workflow should be streamlined and intuitive
- GNAP protocol compliance is paramount

## Making Changes

### Code Style
- Follow standard Go conventions
- Use `gofmt` to format code
- Write tests for new functionality
- Update documentation for user-facing changes

### Testing Your Changes
Always test your changes with the built CLI:

```shell script
# Rebuild after changes
go build -o ./dist/twigbush ./cmd/twigbush

# Test basic functionality
twigbush --help
twigbush init
twigbush run
```


## Submitting Changes

1. Create a feature branch: `git checkout -b feature/your-feature-name`
2. Make your changes and commit them
3. Push to your fork: `git push origin feature/your-feature-name`
4. Open a Pull Request

## Getting Help

- Check existing [Issues](https://github.com/your-org/twigbush/issues)
- Join our [Discussions](https://github.com/your-org/twigbush/discussions)
- Read the [GNAP specification](https://datatracker.ietf.org/doc/draft-ietf-gnap-core-protocol/)
```
## 2. Update Navigation

```
typescript
// ... existing code ...
export default defineNuxtConfig({
// ... existing configuration ...
docus: {
// ... existing docus config ...
aside: {
level: 2,
exclude: []
},
header: {
// ... existing header config ...
},
footer: {
// ... existing footer config ...
}
}
// ... existing code ...
})
```
## 3. Add Development Badges and Status

<llm-snippet-file>content/docs/index.md</llm-snippet-file>
```
markdown
# TwigBush Documentation

::alert{type="warning"}
**Early Development**: TwigBush is currently in early development with no official releases. All features are subject to change.
::

TwigBush is a developer CLI tool for managing GNAP (Grant Negotiation and Authorization Protocol) flows, designed to handle the entire lifecycle and maintenance of TwigBush servers and their resources.

## Key Features

- 🚀 **CLI-First Design** - Manage everything through command-line interface
- 🔐 **GNAP Protocol Support** - Full implementation of GNAP authorization flows
- 🛠️ **Development Tools** - Built-in development server for testing
- 🔑 **Key Management** - Comprehensive cryptographic key handling
- 📊 **Multiple Output Formats** - JSON, YAML, and table outputs

## Quick Links

:::card-grid
#default
:::card{icon="noto:rocket"}
#title
Getting Started
#description
Install TwigBush and start your first GNAP flow in minutes.
:::

:::card{icon="noto:books"}
#title
CLI Reference
#description
Complete documentation for all available commands and options.
:::

:::card{icon="noto:construction"}
#title
Contributing
#description
Help improve TwigBush by contributing code, documentation, or feedback.
:::

:::card{icon="noto:globe-with-meridians"}
#title
GNAP Specification
#description
Learn about the GNAP protocol that powers TwigBush.
:::
:::

## Project Status

| Component | Status |
|-----------|---------|
| CLI Core | 🟡 In Development |
| GNAP Implementation | 🟡 In Development |
| Documentation | 🟡 In Progress |
| Stable API | 🔴 Not Yet |
| Official Release | 🔴 Not Yet |

::alert{type="info"}
Want to contribute? Check out our [Contributing Guide](/docs/development/contributing) to get started.
::

markdown
# twigbush init

Create `~/.twigbush/config.yaml` and generate a default key for development.

## Usage

```shell script
twigbush init [flags]
```

## Description

The `init` command sets up your TwigBush development environment by:

1. Creating the configuration directory (`~/.twigbush/`)
2. Generating a default configuration file (`config.yaml`)
3. Creating a default cryptographic key for development

This is typically the first command you'll run after building TwigBush.

## Examples

```shell script
# Initialize with default settings
twigbush init

# Initialize with custom config location
twigbush --config /path/to/config.yaml init
```


## Configuration File

The generated `config.yaml` includes:

```yaml
# Example configuration structure
server:
  host: "localhost"
  port: 8089
  
keys:
  default: "path/to/generated/key"
  
# Additional configuration options...
```

