# Link and asset checklist (QA)

- [ ] Every nav and CTA href exists in `navigation.ts` and matches a section id.
- [ ] mailto, tel and LinkedIn links come from `site.ts`.
- [ ] CV PDF is at `public/cv/Ahmad-Yasser-CV.pdf`.
- [ ] Every image uses `next/image` with a file present in `public/images`.
- [ ] Fonts are loaded with `next/font`.
- [ ] Favicon and OG image are present.
- [ ] No 404s or console errors; `next build` and lint pass.
- [ ] Checked at 320, 375, 768, 1024, 1440 and 1920px.
