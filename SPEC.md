# todo-spec: A Universal TODO Annotation Format

## 1. Introduction

`todo-spec` provides a universal, human-readable, and machine-parseable format for annotating TODOs in any text-based environment. This specification standardizes the syntax for capturing metadata such as due dates, assignees, priority, and recurrence within a TODO comment or task item.

### 1.1. Design Principles

- **Human-First Readability**: The format is intuitive and easy to understand without special tools.
- **Machine-Parseable**: A consistent structure allows for robust tooling and automation.
- **Universal Compatibility**: It works in source code, Markdown, plain text, and more.
- **Graceful Degradation**: Annotations remain readable even in non-supporting environments.
- **Optional Everything**: All metadata is optional, allowing for simple or complex TODOs.
- **Standards-Aligned**: It aligns with ISO 8601 for dates and iCalendar for recurrence concepts.

## 2. Specification

### 2.1. Basic Structure

A `todo-spec` annotation consists of a keyword, a description, and optional metadata fields.

```
<KEYWORD>: <description> [metadata...]
```

- **KEYWORD**: A case-insensitive term indicating the nature of the annotation. The recommended keywords are `TODO`, `FIXME`, `BUG`, `HACK`, `NOTE`, `INFO`, `IDEA`, `REFACTOR`, `REMINDER`. Parsers MUST recognize `TODO` and `FIXME`.
- **description**: A free-form text string describing the task. It begins after the keyword and a space, and ends at the start of the first valid metadata tag or the end of the line/comment block.

### 2.1.1. Markdown Task Lists

When a `todo-spec` annotation appears in a Markdown task list (`- [ ]` or `- [x]`), the format is modified.

```markdown
- [ ] <description> [metadata...]
```

In this context, a `KEYWORD` is not parsed as a special token. Any text that appears in the position of a keyword (e.g., `TODO:`) is considered part of the `description`. The `- [ ]` or `- [x]` prefix is sufficient to identify the line as a task.

### 2.2. Metadata

Metadata fields provide structured information about the TODO. They can appear in any order after the description.

#### 2.2.1. Formats: Emoji and Text

Both emoji and text-based formats are equally valid. A compliant parser must be able to interpret both.

- **Emoji Format**: `📅 2026-02-15`
- **Text Format**: `due:2026-02-15`

#### 2.2.2. Standard Fields

| Field | Emoji | Text Alternative | Example | Description |
|---|---|---|---|---|
| **Due Date** | 📅 | `due:` | `📅 2026-02-15` | The date the task is expected to be completed. |
| **Scheduled Date** | ⏳ | `scheduled:` | `⏳ 2026-02-01` | The date the task is scheduled to be worked on. |
| **Start Date** | 🛫 | `start:` | `🛫 2026-01-15` | The date work on the task is planned to begin. |
| **Priority** | 🔺⏫🔼🔽⏬ | `priority:` or `p:` | `🔺` or `p:highest` | The urgency or importance of the task. |
| **Recurrence** | 🔁 | `repeat:` or `rec:` | `🔁 every week` | A rule for repeating the task. |
| **Identifier** | 🆔 | `id:` | `🆔 TODO-1234` | A unique identifier for linking to external systems. |
| **Assignee** | 👤 | `@` | `👤martin` or `@martin` | The person or team responsible for the task. |
| **Tags/Projects** | — | `#` or `+` | `#backend` `+ProjectX` | Keywords or project names for categorization. |
| **Status** | ✅🚧❌ | `status:` | `status:in-progress` | The current state of the task. |
| **Created Date** | ➕ | `created:` | `➕ 2026-01-01` | The date the task was created. |
| **Completed Date** | ✅ | `done:` | `✅ 2026-01-20` | The date the task was completed. |
| **Estimate** | ⏱️ | `estimate:` | `⏱️ 2h` | The estimated time or effort required. |

### 2.3. Field-Specific Details

#### 2.3.1. Dates and Times
Dates MUST be in `YYYY-MM-DD` format (ISO 8601). Optional time and timezone information can be included, conforming to ISO 8601 (e.g., `2026-02-15T18:30:00Z`). Natural language dates (e.g., `tomorrow`) are not part of this specification, but tools may implement them as an extension.

#### 2.3.2. Priority Levels

| Level | Emoji | Text Values | Numeric |
|---|---|---|---|
| Highest | 🔺 | `highest`, `critical` | `1` |
| High | ⏫ | `high` | `2` |
| Medium | 🔼 | `medium`, `normal` | `3` |
| Low | 🔽 | `low` | `4` |
| Lowest | ⏬ | `lowest` | `5` |

#### 2.3.3. Recurrence Patterns
The specification recommends support for simple, human-readable patterns (e.g., `every day`, `every 2 weeks`). For complex rules, a subset of the iCalendar RRULE format (RFC 5545) may be used (e.g., `rec:FREQ=WEEKLY;BYDAY=MO,FR`).

#### 2.3.4. Status Values

| Status | Emoji | Meaning |
|---|---|---|
| `todo` | ⬜ | Not started (default) |
| `in-progress` | 🚧 | Currently being worked on |
| `done` | ✅ | Completed |
| `cancelled` | ❌ | Will not be done |
| `blocked` | 🚫 | Waiting on something |

### 2.4. Extensibility

Custom fields are permitted for extensibility. They should use a `key:value` format. To avoid conflicts with standard fields, it is recommended to use a unique prefix, though not required.

### 2.5. Multiline Descriptions

The description is the text between the keyword and the first metadata tag. If a description spans multiple lines, it is up to the host format's comment syntax to handle it. A parser should concatenate the content of these lines.

```python
# TODO: This is a long description
# that continues on the next line.
# 📅 2026-03-01
```

### 2.6. Escaping

To include a character in the description that might otherwise be interpreted as the start of a metadata tag, it MUST be escaped with a backslash (`\`).

`// TODO: This is about the \#backend team, not a tag.`

### 2.7. Scope

- **Nested/Subtasks**: Defining a syntax for task hierarchies is out of scope for this version.
- **Localization**: All keywords and metadata keys MUST be in English.
- **Comment Extraction**: The process of extracting comment text from different file types is the responsibility of the parsing tool and is not defined by this specification.

## 3. Conformance

A conforming parser MUST:
1.  Correctly parse the two primary structures: the generic `keyword: description` format and the keyword-less Markdown task list format.
2.  Recognize both emoji and text formats for all standard metadata fields.
3.  Strictly interpret date formats as per ISO 8601.
4.  Correctly handle backslash-escaped characters in descriptions.

---
*Document Version: 0.1.0-draft*
*Last Updated: 2025-12-28*