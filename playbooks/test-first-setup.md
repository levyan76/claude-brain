# Playbook — Test-first setup

> Trigger : new project OR existing project without tests. Read before proposing setup.

## Why (lessons from a real SaaS)

- Catches bugs immediately (e.g. pricing double-margin, role-based routing slip)
- Refactor with confidence (a failing test = direct signal)
- Avoids hours of manual testing at project end
- Living documentation of expected behavior

## Install

```bash
npm install -D vitest @vitest/ui jsdom @testing-library/react @testing-library/jest-dom
npm install -D @playwright/test @axe-core/playwright
npx playwright install chromium
```

## Configs

- `vitest.config.ts` — alias `@/`, jsdom env, outputFile in `test-results/`
- `playwright.config.ts` — prod baseURL + mobile-chrome project
- `test-setup.ts` — imports `@testing-library/jest-dom`
- `tests/unit/` + `tests/e2e/` structured
- `package.json` scripts : `test`, `test:e2e`, `test:all`
- `.gitignore` : `test-results/`, `playwright-report/`, `.playwright-cache/`

## 5 mandatory first tests

1. ✅ Smoke E2E — root, login, 404 don't crash
2. ✅ Unit test — critical business functions (pricing, geometry, etc.)
3. ✅ i18n FR/EN consistency if bilingual
4. ✅ axe-core a11y on public pages
5. ✅ DB security — RLS + Supabase advisors if applicable
