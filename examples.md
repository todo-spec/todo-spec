# todo-spec Examples

This document provides a comprehensive set of examples for the `todo-spec` format. For the full specification, please see `SPEC.md`.

## 1. Source Code Comments

### JavaScript/TypeScript
```javascript
// TODO: Refactor the authentication module to use JWT 📅 2026-03-15 ⏫ @alice #auth
// FIXME: Potential race condition in the data processing pipeline. 🆔 BUG-123
/*
 * TODO: This is a multiline description for a more complex task.
 * It should be parsed correctly.
 * 📅 2026-04-01 🔼 @bob #refactor
 */
```

### Python
```python
# TODO: Implement caching for the user profile endpoint 📅 2026-02-20 🔼 #performance
# FIXME: SQL injection vulnerability in the search query.
# p:critical @security-team id:PROJ-556

# TODO: Add support for Python 3.12 📅 2026-05-01
```

### Go
```go
// TODO: Implement graceful shutdown in the server 📅 2026-02-10 ⏫ 🆔 TASK-789
// TODO: Write unit tests for the new parser @charlie #testing
```

### Rust
```rust
// TODO: Replace .unwrap() with proper error handling 📅 2026-01-30 🔼 #tech-debt
// NOTE: This implementation is temporary and will be replaced.
```

### HTML
```html
<!-- TODO: Add ARIA labels for improved accessibility 📅 2026-02-01 #a11y -->
<!-- FIXME: The layout breaks on mobile devices. @design-team -->
```

### CSS/SCSS
```css
/* TODO: Consolidate these styles into a single reusable class 🔽 #refactor */
/* TODO: Replace hardcoded colors with theme variables 📅 2026-03-01 */
```

### Shell Script
```bash
# TODO: Add error handling for when the remote server is unavailable ⏫
```

## 2. Markdown Task Lists

```markdown
## Project Phoenix Sprint Plan

- [ ] TODO: Design the new user dashboard 📅 2026-02-01 ⏫ @design-team #ux
- [ ] TODO: Set up the CI/CD pipeline 📅 2026-02-05 🔼 @devops-team
- [ ] TODO: Implement the OAuth2 integration 📅 2026-02-15 🔼 @alice #auth
- [x] BUG: Fix the login redirect loop ✅ 2026-01-18 🆔 BUG-123
- [ ] TODO: Write API documentation 📅 2026-02-20 🔽 #docs
- [ ] TODO: Schedule a recurring weekly sync meeting 🔁 every week @team
```

## 3. Git Commit Messages

```
feat: Add user profile page

This commit introduces the basic structure for the user profile page.

TODO: The user avatar is not yet implemented. 📅 2026-02-25 #feature
TODO: Add a link to the settings page. 📅 2026-03-01 🔽 @bob
```

## 4. Plain Text / Notes

```
Meeting Notes - 2026-01-20
==========================

### Action Items

- TODO: Send the updated proposal to the client 📅 2026-01-22 ⏫ @sarah
- TODO: Schedule a follow-up meeting for next week 📅 2026-01-27
- IDEA: We could use the new charting library for the analytics dashboard.
```

## 5. Advanced Examples

### Text-Only Format
```javascript
// TODO: Implement the search functionality due:2026-04-10 p:high @dave #search
```

### Mixed Formats
```javascript
// TODO: Update dependencies and run tests 📅 2026-03-01 p:medium @charlie
```

### Recurring Tasks
```python
# TODO: Rotate the API keys 🔁 every 90 days ⏫ #security
# TODO: Run the weekly performance report rec:FREQ=WEEKLY;BYDAY=MO @analytics
```

### Escaping
```javascript
// TODO: Document the usage of the \# and \@ characters in the parser.
```

### Custom Fields
```python
# TODO: Sync user data with Salesforce sfdc-id:12345 📅 2026-05-01
```
