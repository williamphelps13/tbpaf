TahoeBPAF.org — MVP Plan

1) Overview
- Goal: Ship a one page, mobile-first site for Tahoe Ballet & Performing Arts Foundation with clear sections  and smooth in-page navigation.
- Here are the sections:
  - Landing Page
  - Next Event
  - About
  - Foundation Board
  - Contact
  - Previous Events
- Approach: Static-first Next.js app with smooth anchor scrolling. Content managed in a free CMS; publish fast via Vercel.
- Audience: Local community and visitors seeking event info, about info, foundation board details, and contact.

1) Stack (MVP)
- Framework: Next.js (Latest version)
- Styling: Tailwind CSS; darker background by default; no dark-mode toggle
- UI Lib: None for MVP (skip shadcn/ui)
- CMS: Sanity (free tier) for all editable content
- Email: Formspree (free) for MVP contact form
- Deploy: Vercel (free plan); GitHub integration for previews

1) Information Architecture (One Page)
- Sticky navbar with anchored links to sections; active-section highlighting as user scrolls.
- Sections (top → bottom):
  - Landing: headline, subhead, hero image, FB/IG links
  - Next Event: title, date/time, location, short blurb, poster
  - About: short text block (rich text allowed)
  - Foundation Board: photo, name, role, brief bio, fun fact
  - Previous Events: simple grid; click event to expand inline thumbnails; click thumb opens native <dialog>
  - Contact: name, email, message → Formspree

1) Intitial
- Landing: headline, subhead, hero image that can be changed by a non-technical individual
- Absolute minimal styling using default Tailwind utility classes
- Static rendering with ISR + webhook revalidation

1) MVP Scope (What’s In / Out)
- In:
  - Sticky navbar + smooth scroll
  - Contact form (name, email, message) → Formspree
- Out (defer):
  - Inline event expanders + native <dialog> image viewer
  - Accessibility: keyboard focus, aria labels, respects reduced motion
  - Arrow-key navigation & hint overlay
  - 10/10/70/10 prev/next bars
  - “Wow” effects (Canvas/Three.js/Rive)
  - Lightbox library; complex carousels
  - Contact “Reason” select

1) CMS Model (Sanity)
- Singletons:
  - landing: { headline, subhead, heroImage, socials[] }
  - about: { body (portable text) }
- Documents:
  - event: { title, date, location?, blurb?, images[] }
  - boardMember: { name, role?, photo, bioShort, funFact }

1) Data Fetching & Publishing
- Server Components fetch via GROQ at build/ISR time using next-sanity client (useCdn: true).
- Revalidation: /api/revalidate endpoint triggers revalidatePath('/') from Sanity webhook for near-instant updates.
- Images: Sanity image CDN → Next <Image /> with width/height to avoid CLS; prefer AVIF/WebP.
- Videos (later): store YouTube/Vimeo URLs; start with images-only in MVP.

1) UX/Design Notes
- Tone: cultural, elegant, community-forward (avoid “salesy landing page” vibe).
- Typography: restrained; real photos from performances; simple, authentic styling.
- Previous Events: card → inline expander → <dialog> for full image; no arrow nav yet.
- Contact: minimal form; clear success/failure messaging (Formspree default is OK for MVP).

1) Performance & Accessibility
- Static pages from Vercel edge; tiny JS footprint.
- Lazy-load images; set explicit dimensions or aspect-ratio.
- Respect prefers-reduced-motion; keep animations minimal in MVP.
- Keyboard: focusable controls, <dialog> gets focus, Esc to close.

1) File Map (MVP)
- app/
  - layout.tsx
  - page.tsx
  - api/revalidate/route.ts      # Sanity webhook target
  - (components)/
    - Navbar.tsx
    - EventCard.tsx              # inline expander + <dialog>
  - (data)/
    - cms.ts                     # Sanity client + queries + urlFor
- sanity/
  - sanity.config.ts
  - schemas/
    - landing.ts
    - event.ts
    - about.ts
    - boardMember.ts
- public/
  - icons/* (optional)

1)  Implementation Checklist
- Repo & Tooling
  - [ ] Initialize Next.js + Tailwind in repo
- CMS
  - [ ] Add Sanity project; create schemas (landing, about, event, boardMember)
  - [ ] Configure sanity.config.ts; optionally embed Studio at /studio
  - [ ] Set SANITY_PROJECT_ID, SANITY_DATASET, SANITY_API_VERSION envs
  - [ ] Add /api/revalidate and Sanity webhook (shared secret)
- App
  - [ ] Navbar with anchored links + active highlighting via IntersectionObserver
  - [ ] Sections wired to GROQ data (server components)
  - [ ] Contact form (Formspree action) with minimal validation
- Deployment
  - [ ] Vercel project + env vars
  - [ ] Preview deploy; verify revalidation and form submission
  - [ ] Core Web Vitals check; image sizes adjusted as needed

1)  Open Questions
- Brand palette & typography specifics (fonts, sizes)?
- Sanity Studio location: embedded at /studio

1)  Next Steps (Pick One)
- A) Scaffold code now: I’ll generate the Next.js + Tailwind app, components, Sanity client/queries, schemas, and /api/revalidate.
- B) Start with CMS: I’ll add Sanity config + schemas and a mock data loader so you can populate content first.
- C) Lite start: I’ll stub the sections with static JSON and swap to Sanity after content is approved.

Tell me which path you prefer: I prefer C

