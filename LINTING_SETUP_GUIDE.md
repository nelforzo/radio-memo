# Linting and Formatting Setup Guide

**Date**: 2025-12-27
**Purpose**: Guide for setting up ESLint and Prettier for Radio Memo project

---

## Files Created

### 1. `.eslintrc.json` - ESLint Configuration

**Purpose**: JavaScript linting rules aligned with industry standards

**Key Rules**:

- ✅ `camelcase`: Enforces camelCase for variables and properties
- ✅ `no-var`: Disallows `var` (use `const`/`let` only)
- ✅ `prefer-const`: Suggests `const` when variables aren't reassigned
- ✅ `semi`: Requires semicolons
- ✅ `quotes`: Enforces single quotes
- ✅ `indent`: 4-space indentation (matches current code)

### 2. `.prettierrc.json` - Prettier Configuration

**Purpose**: Automatic code formatting

**Key Settings**:

- `printWidth: 100` - Max line length
- `tabWidth: 4` - 4 spaces per indentation
- `singleQuote: true` - Use single quotes
- `semi: true` - Always use semicolons
- `trailingComma: "es5"` - Trailing commas where valid
- `endOfLine: "lf"` - Unix line endings

### 3. `.prettierignore` - Prettier Ignore File

**Purpose**: Exclude certain files from formatting

**Excluded**:

- `node_modules/`
- Build outputs
- Images and icons
- `manifest.json`

### 4. `package.json` - NPM Scripts

**Purpose**: Convenient commands for linting and formatting

**Scripts**:

- `npm run lint` - Check for linting errors
- `npm run lint:fix` - Auto-fix linting errors
- `npm run format` - Format all files
- `npm run format:check` - Check formatting without changes

### 5. `CODING_CONVENTIONS_REVISION.md` - Detailed Proposal

**Purpose**: Complete analysis and migration plan

---

## Quick Start (Optional - No Build Process Required)

### Option A: Use Without NPM (Manual)

You can use these tools without installing anything:

1. **VS Code Extensions** (Recommended):
    - Install ESLint extension
    - Install Prettier extension
    - Files will be checked/formatted automatically

2. **Online Tools**:
    - [ESLint Demo](https://eslint.org/demo/)
    - [Prettier Playground](https://prettier.io/playground/)

### Option B: Install Tools Locally (Recommended for CI/CD)

```bash
# Install Node.js (if not already installed)
# Visit: https://nodejs.org/

# Install dependencies
npm install

# Run linting
npm run lint

# Auto-fix linting issues
npm run lint:fix

# Format all files
npm run format

# Check formatting (dry run)
npm run format:check
```

---

## Key Naming Convention Changes

### Before (Current - snake_case)

```javascript
const total_count = 10;
let is_loading_logs = false;
const dom_cache = {};
const frequency_input = document.getElementById('frequency');

function loadLogs() {
    const logs_container = dom_cache.logs_container;
}
```

### After (Proposed - camelCase)

```javascript
const totalCount = 10;
let isLoadingLogs = false;
const domCache = {};
const frequencyInput = document.getElementById('frequency');

// True constants use SCREAMING_SNAKE_CASE
const ITEMS_PER_LOAD = 10;
const CACHE_MAX_SIZE = 100;

function loadLogs() {
    const logsContainer = domCache.logsContainer;
}
```

---

## Migration Steps

### Step 1: Decision

- [ ] Review `CODING_CONVENTIONS_REVISION.md`
- [ ] Decide: Keep snake_case OR adopt camelCase
- [ ] Get team consensus

### Step 2: If Adopting camelCase (Recommended)

#### Phase 1: Setup (30 minutes)

- [x] Create ESLint config (`.eslintrc.json`)
- [x] Create Prettier config (`.prettierrc.json`)
- [x] Create `package.json` with scripts
- [ ] Install VS Code extensions (optional)
- [ ] Test on a small file

#### Phase 2: Update Conventions (1 hour)

- [ ] Update `CODING_CONVENTIONS.md` with new rules
- [ ] Document migration in README
- [ ] Update examples in documentation

#### Phase 3: Refactor Code (4-6 hours)

- [ ] Refactor `app.js`:
    - [ ] Variables: snake_case → camelCase
    - [ ] Constants: Identify true constants → SCREAMING_SNAKE_CASE
    - [ ] Update all references
- [ ] Test thoroughly after each change
- [ ] Commit with clear message

#### Phase 4: Format Everything (1 hour)

- [ ] Run Prettier on all files
- [ ] Fix any ESLint errors
- [ ] Test thoroughly
- [ ] Commit formatting changes separately

#### Phase 5: Automation (Optional, 2 hours)

- [ ] Add pre-commit hooks (husky)
- [ ] Add CI/CD linting checks
- [ ] Document in README

### Step 3: If Keeping snake_case

#### Modify ESLint Config

Update `.eslintrc.json` to allow snake_case:

```json
{
    "rules": {
        "camelcase": "off"
    }
}
```

**Note**: This will make tooling integration harder and conflict with most JavaScript style guides.

---

## VS Code Integration

### Install Extensions

1. **ESLint**
    - Extension ID: `dbaeumer.vscode-eslint`
    - Auto-installs from `.eslintrc.json`

2. **Prettier**
    - Extension ID: `esbenp.prettier-vscode`
    - Auto-installs from `.prettierrc.json`

### Workspace Settings

Create `.vscode/settings.json`:

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
    },
    "files.eol": "\n"
}
```

---

## Testing the Setup

### Test ESLint

Create a test file `test-lint.js`:

```javascript
// This should trigger errors
var x = 1; // Error: no-var
const user_name = 'test'; // Error: camelcase (if enforced)
let y = 2; // Error: missing semicolon

