# Poddero Website — Customization Plan

## Overview

Converting the VIIO Astro template into a Poddero agency website. Poddero provides podcast editing, distribution, and marketing services.

Content source files are in `website-content/` (except Results/Case Studies and Testimonials).

---

## 1. Branding & Identity

| Item | Current | New |
|------|---------|-----|
| Site name | VIIO | Poddero |
| `logoAlt` in `Layout.astro` | "VIIO" | "Poddero" |
| `logoAlt` in `Footer.astro` | "VIIO" | "Poddero" |
| Logo SVG (`src/assets/logo.svg`) | VIIO logo | Generate simple svg of text "Poddero" |


---

## 2. Layout & Navigation (`src/layouts/Layout.astro`)

### Changes
- Update `logoAlt` from "VIIO" to "Poddero"
- Replace nav links array:
  ```
  Current:  [{Home, /}, {About, /}, {Contact, /}]
  New:      [{Home, /#hero}, {Services, /#services}, {Why Poddero, /#why-poddero}, {How It Works, /#how-it-works}, {FAQ, /#faq}, {Contact, /#contact}]
  ```
- Remove header `link1` (Login)
- Replace header `link2` (Get Started) — change to "Book a Consultation" pointing to `#contact`
- Update footer `logoAlt` and `links` to match

### File: `src/layouts/Layout.astro`
- **Line 12-16:** Replace `links` array with new nav items
- **Line 31:** Change `logoAlt="VIIO"` → `logoAlt="Poddero"`
- **Line 33:** Change `link1={{ title: "Login", url: "#" }}` → remove 
- **Line 34:** Change `link2={{ title: "Get Started", url: "#" }}` → `link2={{ title: "Book a Free Consultation", url: "#contact" }}`
- **Line 39:** Change `logoAlt="VIIO"` → `logoAlt="Poddero"`

---

## 3. Header (`src/components/Header.astro`)

### Changes
- No code changes needed to the component itself — all driven by props from `Layout.astro`
- The header already supports dynamic `links`, `link1`, and `link2` props
- Mobile menu will automatically use the new nav links

---

## 4. Hero Section (`src/components/HeroHome.astro`)

### Current Design
- Full-viewport height (`min-height: 100vh`)
- Left-aligned content with title, subtitle, CTA button
- WebGL 3D sphere placeholder on the right (`[data-gl-place]`)
- Gradient background with warm peach/orange/purple tones
- Backdrop blur effect on background

### New Design
- **Same layout structure** — left content, right 3D element 
- **Content update only** — no CSS changes needed

### Props to pass (from `website-content/hero.md`)
```js
<HeroHome
  title="Launch, Grow, and Scale Your Podcast With Confidence"
  subtitle="From professional editing and publishing to distribution and marketing, Podderro helps podcasters reach more listeners while saving hours every week."
  cta={{
    title: "Book a Consultation",
    url: "#contact",
  }}
/>
```


### File: `src/pages/index.astro`
- **Lines 26-33:** Replace props with new content

---

## 5. Services Section — NEW (`src/components/Services.astro`)

### Current State
Does not exist. The `GalleryText.astro` component currently occupies this position but is a product feature showcase — not suitable.

### Design Plan
Create a new `Services.astro` component with:

**Structure:**
```
┌─────────────────────────────────────────┐
│  [GradientBackground with brand colors] │
│                                         │
│  ┌─ Header ──────────────────────────┐  │
│  │  Headline (centered)              │  │
│  │  Subheadline (centered)           │  │
│  └───────────────────────────────────┘  │
│                                         │
│  ┌─ Service Cards Grid ──────────────┐  │
│  │  ┌──────┐  ┌──────┐              │  │
│  │  │ Card │  │ Card │              │  │
│  │  │  1   │  │  2   │              │  │
│  │  └──────┘  └──────┘              │  │
│  │  ┌──────┐  ┌──────┐              │  │
│  │  │ Card │  │ Card │              │  │
│  │  │  3   │  │  4   │              │  │
│  │  └──────┘  └──────┘              │  │
│  └───────────────────────────────────┘  │
│                                         │
│  [data-gl-place] (if keeping 3D mic)    │
└─────────────────────────────────────────┘
```

