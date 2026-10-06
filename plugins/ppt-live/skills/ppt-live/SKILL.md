---
name: ppt-live
description: Build an interactive ppt.live show — live polls, quizzes, word clouds, Q&A, ratings, games — from a topic, notes, or the user's own deck (PDF, PowerPoint, Canva, Keynote, Google Slides), and hand back the links to present it and for the room to join. Use when someone wants a presentation, talk, pitch, workshop, all-hands, training, class, wedding, party or game night the audience can play along with on their phones, asks to "make my deck interactive / live", or mentions ppt.live. Also use after a show to read and summarise what the room said.
---

# ppt.live — turn any talk into a live show

ppt.live puts a show on the big screen and makes every phone in the room a remote: people scan a QR
code (or go to ppt.live/CODE), no app or sign-up, and vote, answer, ask and play. The big screen
answers back live. You build the show with the **ppt.live connector** (MCP tools below).

## 0 · Make sure the connector is there
The tools are `list_slide_types`, `list_templates`, `create_show`, `list_shows`, `get_show`,
`update_show_slides`, `add_picture`, `get_results`. If you don't have them, tell the user how to connect
(one time, then they sign in with ppt.live and press Allow):
- **claude.ai** — Settings → Connectors → *Add custom connector* → `https://www.ppt.live/mcp`
- **Claude Code** — `claude mcp add --transport http ppt https://www.ppt.live/mcp`, then `/mcp` to sign in

Call `list_slide_types` once per conversation: it's the source of truth for every slide's fields.

## 1 · From a topic or notes
Plan first, then one `create_show` call.
- **Open warm**: a `story` title slide, then a `poll` pulse check (or `guests` — everyone's face).
- **Rhythm**: an interactive slide every 2–3 talk slides. Talk = `story` (a headline + a few short lines).
- **One idea per talk slide**; lines ≤ 12 words; `layout:"big"` + `stat` for one number.
- **Energy**: 2–4 `quiz` (or `ff`, fastest fingers) slides, then a `leaderboard`.
- **Honest input**: `wordmap` ("one word for…"), `qa` with `anon:true`.
- **Close**: `qa`, then a `scale` ("How useful was this?").
- **Parties**: `jukebox`, `drumroll` for a reveal, `react` for applause, `game` for party games.
- Put what the presenter should *say* in `notes` (only they see it).
- 8–20 slides. Theme: `clean` (work), `elegant` (weddings, boards), `dark` (launches), `comic` (fun).

## 2 · From the user's own deck — keep their slides, add the live moments
When the user gives you a deck (PDF, .pptx, a Canva/Keynote/Google Slides export) and wants it live,
**don't retype their slides**. Their design is the point. Build the show *around their real pages*:
1. Read the deck. Note what each page says and where the natural breaks are.
2. Plan: every page as a placeholder **in order** — `{ "type":"media", "page": N }` — with live slides
   between them, written **from the deck's own facts**:
   - after the opening / problem → a `poll` that gets the room's own experience
   - after a big number → a `quiz` on that number (the right answer is in the deck)
   - after a "how it works" or product page → a `scale` ("would you use this?")
   - after a list of options / roadmap → a `poll` ("what should we do next?")
   - before the team / ask → a `leaderboard` (if you had quizzes)
   - before the last page → `qa`
   Aim for one live moment every 3–4 pages. Keep pitch decks crisp: 6–8 live slides for ~25 pages.
3. `create_show` with that list. The reply includes a **pages** link.
4. Tell the user: *"Open this link and drop the same deck in as a PDF — every page slots into place,
   looking exactly like your original."* (From PowerPoint: File → Export → PDF. Canva: Share →
   Download → PDF. Keynote: File → Export To → PDF. Google Slides: File → Download → PDF.)

If you can produce the page images yourself (e.g. Claude Code rendering the PDF locally), skip the drop:
`add_picture` each page (JPEG/PNG under ~1 MB, 16:9 ≈ 1600px wide), then `update_show_slides` with
`{ "type":"media", "url": <returned url>, "fit":"contain" }` in place of the placeholders.

## 3 · Hand it over
Always give the user:
- **present** — open on the big screen in their signed-in browser, press F for full screen
- **join** — `ppt.live/CODE`; the QR is on screen too, so most people just scan
- **edit** — to tweak anything
Mention the **pptx** link only if they must present from PowerPoint (live slides then carry a QR and a
"open this slide live" link; the full experience is presenting from ppt.live).

## 4 · Changing it
`get_show` first (it has the slide ids), then `update_show_slides` — `mode:"replace"` with the full
list (keep ids to keep votes), or `mode:"append"`. It works while the show is live.

## 5 · After the show
`get_results` gives every poll, quiz (right answer marked), word cloud, Q&A by upvotes and rating.
Summarise what the room said in plain words: the headline, surprises, the top questions, and what to
follow up on. Offer to turn it into an email or a follow-up show.

## Without the connector (scripts)
`POST https://www.ppt.live/api/decks` with `Authorization: Bearer <token>` (dashboard → Build shows
with AI → Create an API token) and `{ "name", "theme", "slides":[…] }`; slide types at
`GET https://www.ppt.live/api/slide-types`. Never print a token back to the user.