function test() {
    // Error: space-before-function-paren
    console.log('test');
}
```

Run:

```bash
npm run lint test-lint.js
```

Expected errors for camelCase, no-var, semi, spacing.

### Test Prettier

Create a test file `test-format.js`:

```javascript
const x = { a: 1, b: 2 };
function test() {
    console.log('test');
}
```

Run:

```bash
npx prettier --write test-format.js
```

Should format to:

```javascript
const x = { a: 1, b: 2 };
function test() {
    console.log('test');
}
```

---

## Comparison: With vs Without Tooling

### Without Tooling (Current)

- ❌ Manual code review for style
- ❌ Inconsistent formatting
- ❌ Naming convention violations not caught
- ❌ Time spent on style debates
- ❌ Harder onboarding for new developers

### With Tooling (Proposed)

- ✅ Automatic error detection
- ✅ Consistent formatting on save
- ✅ Naming violations caught immediately
- ✅ No time wasted on style debates
- ✅ New developers follow standards automatically
- ✅ CI/CD can enforce standards

---

## Common ESLint Errors and Fixes

### Error: `'camelcase': identifier 'dom_cache' is not in camel case`

```javascript
// ❌ Error
const dom_cache = {};

// ✅ Fix
const domCache = {};
```

### Error: `'no-var': Unexpected var, use let or const instead`

```javascript
// ❌ Error
var count = 0;

// ✅ Fix
let count = 0;
// or
const count = 0;
```

### Error: `'prefer-const': 'x' is never reassigned. Use 'const' instead`

```javascript
// ❌ Error
let x = 5;
console.log(x);

// ✅ Fix
const x = 5;
console.log(x);
```

### Error: `'semi': Missing semicolon`

```javascript
// ❌ Error
const x = 5;

// ✅ Fix
const x = 5;
```

---

## FAQ

### Q: Do I need to install Node.js for this?

**A**: No, if using VS Code extensions. Yes, if you want to use npm scripts or CI/CD.

### Q: Will this break my existing code?

**A**: The config files won't break anything. Refactoring to camelCase requires manual changes and testing.

### Q: Can I use these tools without changing to camelCase?

**A**: Yes, but you'll need to disable the `camelcase` rule in ESLint.

### Q: What if I want to keep some snake_case variables?

**A**: You can allowlist specific patterns in ESLint config, but not recommended.

### Q: How long will migration take?

**A**: Estimated 6-10 hours total for complete migration and testing.

### Q: Will this affect performance?

**A**: No. Linting/formatting are development-time tools only. No runtime impact.

---

## Recommendations

### ✅ Recommended: Adopt Industry Standards

**Why**:

1. **95%+ of JavaScript projects use camelCase** for variables
2. **All major style guides** (Airbnb, Google, StandardJS) mandate camelCase
3. **Tool compatibility**: ESLint, Prettier, IDE plugins all expect camelCase
4. **Easier onboarding**: New developers already know camelCase
5. **Future-proof**: Standards won't change

**Effort**: Medium (6-10 hours)
**Benefit**: High (long-term maintainability, tooling support)

### ❌ Not Recommended: Keep snake_case

**Why not**:

1. Conflicts with 95% of JavaScript ecosystem
2. Requires custom ESLint config
3. Harder to integrate with tools
4. Confusing for new contributors
5. No major style guide supports it

---

## Next Steps

1. **Review** `CODING_CONVENTIONS_REVISION.md` for complete details
2. **Decide** whether to adopt camelCase or keep snake_case
3. **If adopting**: Follow migration steps above
4. **If keeping**: Disable `camelcase` rule in ESLint
5. **Test** thoroughly after any changes
6. **Document** decision in README

---

## Support and Resources

- **ESLint Documentation**: https://eslint.org/docs/latest/
- **Prettier Documentation**: https://prettier.io/docs/en/
- **Airbnb Style Guide**: https://github.com/airbnb/javascript
- **Google Style Guide**: https://google.github.io/styleguide/jsguide.html
- **MDN Code Style**: https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Writing_style_guide/Code_style_guide/JavaScript