**Props interface:**
```ts
interface Service {
  icon: string;       // SVG icon name or path
  title: string;      // e.g., "Audio Enhancing - Crystal-Clear Audio"
  text: string;       // Supporting description
}

interface Props {
  headline: string;
  subheadline: string;
  services: Service[];
}
```

**Card design:**
- Each card: icon (top) + title + description text
- Cards in a 2-column grid on desktop, 1-column on mobile
- Subtle background (white or semi-transparent with backdrop blur)
- Rounded corners, padding, slight shadow or border
- Hover state: subtle lift or border color change

**Gradient background:**
- Reuse `GradientBackground.astro` with custom Poddero brand colors passed via `colors` prop
- Position behind the cards section

**Props to pass (from `website-content/services.md`):**
```js
<Services
  headline="Launch, Grow, and Scale Your Podcast With Confidence"
  subheadline="Launch, Grow, and Scale Your Podcast With Confidence"
  services={[
    {
      icon: "audio-wave",
      title: "Audio Enhancing - Crystal-Clear Audio",
      text: "Remove background noise, balance audio levels, and enhance sound quality to deliver a professional listening experience.",
    },
    {
      icon: "scissors",
      title: "Podcast Editing - Polished Episodes Ready to Publish",
      text: "We cut mistakes, tighten conversations, add intros and outros, and prepare each episode for a smooth and engaging listen.",
    },
    {
      icon: "repurpose",
      title: "Content Repurposing - Turn One Episode Into Weeks of Content",
      text: "Transform podcast episodes into short-form videos, social media posts, clips, quotes, and promotional assets that extend your reach.",
    },
    {
      icon: "users",
      title: "Guest Booking - Connect with guests that add value",
      text: "We research, outreach, and coordinate guest appearances to help you create compelling conversations and grow your audience.",
    },
  ]}
/>
```

### Files to create/modify
- **Create:** `src/components/Services.astro`
- **Modify:** `src/pages/index.astro` — import and add `<Services>` after `<HeroHome>`

---

## 6. Why Poddero Section (`src/components/Introducing.astro`)

### Current Design
- Centered text block with backdrop blur
- Title + paragraph + optional CTA button
- Single column, max-width 50rem, centered
- WebGL GL placeholder positioned absolute right

### New Design
Keep the heading + subheading layout, then add a grid of benefit cards below, then CTA.

**Structure:**
```
┌─────────────────────────────────────────┐
│  ┌─ Header ──────────────────────────┐  │
│  │  "Why Choose Poddero" (centered)  │  │
│  │  Description paragraph (centered) │  │
│  └───────────────────────────────────┘  │
│                                         │
│  ┌─ Benefits Grid (3x2) ─────────────┐  │
│  │  ┌────────┐ ┌────────┐ ┌────────┐│  │
│  │  │ Shield │ │ Clock  │ │ Rocket ││  │
│  │  │Consist │ │ Save   │ │ Growth ││  │
│  │  │ Quality│ │ Hours  │ │ Focused││  │
│  │  └────────┘ └────────┘ └────────┘│  │
│  │  ┌────────┐ ┌────────┐ ┌────────┐│  │
│  │  │ Layers │ │ Globe  │ │ Users  ││  │
│  │  │ All-in │ │ Wider  │ │Dedica- ││  │
│  │  │  One   │ │Distrib.│ │ted Sup.││  │
│  │  └────────┘ └────────┘ └────────┘│  │
│  └───────────────────────────────────┘  │
│                                         │
│  ┌─ CTA ─────────────────────────────┐  │
│  │  [Book a Call] button             │  │
│  └───────────────────────────────────┘  │
│                                         │
│  [data-gl-place] (if keeping 3D mic)    │
└─────────────────────────────────────────┘
```

### Props interface changes
```ts
// Current
interface Props {
  title: string;
  text: string;
  cta?: Link;
}

// New
interface Benefit {
  icon: string;      // SVG icon name or inline SVG
  title: string;
  text: string;
}

interface Props {
  title: string;
  text: string;
  benefits: Benefit[];
  cta?: Link;
}
```

