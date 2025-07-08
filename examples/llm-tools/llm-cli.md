# LLM-Optimized CLI Design

A command-line interface designed for LLM pattern matching rather than human typing efficiency.

## Core Principle

Human CLIs optimize for:
- Minimum keystrokes (`ls`, `rm`, `cp`)
- Positional arguments
- Flags and options (`-rf`, `--verbose`)
- Stateful sessions (cd changes directory)

LLM CLIs optimize for:
- Semantic clarity
- Self-contained commands
- Multiple phrasings
- Explicit context

## Design Sketch

### Traditional Human CLI
```bash
# Terse, stateful, positional
$ cd /var/log
$ tail -f nginx/access.log | grep 404 | awk '{print $7}'
$ rm -rf temp/*
$ ps aux | grep python | kill -9 $(awk '{print $2}')
```

### LLM-Optimized CLI
```bash
# Semantic, stateless, explicit
$ show-logs from="/var/log/nginx/access.log" filter="status:404" fields="url" follow=true
$ show logs from nginx where status equals 404 showing url following updates
$ display nginx access log filtering for 404 errors showing only urls with live updates

# Multiple equivalent commands (all map to same operation)
$ delete-files in="/tmp/temp" pattern="*" recursive=true force=true
$ remove all files from /tmp/temp recursively without confirmation
$ clean directory /tmp/temp removing everything including subdirectories

# Emoji domain markers for clarity
$ 🗑️ remove files from="/tmp/old" older-than="30 days"
$ 📊 analyze logs from="/var/log/app.log" group-by="error-type" 
$ 🔍 search in="/home/user/documents" for="TODO" context=3
$ ⚡ optimize database="production" table="users" column="email"
```

## Key Differences

### 1. Semantic Aliases
```bash
# All equivalent (map to same underlying command)
terminate process with name "python"
kill process named python
stop process where name equals python
end process matching name python
🛑 process name=python
```

### 2. Explicit Context
```bash
# Human: relies on working directory
$ ls *.txt

# LLM: always explicit
$ list files in="/current/directory" matching="*.txt"
```

### 3. Natural Language Options
```bash
# Human: cryptic flags
$ find . -mtime -7 -type f -size +1M

# LLM: readable parameters
$ find files in="." modified-within="7 days" type="file" larger-than="1MB"
$ search for files modified in last week over 1 megabyte
```

### 4. Stateless Design
```bash
# Human: stateful
$ cd /app
$ ls
$ cd logs
$ tail error.log

# LLM: each command complete
$ list files in="/app"
$ show contents of="/app/logs/error.log" last=10
```

## Command Structure

### Grammar
```
[emoji-marker] <verb> [<object>] [<prepositions>] [<parameters>]

Examples:
📊 analyze logs from="/var/log" where="error" group-by="hour"
🔍 search in="." for="password" excluding=".git"
🗑️ clean cache older-than="7 days" keeping="important.*"
```

### Verb Vocabulary
- Primary: show, list, find, create, update, delete, analyze
- Aliases: display/show, remove/delete, search/find
- Domains: 🔍search, 📊analyze, 🗑️cleanup, ⚡optimize

### Parameter Patterns
- Named parameters always
- Multiple formats accepted:
  - `from="/path"` 
  - `from /path`
  - `source is /path`
- Common parameters:
  - Paths: from, to, in, at
  - Filters: where, matching, excluding
  - Time: since, until, within, older-than
  - Options: format, limit, recursive

## Implementation Notes

### Parser Design
```python
# Flexible grammar accepting multiple forms
class LLMCommandParser:
    verbs = {
        'show': ShowCommand,
        'display': ShowCommand,  # Alias
        'list': ListCommand,
        'enumerate': ListCommand,  # Alias
        # ... etc
    }
    
    def parse(self, input: str) -> Command:
        # Extract emoji domain if present
        domain = self.extract_emoji_domain(input)
        
        # Find verb (flexible matching)
        verb = self.extract_verb(input)
        
        # Extract parameters (multiple formats)
        params = self.extract_params(input)
        
        return self.verbs[verb](domain, params)
```

### Help System
```bash
$ help delete
Delete removes files, directories, or other resources.

Syntax variations:
  delete <what> from=<where> [options]
  remove <what> in=<where> [options]
  🗑️ <what> at=<where> [options]

Examples:
  delete files from="/tmp" older-than="30 days"
  remove all logs from="/var/log" matching="*.old"
  🗑️ temporary files in="/cache" exclude="*.important"

Related commands: clean, purge, empty
```

## Why This Works for LLMs

1. **Pattern Matching**: Multiple phrasings map to same command
2. **No Ambiguity**: Explicit parameters prevent misinterpretation
3. **Semantic Grouping**: Emoji domains create visual categories
4. **Contextual Completeness**: Each command is self-contained
5. **Natural Expression**: Commands read like specifications

## VIBES Analysis

**Rating**: `<🔬🎀💠>`

- **Expressive** (🔬): Many valid ways to express any command
- **Context** (🎀): Each command is completely independent
- **Error** (💠): Parser validates and normalizes at parse time

## Navigation

- [Back to LLM Tools](./README.md)
- [Semantic API](./semantic-api.md)
- [Config Language](./config-lang.md)
