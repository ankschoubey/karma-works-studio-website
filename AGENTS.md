# AGENTS.md - Folex Lite Astro

This is an Astro.js project with Tailwind CSS, supporting multilingual sites and i18n.

## Project Overview

- **Framework**: Astro 6.x with TypeScript
- **Styling**: Tailwind CSS v4 with `@tailwindcss/vite`
- **Content**: Static pages with optional MDX
- **i18n**: Built-in multilingual support with language-specific routing

## Build Commands

```bash
npm run dev          # Start dev server with TOML watcher (live config reload)
npm run build        # Production build with sitemap processing
npm run preview      # Preview production build locally
npm run astro-check  # Run Astro TypeScript type checking
npm run format       # Run Prettier on ./src directory
npm run generate-favicons  # Generate favicons from config
```

**Single file type checking:**

```bash
npx astro check ./src/layouts/Base.astro
```

**Watch mode for type checking:**

```bash
npx astro check --watch
```

## Code Style Guidelines

### Formatting

- **2-space indentation** (enforced via .editorconfig)
- **Prettier** is the default formatter for all file types
- VS Code settings: `editor.defaultFormatter: esbenp.prettier-vscode`
- Auto-format on save enabled for markdown files

### File Organization

```
src/
├── components/         # UI components
├── layouts/            # Page layouts
│   ├── components/     # Layout-specific components
│   ├── helpers/       # Helper components (Icons, etc.)
│   └── shortcodes/    # Reusable shortcode components
├── lib/
│   └── utils/         # Utility functions (TypeScript)
├── pages/             # Astro pages
├── i18n/              # Translation files (JSON)
├── config/            # Site configuration (JSON)
└── styles/            # Global CSS
```

### Import Conventions

**Path Aliases** (defined in tsconfig.json):

```typescript
import Component from "@/components/path"; // src/layouts/components/*
import Shortcode from "@/shortcodes/path"; // src/layouts/shortcodes/*
import Helper from "@/helpers/path"; // src/layouts/helpers/*
import Utils from "@/lib/utils/file"; // src/lib/utils/*
import Config from "@/config/file"; // src/config/*
import Generated from "@generated/path"; // .generated/*
```

**Import Order** (use this grouping):

1. Node.js built-ins (`node:path`, etc.)
2. External packages
3. Internal aliases (`@/...`)
4. Relative imports (`./...`, `../...`)

### Astro Components (.astro)

**Frontmatter structure:**

```astro
---
import "external-css";
import "@/styles/global.css";
import type { Type } from "@/lib/utils/types";
import Component from "@/components/Component.astro";

interface Props {
  propA: string;
  propB?: boolean;
}

const { propA, propB = false } = Astro.props;
---
```

**Template guidelines:**

- Use `class:list` for conditional classes
- Use `<slot />` for content projection
- Prefer `{#if}` / `{#each}` / `{#await}` over ternary operators for complex logic

### TypeScript Conventions

**Strict mode enabled** - no implicit any, strict null checks.

**Type definitions:**

```typescript
// Props interface at top of file
interface Props {
  title: string;
  items?: Array<{ id: string; label: string }>;
  [key: string]: any; // For dynamic props
}

// Export utility types/functions
export const formatDate = (date: Date): string => { ... };

// Default exports for utilities
export default function helper() { ... };
```

**Type checking:**

- Uses Astro's strict TypeScript config
- Avoid `any` unless absolutely necessary
- Use generic types when possible

### Naming Conventions

| Type             | Convention               | Example                  |
| ---------------- | ------------------------ | ------------------------ |
| Components       | PascalCase               | `SinglePageLayout.astro` |
| Utilities        | camelCase                | `formatRelativeDate.ts`  |
| Types/Interfaces | PascalCase               | `JSONLDProps`            |
| Constants        | camelCase or UPPER_SNAKE | `enabledLanguages`       |
| Files (JS/TS)    | camelCase                | `textConverter.ts`       |
| Files (Astro)    | PascalCase               | `Header.astro`           |
| CSS classes      | Tailwind utility classes | `class="flex gap-4"`     |

### Error Handling

- **Missing content**: Use `console.warn()` (not `console.error`)
  ```typescript
  if (!content) {
    console.warn("No content provided to titleify");
    return "";
  }
  ```
- **Missing config**: Throw descriptive errors
  ```typescript
  if (!language) {
    throw new Error("Default language configuration not found");
  }
  ```
- **Async operations**: Use try/catch with fallbacks
  ```typescript
  try {
    menu = await import(`../../../src/config/menu.${lang}.json`);
  } catch (error) {
    menu = await import(`../../../src/config/menu.${defaultLanguage}.json`);
  }
  ```

### JSON Configuration Files

- Language files: `src/i18n/{lang}.json`
- Menu files: `src/config/menu.{lang}.json`
- Site config: TOML file (processed to `.generated/config.generated.json`)

### Comments

- Use JSDoc style for functions and exports
- Minimal inline comments - code should be self-documenting
- `// prettier-ignore` for specific formatting exceptions

### Testing

No formal test suite exists. Manual testing via:

```bash
npm run dev      # Test in browser at localhost:4321
npm run build    # Verify production build succeeds
```

## Configuration Files

| File                 | Purpose                                  |
| -------------------- | ---------------------------------------- |
| `astro.config.mjs`   | Astro configuration                      |
| `tsconfig.json`      | TypeScript + path aliases                |
| `.prettierrc`        | Prettier plugins (astro, tailwind, toml) |
| `.editorconfig`      | Indentation, line endings                |
| `.markdownlint.json` | Markdown linting rules                   |

## Development Notes

- TOML config files trigger rebuilds via `scripts/toml-watcher.mjs`
- Sitemap is generated post-build, then drafts are removed
- Favicons are generated from `src/assets/images/favicon.png`
- SEO metadata handled via `JsonLdGenerator` utility