### Template changes (`.astro` file)
1. Keep existing `<h2>` and `<p>` for title and description
2. Add a new `.benefits` grid container after the `<p>`
3. Map over `benefits` array to render cards:
   ```astro
   <div class="benefits">
     {benefits.map((benefit) => (
       <div class="benefit-card" data-sy-reveal="lines">
         <div class="benefit-icon">{/* SVG icon */}</div>
         <h3 class="u-heading benefit-title">{benefit.title}</h3>
         <p class="u-text benefit-text">{benefit.text}</p>
       </div>
     ))}
   </div>
   ```
4. Keep CTA button after the grid

### CSS changes
Add new styles for the benefits grid:
```css
.benefits {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  margin-top: 3rem;
  width: 100%;
  max-width: 60rem;

  @media (--mq-tablet) {
    grid-template-columns: repeat(2, 1fr);
  }

  @media (--mq-phone) {
    grid-template-columns: 1fr;
  }
}

.benefit-card {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 1.5rem;
  border-radius: 1rem;
  background: rgba(var(--rgb-white), 0.5);
  backdrop-filter: blur(4px);
  border: 1px solid rgba(var(--rgb-brand-1), 0.1);
  transition: transform 0.3s var(--ease-out-cubic),
              box-shadow 0.3s var(--ease-out-cubic);

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  }
}

.benefit-icon {
  width: 2.5rem;
  height: 2.5rem;
  margin-bottom: 1rem;
  color: var(--color-brand-1);
}

.benefit-title {
  --fs: 1.1rem;
  --fw: 600;
  margin-bottom: 0.5rem;
}

.benefit-text {
  --fs: var(--font-size-sm);
  color: var(--color-text-alt);
  line-height: 1.5;
}
```

### Props to pass (from `website-content/why-poddero.md`)
```js
<Introducing
  title="Why Choose Poddero"
  text="We're more than a service provider—we're your podcast growth partner. Our streamlined process, attention to detail, and growth-focused approach help creators produce better content, save valuable time, and reach larger audiences."
  benefits={[
    { icon: "shield", title: "Consistent Quality", text: "Every episode is professionally refined to ensure clear audio, polished delivery, and a premium listener experience." },
    { icon: "clock", title: "Save Hours Every Week", text: "Focus on recording and creating while we handle editing, publishing, distribution, and promotional tasks." },
    { icon: "rocket", title: "Growth-Focused Strategy", text: "We don't just publish episodes—we help maximize their reach through distribution and content marketing." },
    { icon: "layers", title: "All-in-One Solution", text: "Production, enhancement, repurposing, guest booking, and marketing—all managed under one roof." },
    { icon: "globe", title: "Wider Distribution", text: "Get your podcast delivered across major listening platforms to reach audiences wherever they listen." },
    { icon: "users", title: "Dedicated Support", text: "Work with a team that understands podcasting and provides reliable communication throughout the process." },
  ]}
  cta={{
    title: "Book a Call",
    url: "#contact",
  }}
/>
```

### Files to modify
- **Modify:** `src/components/Introducing.astro` — update props interface, template, and CSS
- **Modify:** `src/pages/index.astro` — update props passed to `<Introducing>`

---

## 7. How It Works (`src/components/StepsTimeline.astro`)

### Current Design
- Scroll-pinned timeline with 3 numbered steps (01, 02, 03)
- Vertical line on the left with step numbers
- Steps activate as you scroll through them
- Title above the timeline
- 2 WebGL GL placeholders

### New Design
- **Same component structure** — the scroll-pinned timeline works well
- **Content update** — now 4 steps instead of 3
- **Minor CSS tweak** — increase scroll duration for 4 items

### ScrollTrigger adjustment
The current `end` value is `window.innerHeight * 2` for 3 items. For 4 items, increase to `window.innerHeight * 3`:
```js
// Current (line 60)
end: () => `+=${window.innerHeight * 2}px`,

// New
end: () => `+=${window.innerHeight * 3}px`,
```

