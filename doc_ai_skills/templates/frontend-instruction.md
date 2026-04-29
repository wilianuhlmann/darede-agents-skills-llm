# Frontend Development Instructions for AI Agent

## Role and Context

You are a senior frontend developer specializing in **[FRAMEWORK + VERSION]**, **[LANGUAGE]**, and **[BUILD TOOL]**. Your task is to generate production-ready frontend code for **[PROJECT_NAME]** — [ONE-LINE PRODUCT DESCRIPTION].

---

## Documentation (CRITICAL)

### Language

**ALL documentation MUST be written in [English / Portuguese — pick one].**

### Before implementing any feature, read:

- `.ai/frontend-instruction.md` — this file
- `.ai/ARCHITECTURE.md` — current architecture and patterns
- `.ai/contract.md` — API contracts (request/response JSON)
- `.ai/features/<feature_name>/<feature_name>.md` — feature-specific docs
- `[/frontend/README.md]`

### After implementing any feature, update:

| File | What to Update |
|------|----------------|
| `.ai/contract.md` | Add/modify API endpoints with request/response JSON |
| `.ai/ARCHITECTURE.md` | New routes, components, or architectural changes |
| `.ai/features/<name>/<name>.md` | Create/update detailed feature documentation |
| `[/frontend/README.md]` | New routes or major features |

### Feature Documentation Requirements

Every `features/<name>/<name>.md` MUST include:

1. **Overview** — what the feature does
2. **Routes** — list of public/protected routes
3. **Components** — components created (path, props summary)
4. **State** — Zustand / Redux / context stores touched
5. **API endpoints used** — link to entries in `contract.md`
6. **Dependencies** — new libraries added

---

## Critical Constraints

### MUST USE

- **[Framework + version]** — exact (e.g. React 18.3.1)
- **[Language]** with strict mode (e.g. TypeScript `strict: true`)
- **[Build tool]** (e.g. Vite 5.x)
- **[Package manager]** exclusively (e.g. Yarn v3 / pnpm / npm — pick one)
- **[Routing library]** for navigation
- **[State management]** (e.g. Zustand)
- **[Form library]** + **[Schema validation]** (e.g. react-hook-form + zod)
- **[CSS solution]** (e.g. Tailwind)
- **[UI library]** (e.g. shadcn/ui) — single library only

### NEVER USE

- [Banned meta-framework, e.g. Next.js / Remix when SPA is required]
- [Banned package manager — pick one and stay]
- [Banned UI mixing, e.g. Bootstrap + Chakra]
- Class components / legacy patterns
- `require()` (ESM only)
- `console.log` in committed code
- `any` types unless absolutely necessary

---

## Technology Stack

### Core

```json
{
  "[framework]": "[exact version]",
  "[language]": "[major version]",
  "[build tool]": "[major version]"
}
```

### Required Libraries

| Library | Version | Purpose |
|---------|---------|---------|
| [http client] | latest | HTTP client |
| [router] | [^x.y] | Routing |
| [forms] | latest | Form management |
| [schema validation] | latest | Validation |
| [state management] | latest | Global state |
| [styling] | latest | Styling |
| [icons] | latest | Icon library |
| [optional: workflow / charts / etc] | latest | [Purpose] |

---

## Project Structure

```
src/
├── components/       # Reusable UI components
│   ├── ui/           # base components (shadcn or equivalent)
│   ├── layout/       # Sidebar, Header, Layout
│   └── [domain]/     # Feature-specific components
├── pages/            # Page-level components
│   ├── public/       # Public/marketing routes
│   └── app/          # Protected app routes
├── hooks/            # Custom hooks
├── store/            # State stores
├── services/         # API services
├── types/            # Shared types
├── lib/              # Utilities
├── routes/           # Route definitions
├── App.[tsx|jsx]
└── main.[tsx|jsx]
```

---

## Multi-Tenant / Auth Rules

[Replace with the project's auth model. Examples:
- The frontend NEVER sends tenant identifiers in request bodies — they come from the JWT.
- Role-based UI: gate admin features by `user.role === 'admin'`.
- Auth store shape:]

```typescript
interface AuthUser {
  id: string
  email: string
  name: string
  organizationId: string
  role: 'admin' | 'member'
}
```

---

## Code Standards

### Component Pattern

```typescript
interface ButtonProps {
  label: string
  onClick: () => void
}

export default function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>
}
```

### Mandatory Practices

1. **Exports** — `export default` for components, named for utilities.
2. **Imports** — ESM only.
3. **Routing** — centralize in `routes/`.
4. **State** — stores live in `store/`.
5. **API** — single axios/fetch instance in `services/api.ts` with auth interceptor.
6. **Styling** — utility-first via [Tailwind / CSS modules].
7. **Accessibility** — ARIA attributes where appropriate.
8. **No comments narrating obvious code.**
9. **No dead code, unused imports, or `console.log`.**

---

## Required Configuration Files

| File | Purpose |
|------|---------|
| `package.json` | Dependencies + scripts |
| `[tsconfig.json / jsconfig.json]` | Compiler config (strict mode on) |
| `[vite.config.ts / next.config.js]` | Build tool config |
| `[tailwind.config.js]` | Styling config |
| `.env.example` | Environment variables template |

### `.env.example`

```
[VITE_/NEXT_PUBLIC_/REACT_APP_]API_BASE_URL=
[VITE_]APP_NAME=[PROJECT_NAME]
```

---

## API Service

`src/services/api.ts` must include:

- Single client instance with the base URL.
- Request interceptor adding the `Authorization: Bearer <token>` header.
- Response interceptor handling `401` (auto-logout) and surfacing the standard error envelope.
- Typed wrappers for endpoints documented in `.ai/contract.md`.

---

## Validation Checklist

Before considering the project complete:

### Development

- [ ] Dependencies use exact versions specified
- [ ] `[dev command]` starts successfully
- [ ] Strict typing enabled
- [ ] No mixing of UI libraries
- [ ] Routes configured
- [ ] No console errors

### Build

- [ ] `[build command]` completes without errors
- [ ] Output dir contains `index.html` at root
- [ ] Assets are hash-named for cache busting
- [ ] Bundle size within budget

### Deploy

- [ ] Env vars configured per stage
- [ ] HTTPS / SSL configured
- [ ] SPA fallback (404 → index.html) configured at the CDN/host
- [ ] CORS configured on the backend for the frontend origin

---

**Last Updated**: [YYYY-MM]
