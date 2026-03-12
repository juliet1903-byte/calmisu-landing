

## Fix: White page caused by mismatched base path

**Root cause**: `vite.config.ts` sets `base: "/calmisu-landing/"`, so Vite generates all asset URLs prefixed with `/calmisu-landing/`. The Lovable preview serves from `/`, so the browser can't find the JS bundle — nothing renders.

**Solution**: Make the base path conditional — use `/calmisu-landing/` only for production builds (GitHub Pages), and `/` for development (Lovable preview).

### Changes

**`vite.config.ts`** — Change line 6 and line 18:
```ts
// Replace the hardcoded base
const base = "/calmisu-landing/";

// With a conditional based on mode
export default defineConfig(({ mode }) => {
  const base = mode === "production" ? "/calmisu-landing/" : "/";
  // rest stays the same...
});
```

Also update the `rewritePublicPaths` plugin to only rewrite in production mode.

This is a one-file, ~5-line change. Everything else stays untouched.