### Props to pass (from `website-content/how-it-works.md`)
```js
<StepsTimeline
  title="How it works"
  items={[
    {
      title: "Share Your Episode",
      text: "Send us your raw recordings, notes, and requirements through our simple submission process.",
    },
    {
      title: "We Produce & Optimize",
      text: "Our team edits, enhances audio, prepares content assets, and gets everything ready for publishing.",
    },
    {
      title: "Publish & Distribute",
      text: "Your episode is published and distributed across major podcast platforms for maximum reach.",
    },
    {
      title: "Promote & Grow",
      text: "We repurpose content, support marketing efforts, and help your podcast attract more listeners over time.",
    },
  ]}
/>
```

### Files to modify
- **Modify:** `src/components/StepsTimeline.astro` — update ScrollTrigger `end` value for 4 items
- **Modify:** `src/pages/index.astro` — update props with 4 steps

---

## 8. Results / Case Studies — TO BE BUILT (`src/components/CaseStudies.astro`)

### Current State
Does not exist. No content file provided yet.

### Design Plan
Create a new component. Options:

**Option A: Card Grid**
```
┌─────────────────────────────────────────┐
│  "Results That Speak for Themselves"    │
│  subheadline text                       │
│                                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐│
│  │ Case 1   │ │ Case 2   │ │ Case 3   ││
│  │ [image]  │ │ [image]  │ │ [image]  ││
│  │ Client   │ │ Client   │ │ Client   ││
│  │ Metric   │ │ Metric   │ │ Metric   ││
│  │ Desc     │ │ Desc     │ │ Desc     ││
│  └──────────┘ └──────────┘ └──────────┘│
└─────────────────────────────────────────┘
```

**Option B: Carousel** (reuse `Gallery` component pattern with GSAP Draggable)

### Props interface
```ts
interface CaseStudy {
  image?: Image;
  client: string;
  metric: string;      // e.g., "300% download growth"
  description: string;
}

interface Props {
  title: string;
  subtitle: string;
  studies: CaseStudy[];
}
```

### Content needed
Prepare content in `website-content/case-studies.md` with 2-3 case studies including client names, metrics, and descriptions.

---

## 9. Testimonials — TO BE BUILT (`src/components/Testimonials.astro`)

### Current State
Does not exist. No content file provided yet.

### Design Plan
Create a new component. Options:

**Option A: Static Grid**
```
┌─────────────────────────────────────────┐
│  "What Our Clients Say"                 │
│                                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐│
│  │ "Quote"  │ │ "Quote"  │ │ "Quote"  ││
│  │ Name     │ │ Name     │ │ Name     ││
│  │ Role     │ │ Role     │ │ Role     ││
│  └──────────┘ └──────────┘ └──────────┘│
└─────────────────────────────────────────┘
```

**Option B: Carousel** (reuse `Gallery` pattern)

### Props interface
```ts
interface Testimonial {
  quote: string;
  name: string;
  role: string;
  avatar?: Image;
}

interface Props {
  title: string;
  testimonials: Testimonial[];
}
```

### Content needed
Prepare content in `website-content/testimonials.md` with 3-4 client testimonials.

---

## 10. FAQ (`src/components/Faqs.astro`)

### Current Design
- Accordion FAQ with GSAP height animation
- 5 questions
- Title, subtitle, mobile title props
- Questions have a + icon that rotates to x when open
- Border between items

### New Design
- **Same component structure** — accordion works perfectly
- **Content update** — now 8 questions instead of 5
- **No code changes needed** — component handles any number of items

### Props to pass (from `website-content/faqs.md`)
```js
<Faqs
  title="Frequently Asked Questions"
  mobileTitle="FAQs"
  subtitle="Quick answers to the most common questions."
  items={[
    { question: "What podcast services do you provide?", answer: "We offer podcast editing, audio enhancement, content repurposing, guest booking, distribution, and marketing support." },
    { question: "Which podcast platforms do you support?", answer: "We help distribute podcasts to major platforms including Spotify, Apple Podcasts, YouTube, Amazon Music, and more." },
    { question: "How long does it take to deliver an episode?", answer: "Turnaround times vary by project scope, but most episodes are delivered within a few business days." },
    { question: "Do I keep full ownership of my content?", answer: "Yes. You retain complete ownership of your podcast, recordings, and all published content." },
    { question: "Can you work with an existing podcast?", answer: "Absolutely. Whether you're launching a new show or improving an established one, our services can be tailored to your needs." },
    { question: "What do I need to provide?", answer: "Simply send us your recordings and any relevant notes. We'll handle the technical production and optimization process." },
    { question: "Do you offer custom packages?", answer: "Yes. We create customized solutions based on your podcast's goals, publishing frequency, and service requirements." },
    { question: "How do I get started?", answer: "Book a consultation with our team, and we'll discuss your podcast, goals, and the best plan for your growth." },
  ]}
/>
```

