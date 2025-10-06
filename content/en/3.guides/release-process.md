# Release Process

### Single Source of Truth: Git Tags

Version information flows from Git tags → Build flags → Runtime code.


### Creating a Release

1. **Ensure clean state**
   ```bash
   git status
   make test
   ```

2. **Create and push tag**
   ```bash
   # Create tag
   make tag VERSION=v1.0.0
   
   # Or manually
   git tag -a v1.0.0 -m "Release v1.0.0"
   
   # Push tag (triggers release)
   git push origin v1.0.0
   ```

3. **GitHub Actions automatically**:
    - Runs tests
    - Builds for all platforms
    - Creates GitHub release
    - Attaches binaries with version in filename

### Local Development

When building locally without a tag:
```bash
make build          # Builds with version="dev"
make version        # Shows current version info
./dist/twigbush --version  # Shows runtime version
```

### Testing Version Injection
Build with explicit version
```bash
VERSION=v1.2.3-test make build
```


### Check binary version
```bash
./dist/twigbush --version
```


### Check API version
```bash
./dist/twigbush run &
curl http://localhost:8085/version
```

### Version Formats
- **Release**: `v1.0.0`
- **Pre-release**: `v1.0.0-alpha.1`, `v1.0.0-beta.1`, `v1.0.0-rc.1`
- **Development**: `dev` (no tag), or `v1.0.0-5-g1234abc` (commits after tag)

### Semantic Versioning
Follow [semver.org](https://semver.org):
- **MAJOR** (v2.0.0): Breaking API changes
- **MINOR** (v1.1.0): New features, backward compatible
- **PATCH** (v1.0.1): Bug fixes, backward compatible

## Further Reading

# Versioning System

## Overview

This project uses **Git tags as the single source of truth** for versioning. Version information is automatically injected at build time and available in both the CLI and API.

## How It Works

```
Git Tag (v1.2.3) → Makefile → Build Flags → Runtime Code
```

0. **Ensure clean state**: `./git status && make test`
1. **Create a Git tag**: `git tag -a v1.2.3 -m "Release v1.2.3"`
1. **Create a Git tag**: `git tag -a v1.2.3 -m "Release v1.2.3"`
2. **Build**: `make build` automatically detects the tag
3. **Compiler injects** version into the binary via `-ldflags`
4. **Runtime**: CLI and API read from the embedded version

## Usage

### CLI Version
```shell script
$ twigbush --version
twigbush v1.2.3 (commit: abc123, built: 2025-10-06, go: go1.24)
```


### API Version
```shell script
$ curl http://localhost:8088/version
{
  "version": "v1.2.3",
  "gitCommit": "abc123",
  "buildDate": "2025-10-06T12:00:00Z",
  "goVersion": "go1.24"
}
```


All API responses include: `X-API-Version: v1.2.3` header.

## Creating a Release

```shell script
# 1. Ensure clean state
git status
make test

# 2. Create and push tag
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```


GitHub Actions automatically builds and creates a release.

## Versioning Rules

Follow [Semantic Versioning](https://semver.org):
- **MAJOR** (`v2.0.0`): Breaking changes
- **MINOR** (`v1.1.0`): New features, backward compatible
- **PATCH** (`v1.0.1`): Bug fixes, backward compatible

Pre-releases: `v1.0.0-alpha.1`, `v1.0.0-beta.1`, `v1.0.0-rc.1`

## Development Builds

Without a tag, builds use version `dev`:
```shell script
$ make build        # VERSION=dev
$ make version      # Shows current version info
```


## Technical Details

Version information lives in `internal/version/version.go` with variables populated via `-ldflags` at compile time:

```textmate
var Version = "dev"    // Set to git tag during build
var GitCommit = "none" // Set to commit hash
var BuildDate = "unknown"
var GoVersion = "unknown"
```


The Makefile detects versions using `git describe --tags` and injects them during compilation—**no manual version file updates required**.
