# Coding Conventions - Proposed Revisions

**Date**: 2025-12-27
**Purpose**: Align with industry-standard linters and formatters (ESLint, Prettier, Airbnb, Google)

---

## Executive Summary

The current CODING_CONVENTIONS.md uses **snake_case** for variables, which conflicts with JavaScript industry standards. This document proposes revisions to align with:

- ✅ **ESLint** (recommended rules)
- ✅ **Prettier** (default formatting)
- ✅ **Airbnb JavaScript Style Guide**
- ✅ **Google JavaScript Style Guide**
- ✅ **StandardJS**

**Key Change**: Adopt **camelCase** for variables (standard across all major JavaScript style guides)

---

## Comparison: Current vs Industry Standards

| Element        | Current Convention | Industry Standard      | Rationale                            |
| -------------- | ------------------ | ---------------------- | ------------------------------------ |
| Variables      | `snake_case`       | `camelCase`            | All major style guides use camelCase |
| Functions      | `camelCase` ✅     | `camelCase` ✅         | Already aligned                      |
| Constants      | `snake_case`       | `SCREAMING_SNAKE_CASE` | Only for true constants              |
| Mutable config | `snake_case`       | `camelCase`            | Treat as regular variables           |
| HTML IDs       | `kebab-case` ✅    | `kebab-case` ✅        | Already aligned                      |
| CSS classes    | `kebab-case` ✅    | `kebab-case` ✅        | Already aligned                      |

---

## Detailed Revisions

### 1. Naming Conventions

#### ❌ REMOVE: snake_case for Variables

```javascript
// ❌ Current (non-standard)
const total_count = 10;
const frequency_unit = 'MHz';
let current_page = 1;
let is_loading_logs = false;
```

#### ✅ ADOPT: camelCase for Variables

```javascript
// ✅ Proposed (industry standard)
const totalCount = 10;
const frequencyUnit = 'MHz';
let currentPage = 1;
let isLoadingLogs = false;
```

**Rationale**:

- ESLint `camelcase` rule (enabled by default in most configs)
- Prettier formats to camelCase
- Airbnb, Google, StandardJS all mandate camelCase
- 95%+ of npm packages use camelCase

---

#### ✅ ADOPT: SCREAMING_SNAKE_CASE for True Constants Only

```javascript
// ✅ True constants (config values that never change)
const ITEMS_PER_LOAD = 10;
const TIMESTAMP_CACHE_MAX_SIZE = 100;
const BACK_TO_TOP_THRESHOLD = 20;
const MAX_RETRIES = 3;
const API_TIMEOUT_MS = 5000;

// ✅ Regular variables (use camelCase even if const)
const db = new Dexie('RadioMemoDatabase');
const domCache = {};
const timestampCache = new Map();
```

**Rule of Thumb**:

- **SCREAMING_SNAKE_CASE**: Primitive values that are compile-time constants
- **camelCase**: Objects, arrays, functions, or runtime values (even if declared with `const`)

---

#### ✅ KEEP: camelCase for Functions (Already Correct)

```javascript
// ✅ Already following standard
function loadLogs() {}
function formatTimestamp() {}
async function handleFormSubmit() {}

const exportLogs = async function () {};
const closePopoverOnOutsideClick = (e) => {};
```

---

#### ✅ KEEP: kebab-case for HTML/CSS (Already Correct)

```html
<!-- ✅ Already following standard -->
<div id="new-log-form"></div>
<button class="btn-primary"></button>
```

---

### 2. ESLint Configuration

**Recommended**: Add `.eslintrc.json` to enforce standards

```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": ["eslint:recommended"],
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "rules": {
        "camelcase": ["error", { "properties": "always" }],
        "no-var": "error",
        "prefer-const": "error",
        "quotes": ["error", "single"],
        "semi": ["error", "always"],
        "indent": ["error", 4],
        "no-unused-vars": ["warn"],
        "no-console": "off"
    }
}
```

**Key Rules**:

- `camelcase`: Enforce camelCase for variables and properties
- `no-var`: Disallow `var` (already following)
- `prefer-const`: Prefer `const` over `let` when possible
- `quotes`: Single quotes for strings
- `semi`: Require semicolons

---

### 3. Prettier Configuration

**Recommended**: Add `.prettierrc.json` for consistent formatting

```json
{
    "printWidth": 100,
    "tabWidth": 4,
    "useTabs": false,
    "semi": true,
    "singleQuote": true,
    "trailingComma": "es5",
    "bracketSpacing": true,
    "arrowParens": "always",
    "endOfLine": "lf"
}
```

**Key Settings**:

- `printWidth: 100`: Max line length
- `tabWidth: 4`: Consistent with current code
- `singleQuote: true`: Use single quotes
- `semi: true`: Always use semicolons
- `trailingComma: "es5"`: Trailing commas where valid

---

### 4. HTML/Template Formatting

**Recommended**: Add `.prettierrc.html.json` or use default