### Files to modify
- **Modify:** `src/pages/index.astro` — update props with 8 FAQ items

---

## 11. Contact / Book Consultation — NEW (`src/components/Contact.astro`)

### Current State
Does not exist.

### Design Plan
Create a new component with a GoHighLevel form embed.

**Structure:**
```
┌─────────────────────────────────────────┐
│  "Let's Take Your Podcast to the        │
│   Next Level"                           │
│  Description paragraph                  │
│                                         │
│  ┌─ Form Embed ──────────────────────┐  │
│  │  [GoHighLevel iframe/form]        │  │
│  └───────────────────────────────────┘  │
│                                         │
│  [data-gl-place] (if keeping 3D mic)    │
└─────────────────────────────────────────┘
```

### Props interface
```ts
interface Props {
  title: string;
  text: string;
  formEmbed: string;  // GoHighLevel embed code or iframe URL
}
```

### Props to pass (from `website-content/contact.md`)
```js
<Contact
  title="Let's Take Your Podcast to the Next Level"
  text="Tell us about your podcast and goals. We'll create a tailored strategy to help you grow faster and produce better content."
  formEmbed="<!-- GoHighLevel embed code -->"
/>
```

### Files to create
- **Create:** `src/components/Contact.astro`
- **Modify:** `src/pages/index.astro` — import and add `<Contact>` before `<Footer>`

---

## 12. Footer (`src/components/Footer.astro`)

### Current Design
- **Fixed position** (`position: fixed; bottom: 0`) — always visible at bottom
- Logo + nav links + copyright in a row
- Credit link "Created by Sijad"
- Resize observer toggles `has-no-anim` class if footer is too tall

### New Design
- Change from `position: fixed` to `position: static` — standard scrolling footer
- Update branding to Poddero
- Remove credit link
- Optionally add social media links

### CSS changes
```css
/* Current */
footer {
  position: fixed;
  bottom: 0;
  width: 100%;
}

/* New */
footer {
  position: static;
  width: 100%;
}
```

### Template changes
- Remove the credit div (lines 36-39):
  ```astro
  <!-- Remove this -->
  <div class="credit">
    <a href="https://github.com/sijad/viio-astro">Created by Sijad</a>
  </div>
  ```
- Optionally add a social links section

### Script changes
- Remove or simplify the `Footer` class resize observer logic since the footer is no longer fixed-position and doesn't need height calculation

### Files to modify
- **Modify:** `src/components/Footer.astro` — CSS positioning, remove credit, update branding

---

## 13. 3D Model Change (Optional)

### Option A: Replace sphere with podcast mic
- Replace procedural `IcosahedronGeometry` in `GL.astro` with `GLTFLoader` loading a `.glb` model
- Add `AmbientLight` + `DirectionalLight` for model lighting
- Remove custom glass shader code (vertex/fragment shaders)
- Place `.glb` file in `public/models/microphone.glb`
- Keep scroll-based positioning system (`[data-gl-place]` waypoints)

### Option B: Remove 3D entirely
- Remove `GL.astro` and `GlPlaces.astro`
- Remove all `[data-gl-place]` divs from components
- Better performance, cleaner corporate feel

---

## 14. Sections to Remove from Current Page

| Component | Current Purpose | Action |
|-----------|-----------------|--------|
| `GalleryText.astro` | Product feature showcase | **Remove** — replaced by Services |
| `Statements.astro` | Full-screen stacked panels | **Remove** — replaced by Why Poddero cards |
| `Plans.astro` | SaaS pricing tiers | **Remove** — agency uses custom quotes |
| `TextMarquee.astro` | Scrolling tagline | **Remove or keep** — can repurpose for trust signal |
| `ImageMarquee.astro` | Logo ticker | **Keep if** you have client/partner logos |
| `GL.astro` + `GlPlaces.astro` | 3D sphere | **Replace** with mic model or **Remove** |

