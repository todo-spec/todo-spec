# todo-spec

## A Universal TODO Annotation Format Specification

TODO comments are everywhere in software development, but they're inconsistent and unstructured:

```python
# TODO: Fix this later
# TODO(john): Refactor by Friday
# TODO [HIGH]: Memory leak in parser
# FIXME: This is urgent!!!
# @todo implement caching - due 2025-02-01
```

This inconsistency means:

- **No tooling interoperability** – Each IDE, linter, and project tracker parses TODOs differently
- **Lost context** – Priority, ownership, and deadlines are expressed ad-hoc or not at all
- **No lifecycle management** – TODOs are created but rarely tracked to completion
- **Scattered information** – Task context lives in code, Jira, Linear, GitHub Issues, and Slack separately

---

## The Solution: todo.spec

A lightweight, universal format that works inline anywhere text exists:

```javascript
// TODO: Implement caching layer 📅 2026-02-15 ⏫ @simenandre #backend
```

```markdown
- [ ] Review PR for auth module 📅 2026-01-20 🔁 every friday #security
```

```python
# TODO: Refactor database connection pooling 📅 2026-03-01 ⏫ 🆔 TODO-1234
```

---

## Core Design Principles

### 1. **Human-First Readability**
The format must be instantly understandable without documentation. A developer glancing at a TODO should immediately grasp its meaning.

### 2. **Machine-Parseable**
Consistent syntax enables tooling: IDE plugins, linters, CI/CD integrations, and synchronization with external task managers.

### 3. **Universal Compatibility**
Works in any text context:
- Source code comments (any language)
- Markdown task lists
- Plain text files
- Git commit messages
- Documentation
- Configuration files

### 4. **Graceful Degradation**
A todo.spec annotation is still a valid, readable TODO even in tools that don't understand the format. The metadata enhances but doesn't obscure.

### 5. **Optional Everything**
Every metadata field is optional. A simple `// TODO: Fix bug` is valid. Metadata can be added incrementally as needed.

### 6. **Standards-Aligned**
Where possible, align with existing standards:
- **Dates**: ISO 8601 / RFC 3339 (`YYYY-MM-DD`)
- **Recurrence**: Inspired by iCalendar RRULE (RFC 5545)
- **Identifiers**: Flexible, supporting UUIDs or external references

---

## Proposed Syntax

### Basic Structure

```
TODO: <description> [metadata...]
```

Metadata fields can appear in any order after the description.

### Metadata Fields

| Field | Text Alternative | Emoji | Example |
|-------|------------------|-------|---------|
| **Due Date** | `due:` | 📅 | `due:2026-02-15` or `📅 2026-02-15` |
| **Scheduled Date** | `scheduled:` | ⏳ | `scheduled:2026-02-01` |
| **Start Date** | `start:` | 🛫 | `start:2026-01-15` |
| **Priority** | `priority:` or `p:` | 🔺⏫🔼🔽⏬ | `p:highest` or `🔺` |
| **Recurrence** | `repeat:` or `rec:` | 🔁 | `rec:weekly` or `🔁 every week` |
| **Identifier** | `id:` | 🆔 | `id:TODO-1234` or `🆔 TODO-1234` |
| **Assignee** | `@` | 👤 | `@martin` or `@team-backend` |
| **Tags/Projects** | `#` or `+` | — | `#backend` or `+ProjectX` |
| **Status** | `status:` | ✅🚧❌ | `status:in-progress` |
| **Created Date** | `created:` | ➕ | `created:2026-01-01` |
| **Completed Date** | `done:` | ✅ | `done:2026-01-20` |
| **Estimate** | `estimate:` | ⏱️ | `estimate:2h` or `⏱️ 2h` |

### Priority Levels

| Level | Emoji | Text Values |
|-------|-------|-------------|
| Highest | 🔺 | `highest`, `critical`, `1` |
| High | ⏫ | `high`, `2` |
| Medium | 🔼 | `medium`, `normal`, `3` |
| Low | 🔽 | `low`, `4` |
| Lowest | ⏬ | `lowest`, `5` |

### Recurrence Patterns

Simple patterns (human-friendly):
```
🔁 every day
🔁 every week
🔁 every month
🔁 every friday
🔁 every 2 weeks
🔁 every weekday
```

Advanced patterns (RRULE-compatible for tooling):
```
rec:FREQ=WEEKLY;BYDAY=MO,WE,FR
rec:FREQ=MONTHLY;BYMONTHDAY=15
```

### Status Values

| Status | Emoji | Meaning |
|--------|-------|---------|
| `todo` | ⬜ | Not started (default) |
| `in-progress` | 🚧 | Currently being worked on |
| `done` | ✅ | Completed |
| `cancelled` | ❌ | Will not be done |
| `blocked` | 🚫 | Waiting on something |

---

## Format Variants

### Text Format (Default)
The default, most compatible format:
```javascript
// TODO: Implement user authentication due:2026-02-15 p:high @sarah #security
```

### Emoji Format
An alternative format, optimized for readability and modern editor support:
```javascript
// TODO: Implement user authentication 📅 2026-02-15 ⏫ @sarah #security
```

### Mixed Format
Both can coexist:
```javascript
// TODO: Implement user authentication 📅 2026-02-15 p:high @sarah #security
```

---

## Usage Examples

### Source Code (Various Languages)

