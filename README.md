# ppt.live for Claude

Turn any talk, pitch, class or party into a **live show**: the room scans a QR code and votes, answers,
asks and plays on their phones while the big screen answers back. Claude builds it for you on
[ppt.live](https://www.ppt.live), from a topic, your notes, or **your own deck** (PowerPoint, Canva,
Keynote, Google Slides or PDF).

This repo is the **ppt.live plugin**: the ppt.live connector (MCP, sign in with ppt.live) plus the
`ppt-live` skill that teaches Claude to make great shows.

## Claude Code

```bash
claude plugin marketplace add sameeeeeeep/ppt-live-claude
claude plugin install ppt-live@ppt-live
```

Then type `/mcp`, pick **ppt**, and choose **Authenticate**. Your browser opens ppt.live: sign in (or
make a free account) and press **Allow**. That's it.

## claude.ai (web, desktop, mobile)

1. **Connector:** Settings → Connectors → *Add custom connector* → `https://www.ppt.live/mcp` → Connect,
   then sign in with ppt.live and press **Allow**.
2. **Skill (recommended):** download [`ppt-live-skill.zip`](ppt-live-skill.zip) and upload it in
   Settings → Capabilities → Skills.

## Try it

> Make a 20-minute ppt.live show for Monday's all-hands: a mood poll, a 5-question quiz on our Q3
> numbers, then open Q&A.

> *(attach your deck)* Make this live: add polls and a quiz from my own content.

For your own deck, Claude places your pages and adds the live moments. Then you drop the same file in as a
PDF once, and every page slots in looking exactly like the original. Claude sends back the link to
present and the code your audience joins with (`ppt.live/ABC123`); after the show, ask it for the results.

Disconnect any time from your ppt.live dashboard → Connected apps.
