---
name: amethy-universe
description: >
  Generate HTML, React, or CSS artifacts in Amethy's personal design system — a family of themed styles
  ranging from dark cosmic to cute 3D to cow-print. ALWAYS use this skill when Amethy asks to build any
  visual artifact, UI, app, dashboard, game, or interactive component. Trigger on: "my style", "like before",
  "build me a", "make it cute", "make it dark", "moo style", "cow theme", "cosmic", "zodiac", "same vibe",
  or any request for a UI component or page — even if no explicit style is mentioned. When in doubt, use this skill.
---

# Amethy Universe — Design System

Amethy has 4 named style presets. Every artifact must use exactly one. When no theme is specified, **ask** — or
default to `cosmic-dark` for data-heavy apps and `cute-3d` for games/trackers.

---

## Step 1 — Pick the Theme

| Theme | When to use | Vibe |
|-------|------------|------|
| `cosmic-dark` | Dashboards, data viz, serious tools | Deep space, neon glows, mystical |
| `cosmic-light` | Documents, portfolios, softer tools | Pastel nebula, frosted glass |
| `cute-3d` | Games, trackers, habit apps, fun tools | Bouncy, springy, emoji explosions |
| `moo` | Anything playful or when Amethy says "cow" | Black/white/green, cow faces, grass |

After picking, load the matching reference file:
- `cosmic-dark` → read `references/cosmic-dark.md`
- `cosmic-light` → read `references/cosmic-light.md`
- `cute-3d` → read `references/cute-3d.md`
- `moo` → read `references/moo.md`

---

## Step 2 — Interaction Patterns

Every theme supports the same 5 gamification patterns. Add whichever fit the app.

### Pattern 1 · XP Bar
- Animated fill with springy `cubic-bezier(0.34,1.56,0.64,1)` transition
- Floating `+XP` burst on earn (fixed-position div, auto-removed after 1.2s)
- Level-up flash: card border/shadow pulses brightly for 700ms
- Level names must match the theme (cosmic: Apprentice→Oracle; moo: Calf→Cow Queen)
- A mascot floats above the bar (cosmic: planet with eyes; moo: cow planet; cute: chibi orb)

### Pattern 2 · Streak Counter
- 7-day grid, completed days styled distinctly per theme
- Flame/mascot icon wobbles with CSS `rotate` keyframe
- Check-in button disables after tap with confirmation text
- Burst particles fire on successful check-in

### Pattern 3 · Oracle Spin Wheel
- Canvas-drawn wheel with 8 zodiac segments
- Spin deceleration: `ease = 1 - Math.pow(1-t, 4)` over ~2800ms
- Result fades in via `popIn` / `fadeReveal` animation
- Speech bubble or result panel — style per theme
- Wheel center has theme mascot icon

### Pattern 4 · Constellation / Zodiac Unlock Grid
- 4-col grid (2-col on mobile), aspect-ratio:1 nodes
- Locked nodes: `filter: grayscale(1) brightness(0.7) opacity(0.5)`
- Unlock animation: `zodiacPop` keyframe — scale from 0.4 + rotate in
- Particle burst on unlock: 12–20 particles, theme-appropriate emoji/colors
- Progress bar above with theme stripe pattern

### Pattern 5 · Resonance / Tap Orb
- Orb bobs continuously with `translateY` keyframe
- `:active` squishes: `scale(0.88) translateY(8px)`, pauses float animation
- Each tap spawns a `ripple-ring` div (absolute, border-radius:50%, animates scale+opacity out)
- Face changes expression as charge % increases (5 face stages)
- Full charge → burst particles + face changes → resets after 800ms

---

## Step 3 · Universal Rules (all themes)

**Animations:**
- Springy physics: `cubic-bezier(0.34, 1.56, 0.64, 1)` for pops/reveals
- Smooth ease-out: `cubic-bezier(0.16, 1, 0.3, 1)` for bars/slides
- Float loops: `ease-in-out infinite alternate`
- Blink: `scaleY(0.1)` at ~94% of a 3.5–4s loop

**3D card effect (cute-3d + moo):**
```css
.card {
  box-shadow: 0 8px 0 <shadow-color>, 0 12px 28px <ambient>;
  transition: transform 0.15s, box-shadow 0.15s;
}
.card:hover  { transform: translateY(-4px); box-shadow: 0 12px 0 <shadow-color>, ...; }
.card:active { transform: translateY(5px);  box-shadow: 0 3px  0 <shadow-color>, ...; }
```

**Particle burst helper (copy into every artifact):**
```js
function burst(x, y, items, n) {
  for (let i = 0; i < n; i++) {
    const p = document.createElement('div');
    p.style.cssText = `position:fixed;pointer-events:none;z-index:999;font-size:18px;
      left:${x}px;top:${y}px;
      --dx:${Math.cos((i/n)*2*Math.PI)*( 50+Math.random()*80)}px;
      --dy:${Math.sin((i/n)*2*Math.PI)*( 50+Math.random()*80)}px;
      --rot:${Math.random()*360}deg;
      animation:particleFly 0.9s ease-out ${Math.random()*0.12}s forwards`;
    p.textContent = items[Math.floor(Math.random() * items.length)];
    document.body.appendChild(p);
    setTimeout(() => p.remove(), 1100);
  }
}
// Required keyframe:
// @keyframes particleFly {
//   0%  { opacity:1; transform:translate(0,0) scale(1.2) rotate(0deg); }
//   100%{ opacity:0; transform:translate(var(--dx),var(--dy)) scale(0) rotate(var(--rot)); }
// }
```

**Typography:**
- Cute themes: `Fredoka One` (headings) + `Nunito` (body)
- Cosmic themes: `Orbitron` (labels/numbers) + `Cinzel` (titles) + `Noto Sans SC` (body/CJK)
- Always load from Google Fonts

**Never:**
- White body background on cosmic themes
- Light text on light background
- Generic shadows without a bottom-face offset (kills the 3D illusion)
- Gradients on text in Claude.ai artifacts (use `color` + `text-shadow` instead — `-webkit-text-fill-color:transparent` is filtered)

---

## Step 4 · Output

- Single `.html` file with all CSS and JS inline (no external files except Google Fonts CDN)
- For React: inject full CSS via a `<style>` tag inside the component — Tailwind alone cannot reproduce this system
- Test mentally: dark bg? ✓ hover glows? ✓ click feedback? ✓ at least one float animation? ✓

---

## Reference Files

Load the relevant one after choosing a theme:

| File | Contents |
|------|----------|
| `references/cosmic-dark.md` | Full CSS vars, star bg, neon glow patterns, component library |
| `references/cosmic-light.md` | Pastel nebula vars, frosted glass cards, soft shadows |
| `references/cute-3d.md` | Pastel vars, spring animations, chibi planet, full component CSS |
| `references/moo.md` | Cow-print vars, grass bg, cow planet, diagonal stripe patterns |
