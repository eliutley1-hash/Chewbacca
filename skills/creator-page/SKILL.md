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

## Third round (2026-10-13)

- Instagram carousel videos often download as VP9, which iPhones won't play.
  He saw the background video in Claude but not on his phone. Check the
  codec (`strings | grep vp09`) and take an H.264 format instead (`yt-dlp -F`
  lists them, often formats 0 to 2), and always give a background video a
  poster frame.
- Photos scattered in the corners around a statement "looks like a
  PowerPoint". Leave them out.
- A sideways strip tied to page scroll moved too fast to read. Use tabs
  that swap a swipeable gallery with arrows and a counter.
- Add a community section when the creator values it: friends become
  family, and they only promote things they and their friends would use.

## Fourth round (2026-10-14, his words)

- The pitch: "more in depth than a link tree". Keep the links and email, but
  let brands and followers get to know the person.
- The about-him topics were "too much to scroll through". One tabbed section
  (topic tabs, the topic's copy, then a swipe gallery with arrows) replaced six
  tall chapters and cut the phone page from 10,780px to about 6,400px.
- Numbers: "only put the good stuff". Followers, biggest reel views, total
  reel views, recent growth. No average likes, no fine print. The reels grid
  (`/reels/` tab, logged out) shows play counts on each tile; those are the
  public view numbers and run higher than `video_view_count` in the embed.
- Lead the favorite-posts strip with the biggest reels. Show views only, and
  only when they're 10K or more.
- Instagram's progressive formats (1 to 3) are H.264 but huge. No ffmpeg on
  this Mac: an AVAssetReader/Writer Swift script at 540x960, 1.6 Mbps, first
  40 seconds, lands around 8 MB, under the artifact's 15 MB file limit.

- Galleries are swipe only: no arrow buttons, small dots underneath, and
  mouse drag-to-swipe so it also works when he demos on a computer.
- Social links (Instagram, YouTube, TikTok) go right after the statement,
  before "What I bring". Show a "Coming soon" card for any he doesn't have yet.
- Favorite posts: just the three biggest reels.

## Media kit PDF (2026-10-14)

- The media kit is the numbers document for brands, separate from the page.
  He wanted it "clean", "modern, like map style", not interactive, keeping
  every fact from the creator's own kit and only rewriting.
- What worked: Letter pages in HTML, paper and ink with faint SVG contour
  lines (marching squares), mono map labels, one blaze-orange accent,
  photos in thin frames with "Fig." captions. Printed with headless Chrome
  `--print-to-pdf --no-pdf-header-footer`, checked page by page with PyMuPDF.
- Grid columns holding huge type need `minmax(0, 1fr)`, or the name pushes
  the column off the page. Images in a fixed-height grid row need a fixed
  height on the frame too.
- Messages attachments can't be read by the app directly (no Full Disk
  Access). Copy them out with `run_in_terminal` first.

## The Pholio template (2026-10-16, his words)

- Pholio pages now follow Gavin's template, not Colby's long scroll film. See
  the gallery at phoilio-gavinjmunroe-4969s-projects.vercel.app/gallery
  (pages /milesanddmiles and /tysonnek). He said: "check both the pholios i
  send and replicate somthing similair... this is very importart".
- Template order, phone width: photo hero fading into the page with the name
  over the fade; @handle left, "Work with me" right; one solid accent Shop
  bar; social icons; "In my shop" side-scroll cards; an auto-scrolling "Work
  with me" strip with a pause button; one big statement line; a second photo
  strip; a contact button. Short. Keep that order and change only the look.
- The look comes from the creator's own Instagram grid: "scrape her instagram
  and find her vibe. her colors so that it will tones and niche." Open the
  profile logged out (close the app popup), scroll the grid, and pull the
  palette and mood from it before picking colors or fonts. Their questionnaire
  answer ("earthy tones, girly and fun") narrows it; the grid decides it.
- Photos: "only use photos THAT LOOK DOPE AND CLEAN AND SHOW HER WELL."
  Contact-sheet every photo first and pick on purpose: sharp, well lit, face
  visible, the creator as the subject. Cut group shots, busy shots and
  anything where they're small in the frame, even if fewer photos remain.
- Drive folders shared by link download logged out: read the file ids from
  the folder page (`[data-id]`), then
  `curl -L "https://drive.usercontent.google.com/download?id=<id>&export=download&confirm=t"`.
- TikToks: `brew install yt-dlp`, then download one at a time (the load guard
  blocks backgrounded loops). The info JSON carries the real view counts.
- Brand cards without a known photo-to-brand match stay text only. Never pair
  a photo with a brand the creator didn't tie to it.

## Never

- Never publish a page that uses a real person's name and photos without the
  user saying so. Build it as a file first.
- Never fill a stat, brand or contact with a guess. Mark it.
- Never invent a new layout for a Pholio page when the template exists, and
  never pick colors before looking at the creator's own feed.
- Never use a photo that doesn't show the creator well just to fill a strip.