**JavaScript/TypeScript:**
```javascript
// TODO: Add input validation 📅 2026-02-01 ⏫ #security
// FIXME: Memory leak in event handler 📅 2026-01-20 🆔 BUG-456 @john
```

**Python:**
```python
# TODO: Optimize database queries 📅 2026-03-01 🔼 #performance
# TODO: Add retry logic for API calls 🔁 every sprint @backend-team
```

**Go:**
```go
// TODO: Implement graceful shutdown 📅 2026-02-15 ⏫ 🆔 TASK-789
```

**Rust:**
```rust
// TODO: Replace unwrap() with proper error handling 📅 2026-01-30 🔼 #tech-debt
```

**HTML/JSX:**
```html
<!-- TODO: Add aria labels for accessibility 📅 2026-02-01 #a11y -->
```

**CSS:**
```css
/* TODO: Replace with CSS variables 📅 2026-02-15 🔽 #refactor */
```

**Shell:**
```bash
# TODO: Add error handling for missing env vars 📅 2026-01-25 ⏫
```

### Markdown Task Lists

```markdown
## Sprint 23 Tasks

- [ ] Design new onboarding flow 📅 2026-02-01 ⏫ @design-team #ux
- [ ] Implement OAuth2 integration 📅 2026-02-15 🔼 @martin #auth
- [ ] Write API documentation 📅 2026-02-20 🔽 #docs
- [x] Fix login redirect bug ✅ 2026-01-18 🆔 BUG-123
```

### Plain Text / Notes

```
Meeting Notes - 2026-01-15
==========================

Action items:
- TODO: Send proposal to client 📅 2026-01-17 ⏫ @sarah
- TODO: Schedule follow-up meeting 📅 2026-01-20 🔁 every 2 weeks
- TODO: Review contract terms 📅 2026-01-22 @legal-team
```

### Git Commit Messages

```
feat: Add user profile endpoint

TODO: Add rate limiting 📅 2026-02-01 #security
TODO: Add caching layer 📅 2026-02-15 🔼 #performance
```

---

## Integration Possibilities

### IDE/Editor Plugins
- Syntax highlighting for todo.spec metadata
- Inline date pickers and priority selectors
- TODO panel with filtering and sorting
- Jump-to-definition for referenced issues

### CLI Tools
- `todospec list` – List all TODOs in a project
- `todospec lint` – Validate TODO format
- `todospec sync` – Sync with external tools (Linear, Jira, GitHub Issues)
- `todospec report` – Generate TODO reports

### CI/CD Integration
- Fail builds on overdue high-priority TODOs
- Auto-create issues from new TODOs
- Track TODO debt over time

### External Tool Sync
- **GitHub Issues**: Two-way sync between code TODOs and issues
- **Linear**: Create/update Linear issues from TODOs
- **Jira**: Link TODOs to Jira tickets via ID
- **Slack**: Notify on approaching due dates

---

## Relationship to Existing Standards

### todo.txt
The original plain-text TODO format. todo.spec extends similar concepts but:
- Works inline in any text (not just dedicated files)
- Adds emoji syntax for visual clarity
- Includes more metadata fields (recurrence, estimates, etc.)

### GitHub Flavored Markdown (GFM)
GFM defines `- [ ]` task list syntax. todo.spec:
- Builds on top of GFM task lists
- Adds structured metadata after the description

### iCalendar / RFC 5545
The VTODO component defines task properties. todo.spec:
- Draws inspiration from VTODO fields (PRIORITY, DUE, RRULE)
- Uses simplified, human-readable syntax
- Designed for inline annotation, not standalone files

### Obsidian Tasks Plugin
Popular Markdown task format using emojis. todo.spec:
- Adopts similar emoji conventions where sensible
- Extends to work in source code comments
- Provides text-only alternative syntax



---

## Implementation Roadmap

### Phase 1: Specification
- [ ] Finalize core syntax
- [ ] Write formal grammar (ABNF or PEG)
- [ ] Document all fields and valid values
- [ ] Create test suite with edge cases

### Phase 2: Reference Implementation
- [ ] Parser library (TypeScript/JavaScript)
- [ ] CLI tool for basic operations
- [ ] VSCode extension (syntax highlighting + TODO panel)

### Phase 3: Ecosystem
- [ ] GitHub Action for TODO linting
- [ ] Integration modules (Linear, Jira, GitHub Issues)
- [ ] Language-specific parsers (Python, Go, Rust)

### Phase 4: Adoption
- [ ] Documentation site
- [ ] Publish as informational RFC or community standard
- [ ] Seek adoption from tool vendors

---

## References

- [RFC 5545 - iCalendar](https://datatracker.ietf.org/doc/html/rfc5545) – VTODO component, RRULE recurrence
- [RFC 8984 - JSCalendar](https://datatracker.ietf.org/doc/html/rfc8984) – JSON calendar/task format
- [RFC 7763 - text/markdown](https://datatracker.ietf.org/doc/html/rfc7763) – Markdown media type
- [RFC 3339 - Date and Time](https://datatracker.ietf.org/doc/html/rfc3339) – Timestamp format
- [todo.txt](https://github.com/todotxt/todo.txt) – Plain text TODO format
- [Obsidian Tasks](https://publish.obsidian.md/tasks/) – Markdown task plugin
- [GFM Task Lists](https://github.github.com/gfm/#task-list-items-extension-) – GitHub task list syntax

---

*Document Version: 0.1.0-draft*
*Last Updated: 2025-12-28*
*Authors: Simen A. W. Olsen, [Colleague Name]*