```json
{
    "htmlWhitespaceSensitivity": "css",
    "printWidth": 100,
    "tabWidth": 4,
    "useTabs": false,
    "bracketSameLine": false
}
```

**Best Practices**:

```html
<!-- ✅ Good: Proper indentation, semantic HTML -->
<div class="log-entry" data-log-id="${log.id}">
    <div class="log-timestamp-row">
        <span class="log-timestamp">${formatTimestamp(log.timestamp)}</span>
    </div>
    <div class="log-band-freq-row">
        <span class="log-band">${escapeHtml(log.band)}</span>
    </div>
</div>

<!-- ❌ Bad: Inconsistent indentation -->
<div class="log-entry" data-log-id="${log.id}">
    <div class="log-timestamp-row">
        <span class="log-timestamp">${formatTimestamp(log.timestamp)}</span>
    </div>
</div>
```

---

### 5. CSS Formatting with Prettier

**Recommended**: `.prettierrc.css.json`

```json
{
    "printWidth": 100,
    "tabWidth": 4,
    "useTabs": false,
    "singleQuote": false
}
```

**Best Practices**:

```css
/* ✅ Good: Consistent formatting, alphabetical properties */
.btn-primary {
    background: none;
    border: 1px solid #000;
    border-radius: 0;
    color: #000;
    cursor: pointer;
    font-size: 14px;
    padding: 10px 16px;
    transition: all 0.3s;
}

/* ✅ Good: Dark mode follows same pattern */
@media (prefers-color-scheme: dark) {
    .btn-primary {
        border: 1px solid #e0e0e0;
        color: #e0e0e0;
    }
}
```

---

### 6. Updated Naming Examples

#### Variables (camelCase)

```javascript
// State management
let loadedCount = 0;
let totalCount = 0;
let hasMoreLogs = false;
let isLoadingLogs = false;
let lastFrequencyValue = '';

// DOM cache
let domCache = {};
const timestampCache = new Map();

// Form data
const frequencyInput = document.getElementById('frequency');
const bandDisplay = document.getElementById('band');
const logsContainer = document.getElementById('logs');
```

#### Constants (SCREAMING_SNAKE_CASE only for true constants)

```javascript
// ✅ True constants
const ITEMS_PER_LOAD = 10;
const TIMESTAMP_CACHE_MAX_SIZE = 100;
const BACK_TO_TOP_THRESHOLD = 20;

// ✅ Regular const (use camelCase)
const db = new Dexie('RadioMemoDatabase');
const domCache = {};
```

#### Functions (camelCase - already correct)

```javascript
function loadLogs() {}
function formatTimestamp() {}
function detectBandFromFrequency() {}
async function handleFormSubmit() {}
```

---

### 7. Migration Strategy

**Option A: Gradual Migration** (Recommended)

1. Update CODING_CONVENTIONS.md to new standards
2. Add ESLint + Prettier configs
3. Apply to new code immediately
4. Refactor existing code in batches:
    - Phase 1: Critical/frequently-used files (app.js)
    - Phase 2: Less-frequently-modified files
    - Phase 3: Documentation updates

**Option B: Big Bang Migration**

1. Add ESLint + Prettier configs
2. Run `eslint --fix` and `prettier --write` on all files
3. Test thoroughly
4. Single commit with all changes

**Recommendation**: Option A for lower risk

---

### 8. Updated CODING_CONVENTIONS.md Sections

#### Section: Naming Conventions

````markdown
### Variables

- Use `camelCase` for all variable names
- Use descriptive names that clearly indicate purpose
- Examples:
    ```javascript
    const totalCount = 10;
    const frequencyUnit = 'MHz';
    let currentPage = 1;
    let isLoadingLogs = false;
    ```
````

### Constants

- Use `SCREAMING_SNAKE_CASE` for compile-time constants only
- Use `camelCase` for const objects, arrays, and functions
- Examples:

    ```javascript
    // ✅ True constants (primitive config values)
    const ITEMS_PER_LOAD = 10;
    const MAX_RETRIES = 3;
    const API_TIMEOUT_MS = 5000;

    // ✅ Const but not SCREAMING (objects/functions)
    const db = new Dexie('RadioMemoDatabase');
    const domCache = {};
    const timestampCache = new Map();
    ```

### Functions

- Use `camelCase` for all function names
- Use descriptive, action-oriented names (verbs)
- Examples:

    ```javascript
    function loadLogs() {}
    function formatTimestamp() {}
    async function handleFormSubmit() {}

    const exportLogs = async function () {};
    ```

### IDs and Classes (HTML/CSS)

- Use `kebab-case` for HTML element IDs
- Use `kebab-case` for CSS class names
- Examples:
    ```html
    <div id="new-log-form"></div>
    <button class="btn-primary"></button>
    ```

````

---

### 9. Code Quality Tools Setup

**package.json** (recommended for linting/formatting scripts)
```json
{
  "name": "radio-memo",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "lint": "eslint *.js",
    "lint:fix": "eslint *.js --fix",
    "format": "prettier --write \"*.{js,css,html,md}\"",
    "format:check": "prettier --check \"*.{js,css,html,md}\""
  },
  "devDependencies": {
    "eslint": "^8.57.0",
    "prettier": "^3.2.4"
  }
}
````

**Installation** (optional, can use without npm):

```bash
# If using Node.js for development
npm install --save-dev eslint prettier

