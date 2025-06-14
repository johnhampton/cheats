# Navi Cheat Sheet Format Guide

This guide documents the syntax and best practices for creating and maintaining navi cheat sheets in this repository.

## Overview

[Navi](https://github.com/denisidoro/navi) is an interactive command-line cheatsheet tool that allows you to browse through cheatsheets and execute commands. It provides an interactive interface for selecting commands and filling in variable values.

## File Format

Navi cheat sheets use `.cheat` files with a specific syntax for organizing commands.

## Syntax Reference

### 1. Category/Tag Line (`%`)

Starts a new cheatsheet section with tags for categorization.

```
% tag1, tag2, tag3
```

**Example:**
```
% databases, postgres, production
```

**Usage:**
- Tags are used for searching and filtering
- Multiple tags are comma-separated
- Choose descriptive tags that help with discovery

### 2. Alias/Reference Line (`@`)

References tags from another section, allowing variable reuse.

```
@ tag1, tag2
```

**Example:**
```
@ postgres, pgcli, prod
```

**Usage:**
- Inherits variables from the referenced section
- Useful for creating variations of similar commands

### 3. Description Line (`#`)

Describes what the command does.

```
# Description of the command
```

**Example:**
```
# Connect to production database with read-only user
```

**Best Practices:**
- Be clear and concise
- Include important details (e.g., "read-only", "production")
- Start with a verb when possible

### 4. Command Line

The actual executable command with optional variable placeholders.

```
command --option <variable_name>
```

**Example:**
```
kubectl logs -n <namespace> <pod> -f
```

**Variable Rules:**
- Variables are enclosed in angle brackets: `<variable_name>`
- Variable names should contain only alphanumeric characters and underscores
- Variables can appear multiple times in a command

### 5. Variable Definition (`$`)

Defines how to populate variable values.

```
$ variable_name: command to generate values
```

**Example:**
```
$ namespace: kubectl get ns --no-headers -o custom-columns=:metadata.name
```

**Features:**
- The command after `:` generates a list of selectable values
- Variables can reference other variables
- Commands should output one value per line
- Can use static lists: `$ environment: production staging development`

### 6. Comment Line (`;`)

For metadata or comments (not displayed to users).

```
; This is a comment
```

## Complete Example

```cheat
% kubernetes, k8s, logs

# Stream logs from a pod
kubectl logs -n <namespace> <pod> -f

# Get logs from all pods in a deployment
kubectl logs -n <namespace> -l app=<app_label> --tail=<lines>

$ namespace: kubectl get ns --no-headers -o custom-columns=:metadata.name
$ pod: kubectl get pods -n <namespace> --no-headers -o custom-columns=:metadata.name
$ app_label: kubectl get deployment -n <namespace> -o jsonpath='{.items[*].metadata.labels.app}' | tr ' ' '\n' | sort -u
$ lines: echo -e "50\n100\n200\n500\n1000"
```

## Organization Best Practices

### File Structure
- One `.cheat` file per tool or service
- Name files after the primary tool: `git.cheat`, `docker.cheat`
- Group related commands within the same section

### Tag Strategy
1. **Primary tag**: The main tool name (e.g., `git`, `kubernetes`)
2. **Category tags**: The type of operation (e.g., `database`, `deployment`)
3. **Environment tags**: If applicable (e.g., `production`, `staging`)

### Variable Naming
- Use descriptive names: `<database_name>` not `<db>`
- Be consistent across files
- Use underscores for multi-word variables: `<cluster_name>`

### Command Selection
- Include commonly used commands
- Focus on commands with complex syntax
- Include variations for different use cases
- Add commands that benefit from variable substitution

## Advanced Features

### Variable Dependencies

Variables can reference other variables in their generation commands:

```
$ deployment: kubectl get deployment -n <namespace> --no-headers -o custom-columns=:metadata.name
$ pod: kubectl get pods -n <namespace> -l app=<deployment> --no-headers -o custom-columns=:metadata.name
```

### Multi-line Commands

Use backslashes for readability:

```
# Create a complex resource
kubectl create deployment <name> \
  --image=<image> \
  --replicas=<replicas> \
  -n <namespace>
```

### Static Variable Options

For fixed choices, list them directly:

```
$ log_level: debug info warning error critical
$ environment: dev staging prod
```

### Command Chaining

Combine multiple commands with proper shell operators:

```
# Build and push Docker image
docker build -t <repository>/<image>:<tag> . && \
docker push <repository>/<image>:<tag>
```

## Testing Your Cheat Sheets

1. **Syntax Validation**: Ensure proper formatting
   - Each section starts with `%`
   - Variables are properly enclosed in `<>`
   - Variable definitions use correct `$` syntax

2. **Variable Generation**: Test that variable commands produce expected output
   ```bash
   # Test a variable generation command directly
   kubectl get ns --no-headers -o custom-columns=:metadata.name
   ```

3. **Interactive Testing**: Use navi to test the full experience
   ```bash
   navi --path /path/to/your/cheats
   ```

## Common Patterns in This Repository

### Database Connections
```
% database, postgres, environment

# Connect to database
psql -h <host> -U <user> -d <database>

$ database: psql -h <host> -U <user> -d postgres -t -c "SELECT datname FROM pg_database WHERE datistemplate = false;" -A
```

### Kubernetes Operations
```
% kubernetes, troubleshooting

# Get pod logs with timestamps
kubectl logs -n <namespace> <pod> --timestamps --tail=<lines>

$ namespace: kubectl get ns --no-headers -o custom-columns=:metadata.name
$ pod: kubectl get pods -n <namespace> --no-headers -o custom-columns=:metadata.name
$ lines: echo -e "50\n100\n200\n500"
```

### Git Workflows
```
% git, branch

# Create and switch to new branch
git checkout -b <branch_name>

# Delete local branch
git branch -d <branch_name>

$ branch_name: git branch --format='%(refname:short)' | grep -v '^master$' | grep -v '^main$'
```

## Troubleshooting

### Variable Not Expanding
- Check that variable name is enclosed in `<>`
- Ensure variable is defined with `$` in the same section or referenced section

### Command Not Found
- Verify the command is available in your shell
- Check for typos in the command

### Empty Variable Lists
- Test the variable generation command separately
- Ensure the command outputs to stdout
- Check for proper permissions to run the command

## Contributing Guidelines

When adding new cheat sheets:

1. Follow the existing format and style
2. Test commands before committing
3. Use meaningful variable names
4. Include helpful descriptions
5. Group related commands together
6. Add appropriate tags for discoverability