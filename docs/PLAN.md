# Portfolio Build Plan: Ahmad Yasser Morsy

## Stack decision
- **Next.js (App Router) + TypeScript**: scalable, with server API routes built in.
- **Tailwind CSS + Framer Motion + Lenis (smooth scroll) + tsparticles / React Three Fiber (backgrounds)**.
- **Zod** for validation. **Brevo REST API** for email.
- **DB-ready**: `src/lib/db/index.ts` is the single adapter point. Plug in Prisma + Postgres (Neon/Supabase) later without touching routes.
- **Hosting: Vercel.** It has first-class Next.js support, serverless API routes, preview deploys per PR, and zero-config env vars. Cloudflare needs adapters and has Node API limits, so choose it only if costs or edge needs change later.

## Design rules
1. Dark, futuristic tech theme: neon cyan/violet accents, glassmorphism cards, grid glow.
2. Interactive background (`ParticleField`, `GridGlow`) that reacts to the pointer. Disable or lighten it on mobile and when `prefers-reduced-motion` is set.
3. Smooth scrolling (`SmoothScroll`), section reveals (`Reveal`), page transitions (`PageTransition`).
4. Mobile-first and responsive from 320px to 4K. Use fluid type (`clamp`), a mobile nav drawer, and touch-sized targets.

## Page sections (single page, `src/app/(site)/page.tsx`)
Hero → About → Skills → Experience → Education → Languages → Booking → Contact → Footer.
All content comes from `src/content/*` (taken from the CV). Components hold no hard-coded text.

## Wiring rules
- Every nav item and CTA reads its target from `src/config/navigation.ts`. Each section has a matching `id`.
- Section ids: `#hero #about #skills #experience #education #languages #booking #contact`.
- Internal links use `next/link` or anchors. External links use `rel="noopener noreferrer"`.
- CV download button → `/cv/Ahmad-Yasser-CV.pdf` (add the file to `public/cv`).
- Contact links come from `src/config/site.ts`: mailto, tel, LinkedIn.

## Assets
- Use `next/image` for images, with explicit width/height and `priority` on the hero image.
- Use `next/font` to self-host fonts. Do not use CDN links.
- Put icons in `public/icons`. Use SVG or `lucide-react`.
- Add favicon, OG image and `sitemap.ts`/`robots.ts`.
- Done when `next build` passes with no 404s or console errors.

## Backend
- `POST /api/contact`: validate (`validation/contact.ts`), rate-limit, honeypot field, send two emails via Brevo (notification to Ahmad, auto-reply to the visitor).
- `GET /api/calendar/slots?date=`: return available slots from `config/site.ts` (working days, hours, timezone Africa/Cairo) minus booked ones.
- `POST /api/calendar/book`: validate, check the slot is free, save it (DB adapter, with in-memory or file fallback until a DB exists), send confirmation emails with an `.ics` attachment via Brevo.
- All Brevo calls live in `src/lib/brevo/*`. Routes never call fetch to Brevo directly.
- Env vars are validated in `src/config/env.ts`. See `.env.example`:
  `BREVO_API_KEY, BREVO_SENDER_EMAIL, BREVO_SENDER_NAME, CONTACT_RECEIVER_EMAIL, NEXT_PUBLIC_SITE_URL, DATABASE_URL (later)`.
- Details: `docs/BREVO.md`. Git: `docs/GIT_SETUP.md`.

## Build order for the builder models
1. Config and tooling: package.json, Tailwind, tsconfig, env, `site.ts`, `navigation.ts`.
2. Content files, filled from the CV.
3. Layout, ui, animations and backgrounds.
4. Sections, then page assembly.
5. Backend: validation, brevo lib, API routes, forms, calendar UI.
6. QA: responsive check, link and asset audit, Lighthouse, build.
7. Deploy to Vercel.
