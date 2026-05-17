# Project Prowess — Chained Recruitment Puzzle (v0.1)

A Cicada-3301-inspired chained cryptographic puzzle designed to find people Elijah would want to work with. v0.1 ships three real layers behind the entry page, each cryptographically gating the next.

**Live:** https://thebigjah.github.io/project-prowess/

## Architecture

Every layer's solution IS the URL of the next layer. You cannot skip ahead because the URLs are derived from hash functions you don't have an input for until you solve the prior layer.

```
/                                       Entry. Hidden base64 in HTML source.
/cbbf2643559af0f4/                      Layer 1. Vigenère with acrostic key.
/2ce8286c706bacb1/                      Layer 2. Code comprehension + sha256 brute force.
/6e8ef5c841cdc748/                      Final. Personal contact + filter question.
```

## How each layer gates the next

- **Layer 0 → 1:** Layer 0 hides `L2NiYmYyNjQzNTU5YWYwZjQv` (base64 of `/cbbf2643559af0f4/`) in an HTML comment. Solver views source.
- **Layer 1 → 2:** Layer 1 shows a poem with first-letter acrostic spelling `FORGE` (the Vigenère key) plus a ciphertext `UFFIIJR KU TWCNKWX GCGWM 2QV8286I706FFQS1`. Decrypting yields `PROCEED TO PROWESS SLASH 2CE8286C706BACB1`, which is the Layer 2 path.
- **Layer 2 → final:** Layer 2 shows a JavaScript function `f(n) = sha256(sha256(sha256(str(n)+"prowess").slice(0,16)+"prowess").slice(0,16)+"prowess").slice(0,16)` and target `0285543017b99610`. Solver finds N=9024 by brute force, then computes `sha256("9024finish").slice(0,16)` = `6e8ef5c841cdc748` for the final path.
- **Final → log:** Final page is a form that POSTs each submission as an ntfy push to topic `prowess-625370c90178bbc65dc8ba6f`. Elijah subscribes to that topic in his ntfy app and gets every submission as a persistent notification. Solver also keeps a local copy in their browser's localStorage with a receipt hash. No email-sending step; submissions auto-log.

**Important:** Elijah must subscribe to `ntfy.sh/prowess-625370c90178bbc65dc8ba6f` in his ntfy app (one-tap) before launch, or submissions silently expire from ntfy's 12-hr default retention. Once subscribed, the ntfy app keeps history on his device permanently.

## How to add more layers

1. Decide what skill the new layer tests (modern crypto, LLM prompt engineering, multimodal embeddings, OSINT, distributed tech, etc.)
2. Pick a seed string. Compute the new layer's path as `sha256(seed)[:16]`.
3. Build the gating mechanism: the previous layer's solution should produce the new layer's path through a deterministic function the solver can verify themselves.
4. Write the HTML page in a new folder named with the hash.
5. Update the prior layer to point at it.
6. Test the full chain end to end.

## Aesthetic rules

- Monochrome only. Off-white ink on near-black background. Single warm accent (oxide gold).
- Monospace everywhere.
- No navigation. No "next" buttons. No breadcrumbs. No help.
- No exclamation marks. No emoji. No friendly UX copy.
- Each page identifies itself only by Roman numerals (i, ii, iii).
- Signed only `— ebp · 2026`.
- robots noindex, nofollow on every layer.

## What the spec says about v2 (future)

See `~/.claude/elijahbot/drafts/project-prowess-research/v2-spec.md` for the comprehensive v2 design — 9 chained layers covering steg, classical crypto, code, prompt engineering, embeddings, RSA, distributed tech, OSINT, meta-comprehension. v0.1 ships layers 0-2 to validate the chain mechanic; v2 expands to the full filter.

## Source provenance

This is the post-revamp incarnation of "Project Prowess" that Elijah worked on across ChatGPT Marks III/IV/V (Feb 2025 - May 2025), originally evolving into "Cuttlefish 0167." The v2 spec at `drafts/project-prowess-research/v2-spec.md` includes a synthesis of that evolution. Returning to "Project Prowess" name per Elijah's 2026-05-17 directive.
