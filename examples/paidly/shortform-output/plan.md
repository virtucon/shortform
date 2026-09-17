# Shortform plan: Paidly

- Objective: "attract freelancers as customers" → archetype `attract-customers` (the viewer should want the product; hook is their pain, stated bluntly)
- Audience: freelancers and tiny studios who invoice clients and hate chasing late payments
- Platform: tiktok (built to the universal safe zone, x 60–920 / y 250–1440, so it also works on Shorts and Reels)
- Length: 25s
- Tone: blunt, warm, relieved
- Palette / fonts: ink `#14213d`, paper `#fffaf0`, mint `#2ec4a0`, coral `#ff6b5e`, sun `#ffd166`; radius 20px, 2px ink borders with hard offset shadows; Space Grotesk 500–700 (the font tops out at 700)
- Music: `upbeat.mp3` (Funky House, CC0) at volume 0.4
- CTA: "Send your first invoice free — link in bio", with `paidly.example` and the paidly. wordmark on screen
- Assumptions: brief said "just do it, no questions", so the approval gate is skipped and the top-ranked hook that fits the type rules is taken (hook 2). No voiceover. No user media; the project has no logo or image files, so the wordmark is rebuilt as text. Tone and music inferred.

## Hooks
1. "Still writing “just following up”?" — Pain — the landing page's own phrase; strongest line, but it cannot be set at 110px+ in three lines without stranding `up”?` or `“just` on its own line, so by the type rule it is passed over.
2. "Freelancers: stop chasing invoices." — Call-out — names the audience, breaks cleanly as three lines at 140px. **Chosen.** The "just following up" phrase survives as the email subjects stacked under the hook.
3. "Invoices that chase themselves." — Reveal — the real headline; used as the reveal line in scene 3.

## Storyboard

| # | Time | Dur | Beat | On-screen text | Visual | Motion / transition | Cut on |
|---|------|-----|------|----------------|--------|---------------------|--------|
| 1 | 0.0 | 2.5 | Hook | Freelancers: / stop chasing / invoices. | Ink background. Under the hook, an inbox pile: "Just following up…", "Re: just following up…", "Re: Re: just following up…" | Hook on screen at frame 1, mid-slam (scale 1.05→1); rows stack by 0.8s; "chasing" flips coral to sun at 1.4s | planned |
| 2 | 2.5 | 3.29 | Agitate | $2,400. / 14 days late. → And you’re the / one chasing. | The real invoice card: "Invoice #0042", "14 days late" pill, "Brand refresh — Acme Bakery", "$2,400.00" | Card pushes up, pill pulses; caption swaps at 4.3s and card tilts | planned |
| 3 | 5.79 | 3.21 | Reveal | For freelancers and tiny studios / paidly. / Invoices that chase themselves. | Paper panel pushes up over the ink; 280px wordmark, then the real headline with "themselves." on mint | Push-up transition, wordmark scales in, headline rises line by line | strong beat 5.79 |
| 4 | 9.0 | 4.0 | Proof 1 | 30-second invoices → Pick a client. / Type an amount. / Hit send. | Rebuilt send form: Client "Acme Bakery", Amount "$2,400.00", ink "Send" button turning into mint "Sent ✓" | Fields fill with each caption (9.0, 10.37, 11.5); button press at 11.7s | caption 2 on strong beat 10.37 |
| 5 | 13.0 | 4.0 | Proof 2 | Auto-reminders → Friendly on day 1. / Firm on day 7. | The real timeline: "Sent", "Reminder 1 — friendly" (day 1), "Reminder 2 — firm" (day 7) | Rows check in on a stagger, day badges pop | planned |
| 6 | 17.0 | 4.0 | Proof 3 | One-tap pay → Card or bank transfer. / No login. | Invoice #0042, $2,400.00, "Pay by card" and "Pay by bank transfer"; "Pay by card" is tapped and the "Paid" row fills mint | Button press at 18.3s, Paid fills at 19s | planned |
| 7 | 21.0 | 4.0 | CTA | paidly. / Send your first invoice free / Link in bio / paidly.example / Free for your first 5 invoices a month | Paper panel pushes up and away, back to the hook's ink background | Push-up in, settled by 22.3s, no exit: stays readable through the last frame | planned |

Durations: 2.5 + 3.29 + 3.21 + 4 + 4 + 4 + 4 = 25.0s.

Beat snap: the grid was reduced to strength ≥ 0.9 and thinned to one per ≥1s, which left 11 beats, none after 16.1s. Only the reveal (6.0 → 5.79) and the "Type an amount." caption (10.3 → 10.37) were within 0.3s; every other cut keeps its planned time.

Layout: every scene's content block is centred on the safe rectangle (x 60–920, y 250–1440) and spans 840–1110px of its 1190px height.

Word count on screen (captions): about 50 words, under the 60-word ceiling.

## Claims check

| On screen | Source |
|---|---|
| Freelancers: stop chasing invoices. | index.html eyebrow "For freelancers and tiny studios"; features "Paidly does the chasing" |
| “just following up” (email subjects) | index.html hero sub: never have to write "just following up" again |
| Invoice #0042, 14 days late, Brand refresh — Acme Bakery, $2,400.00 | index.html example invoice card |
| Invoices that chase themselves. | index.html h1 |
| 30-second invoices; Pick a client, type an amount, hit send | index.html features |
| Auto-reminders; Friendly on day 1. Firm on day 7. | index.html features, PRODUCT.md |
| Sent / Reminder 1 — friendly / Reminder 2 — firm / Paid | index.html timeline |
| One-tap pay; card or bank transfer; No login | index.html features |
| Send your first invoice free | index.html primary button |
| Free for your first 5 invoices a month; paidly.example | index.html footer |

Not claimed (no source support): customer counts, "paid X days faster", testimonials, ratings, integrations, pricing beyond the free tier.
