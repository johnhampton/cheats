# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of command-line cheat sheets using the [navi](https://github.com/denisidoro/navi) format. These files serve as a personal knowledge base for quick command reference across various development and operations tools.

## Cheat Sheet Format

Each `.cheat` file follows the navi syntax:
- `% category, tags` - Header line defining categories and tags
- `# Description` - Command description
- `command` - The actual command to execute
- `$ variable: command` - Variables populated by command output
- `@ alternate-tag` - Cross-reference tags from other sections

For comprehensive documentation on the navi format, syntax, and best practices, see [docs/navi-format-guide.md](docs/navi-format-guide.md).

## Common Development Tasks

### Adding New Cheat Sheets
1. Create a new `.cheat` file named after the tool/service
2. Start each section with `% tags` 
3. Include clear descriptions with `#` before each command
4. Define variables with `$ variable: command` for dynamic values
5. Test with `navi --path .` before committing

### Updating Existing Cheat Sheets
- Maintain consistency with existing format
- Test variable generation commands work correctly
- Ensure commands are still valid and working
- Follow patterns established in similar cheat sheets

## Architecture Notes

The repository is organized as a flat collection of `.cheat` files, each focusing on a specific tool or service:
- **Database Operations**: `databases.cheat` contains connection strings for different environments
- **Kubernetes/Container**: `k8s.cheat`, `argocd.cheat`, `kubefwd.cheat`
- **Version Control**: `git.cheat`, `github.cheat`, `graphite.cheat`
- **Development Tools**: `nix.cheat`, `cabal.cheat`, `tmux.cheat`
- **Infrastructure/DevOps**: `qaloader.cheat`, `gcloud.cheat`

The cheat sheets are designed to work with the `navi` command-line tool, which allows for interactive searching and execution of commands with variable substitution.