# Run linting
npm run lint

# Auto-fix issues
npm run lint:fix

# Format all files
npm run format
```

---

### 10. Git Hooks (Optional but Recommended)

**.husky/pre-commit** (if using husky)

```bash
#!/bin/sh
npm run lint
npm run format:check
```

**Or use lint-staged**:

```json
{
    "lint-staged": {
        "*.js": ["eslint --fix", "prettier --write"],
        "*.{css,html,md}": ["prettier --write"]
    }
}
```

---

### 11. Editor Integration

**VS Code** (`.vscode/settings.json`)

```json
{
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "eslint.validate": ["javascript"],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

**Extensions to Install**:

- ESLint (dbaeumer.vscode-eslint)
- Prettier (esbenp.prettier-vscode)

---

## Benefits of Adopting Industry Standards

### 1. **Tooling Compatibility**

- ✅ Works with ESLint out-of-the-box
- ✅ Compatible with Prettier defaults
- ✅ Integrates with most IDE/editor plugins
- ✅ Easier onboarding for new developers

### 2. **Community Standards**

- ✅ Matches 95%+ of JavaScript projects
- ✅ Aligns with all major style guides
- ✅ Easier to find examples and documentation
- ✅ Better code reviews from external contributors

### 3. **Automated Formatting**

- ✅ Prettier can auto-format on save
- ✅ ESLint can auto-fix many issues
- ✅ Reduces manual code review time
- ✅ Eliminates style debates

### 4. **Future-Proof**

- ✅ Standards are stable and widely adopted
- ✅ Tools will continue to support these conventions
- ✅ New team members already familiar with standards
- ✅ Easier migration to frameworks if needed

---

## Risks and Mitigation

### Risk 1: Breaking Changes

**Impact**: High - All variable names need updating
**Mitigation**:

- Use search-replace with regex
- Test thoroughly after changes
- Update in phases (critical files first)

### Risk 2: Learning Curve

**Impact**: Low - Team familiar with snake_case
**Mitigation**:

- Document migration clearly
- Provide examples
- Set up ESLint to catch issues early

### Risk 3: Git History Disruption

**Impact**: Medium - Large refactor commit
**Mitigation**:

- Use `git blame --ignore-rev` to skip refactor commits
- Document refactor commit in `.git-blame-ignore-revs`
- Keep functional changes separate from style changes

---

## Recommendation

### ✅ ADOPT the following changes:

1. **Immediate** (New Code):
    - Use camelCase for all new variables
    - Use SCREAMING_SNAKE_CASE only for true constants
    - Add ESLint and Prettier configs

2. **Phase 1** (1-2 weeks):
    - Refactor app.js to camelCase
    - Update CODING_CONVENTIONS.md
    - Document migration in README

3. **Phase 2** (2-4 weeks):
    - Refactor remaining JavaScript files
    - Format CSS/HTML with Prettier
    - Update all documentation

4. **Phase 3** (Ongoing):
    - Enable pre-commit hooks
    - Add CI/CD linting checks
    - Enforce standards for new PRs

---

## Example: Before and After

### Before (Current)

```javascript
// Variables
const total_count = 10;
let is_loading_logs = false;
const dom_cache = {};
const timestamp_cache = new Map();

// Functions (already correct)
function loadLogs() {
    const logs_container = dom_cache.logs_container;
    const back_to_top_link = dom_cache.back_to_top_link;
}
```

### After (Proposed)

```javascript
// Variables
const totalCount = 10;
let isLoadingLogs = false;
const domCache = {};
const timestampCache = new Map();

// Constants (only for true config values)
const ITEMS_PER_LOAD = 10;
const CACHE_MAX_SIZE = 100;

// Functions (unchanged)
function loadLogs() {
    const logsContainer = domCache.logsContainer;
    const backToTopLink = domCache.backToTopLink;
}
```

---

## Action Items

- [ ] Review and approve this proposal
- [ ] Create `.eslintrc.json` configuration
- [ ] Create `.prettierrc.json` configuration
- [ ] Update CODING_CONVENTIONS.md
- [ ] Create migration plan document
- [ ] Refactor app.js as pilot (Phase 1)
- [ ] Test thoroughly
- [ ] Update remaining files
- [ ] Add pre-commit hooks (optional)
- [ ] Document in README

---

## References

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
- [StandardJS](https://standardjs.com/)
- [ESLint Rules](https://eslint.org/docs/latest/rules/)
- [Prettier Options](https://prettier.io/docs/en/options.html)
- [MDN JavaScript Coding Style](https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Writing_style_guide/Code_style_guide/JavaScript)

---

**Conclusion**: Adopting industry-standard naming conventions will improve tooling compatibility, reduce onboarding time, and align the project with 95%+ of the JavaScript ecosystem.