---

## 15. Page Structure (`src/pages/index.astro`)

### New section order
1. **Hero** — `HeroHome` (content updated)
2. **Services** — `Services` (NEW)
3. **Why Poddero** — `Introducing` (major tweak with benefits grid)
4. **How It Works** — `StepsTimeline` (4 steps, scroll duration updated)
5. **Results / Case Studies** — `CaseStudies` (NEW — content pending)
6. **Testimonials** — `Testimonials` (NEW — content pending)
7. **FAQ** — `Faqs` (8 items, content updated)
8. **Contact** — `Contact` (NEW — GoHighLevel embed)
9. **Footer** — `Footer` (repositioned + rebranded)

### Imports to add
```js
import Services from "../components/Services.astro";
import CaseStudies from "../components/CaseStudies.astro";
import Testimonials from "../components/Testimonials.astro";
import Contact from "../components/Contact.astro";
```

### Imports to remove
```js
import GalleryText from "../components/GalleryText.astro";
import Statements from "../components/Statements.astro";
import Plans from "../components/Plans.astro";
import TextMarquee from "../components/TextMarquee.astro";
```

---

## 16. Files Summary

### Files to modify
| File | Changes |
|------|---------|
| `src/pages/index.astro` | Rebuild page structure, update all imports and props |
| `src/layouts/Layout.astro` | Rebrand, update nav links, update header/footer props |
| `src/components/Introducing.astro` | Add benefits grid (new props, template, CSS) |
| `src/components/StepsTimeline.astro` | Update ScrollTrigger end duration for 4 items |
| `src/components/Footer.astro` | Fix positioning, remove credit, rebrand |
| `src/components/GL.astro` | Replace sphere with mic model (if keeping 3D) |
| `src/styles/global.css` | Update color palette variables |

### Files to create
| File | Purpose |
|------|---------|
| `src/components/Services.astro` | Services section with 4 service cards + gradient background |
| `src/components/CaseStudies.astro` | Results/case studies section (content pending) |
| `src/components/Testimonials.astro` | Client testimonials section (content pending) |
| `src/components/Contact.astro` | Contact form with GoHighLevel embed |
| `public/models/microphone.glb` | 3D mic model (if keeping 3D) |

### Files to delete
| File | Reason |
|------|--------|
| `src/components/Plans.astro` | SaaS pricing not needed for agency |
| `src/components/GalleryText.astro` | Product showcase replaced by Services |
| `src/components/Statements.astro` | Stacked panels replaced by Why Poddero cards |
| `src/components/Webgl.astro` | Empty unused file |

### Files to keep (unchanged)
| File | Reason |
|------|--------|
| `src/components/Button.astro` | Used throughout for CTAs |
| `src/components/Gallery.astro` | May reuse for Case Studies or Testimonials carousel |
| `src/components/ImageMarquee.astro` | Keep if using client/partner logo ticker |
| `src/components/GradientBackground.astro` | Reuse for Services section |
| `src/components/SmoothScroll.astro` | Lenis smooth scroll — keep |
| `src/components/RectReveal.astro` | May reuse for other sections |
| `src/components/GlPlaces.astro` | Keep if keeping 3D model |

---

## 17. Content Checklist

- [x] Hero content (`website-content/hero.md`)
- [x] Services content (`website-content/services.md`)
- [x] Why Poddero content (`website-content/why-poddero.md`)
- [x] How It Works content (`website-content/how-it-works.md`)
- [x] FAQ content (`website-content/faqs.md`)
- [x] Contact content (`website-content/contact.md`)
- [ ] Case Studies content (`website-content/case-studies.md`) — **needed**
- [ ] Testimonials content (`website-content/testimonials.md`) — **needed**
- [ ] Poddero logo (SVG)
- [ ] Poddero brand colors (RGB values)
- [ ] 3D microphone `.glb` model (if keeping 3D)
- [ ] Client/partner logos (if keeping logo ticker)
- [ ] GoHighLevel form embed code
