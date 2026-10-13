---
name: creator-page
description: "Build a one-page creator site (a link-in-bio page for brands) from a creator's public Instagram, Linktree and YouTube. Use when the user asks for a creator page, influencer page, media kit page, Linktree replacement, bio link page, or 'make one for' a named creator from their Instagram. Also fires on: micro influencer, UGC creator, PR packages, brand deals page, link in bio."
license: MIT
requires: []
---

# Creator page

A single scrolling page a micro-creator puts in their bio so brands see why to
work with them. Phone first: almost everyone opens it from Instagram.

## Getting the real data (logged out)

- Profile counts and bio: the profile page text.
- Per-post likes, comments, reel view counts and full-size photos, including
  every carousel child: fetch `/p/<shortcode>/embed/captioned/` from an
  instagram.com tab and parse the `contextJSON` string. `video_view_count` is
  there; `video_url` is not.
- Reel video files: `yt-dlp` in a scratch venv works logged out.
- `/api/v1/users/web_profile_info` refuses logged out ("require_login").
- Brands: the creator's Linktree (discount codes and product links) are the
  only real partner evidence. Never invent brands.
- Check TikTok by handle and by `tiktok.com/search/user?q=`. If nothing
  matches, leave a marked placeholder and say BLOCKED.
- Email is usually not public. Leave a marked placeholder, never a made-up one.

## What the user wants (2026-10-13 feedback, his words)

- Favorite posts as full-size 9:16 cards in a sideways swipe carousel you can
  watch, with views, likes and comments under each. "don't consolidate them",
  "make them full size so you could watch them".
- Immersive, not a template: motion as you scroll, something living in the
  background, photos that pop in. The flat dark-card version read as "Etsy",
  "Wix", "a word PowerPoint that you threw together".
- Every section has to help a brand say yes. A cute section about the
  creator's signature pose "wouldn't really want to like help any brand work
  with him". Cut it.
- Sections he liked: "What I bring to the table" in the creator's voice,
  "Brands I've worked with" (not "Already rocking with"), "Small page, loud
  room." with "Big community." under it, and the closing "Let's go somewhere
  together" pitch.
- Numbers: followers, average likes per post, average views per reel ("the
  reels is where it's really going to be interesting"). Never show engagement
  rate or following count.
- A hero idea he asked for: the name rising from behind the mountain. A sky
  flood-fill mask (blue pixels connected to the top row) on the hero photo
  makes a clean foreground cutout to layer over the name.

## Second round (2026-10-13, his words)

- Scroll has to feel instant. Pinned sections that need "like four times"
  or "six times" of scrolling before anything moves read as lag. Play the
  intro on load (name rises in the first seconds, then "welcome", then a
  "swipe down for more" cue) and tie reveals to normal scroll position.
- Don't call a creator's vehicle or gear old without checking. Colby's
  4Runner is brand new and he is building it out.
- Interests as plain pill bubbles "looks kind of simple". Use photo tiles.
- A scenic clip from their own posts (a sunset, not a talking reel) works
  as a section background. Carousel children can hold videos: `yt-dlp` on
  the `/p/` link downloads the ones it can.
- Give the gym its own section when the creator trains.

## Never

- Never publish a page that uses a real person's name and photos without the
  user saying so. Build it as a file first.
- Never fill a stat, brand or contact with a guess. Mark it.
