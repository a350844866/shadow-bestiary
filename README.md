# Shadow Bestiary · 影戏山海

A shadow-play puzzle game in the browser. Beasts from the *Shan Hai Jing* (the
*Classic of Mountains and Seas*, a Chinese bestiary over 2,000 years old) are
prowling behind a village's paper screen at night. You can't see them, only
their shadows. Carry the oil lamp around behind the screen to reshape the
shadow, read the ancient descriptions on the talismans, and seal each beast
before the lamp oil runs out.

- Pick the wrong talisman, or run out of oil, and the beast tears through the
  paper.
- Pick the right one and the paper burns away to show the beast in full 3D.

Entry for the **Gaming** track of the Meshy Z-Axis Awards 2026.

## How to play

| | Mouse / touch | Keyboard |
|---|---|---|
| Move the lamp behind the screen | drag (or just move the mouse) | arrows / WASD |
| Bigger / smaller shadow (lamp nearer / further) | scroll, or ＋ / － | Q / E |
| Choose a talisman | click | 1–4 |
| Continue | button | Enter / Space |

The clues are about the creatures' shapes. Count tails, legs, wings and horns. A
fox with nine tails and a fox with wings throw very different shadows, but only
once the lamp is in the right place.

## Why the shadows matter

Each shadow is cast by the actual 3D model of the beast. Every frame the game
renders the models from the lamp's position into a mask and projects it onto
the paper, the same geometry a real oil lamp and a real puppet would give. The
puzzle is in the generated geometry itself: where the lamp is, which way the
beast is turned, how many tails overlap.

## How it was made

1. **Bestiary research (human + Claude):** chose 8 beasts whose classical
   descriptions are about silhouette (one leg, five tails, wings on a fish, no
   face). Paired each with decoys from the same book so the shadow, not the
   wording, decides.
2. **Concept art (OpenAI gpt-image):** one image per beast, in a gongbi
   folk-sculpture style. Each is a single creature on a plain background, with
   tails, legs and wings drawn apart so the outline reads cleanly. Two beasts
   needed rewritten prompts before their outline read. The 8 images are in
   [`concepts/`](https://github.com/a350844866/shadow-bestiary/tree/gh-pages/concepts).
3. **3D models (Meshy, Image to 3D):** each concept image went through Meshy 6
   Lite with PBR textures, one generation per beast (plus one retry for Bi
   Fang). Then:
   - compressed for the web with glTF-Transform (meshopt geometry, WebP
     textures, half the triangles; about 0.6 MB per beast);
   - in the game, turned to face across the stage and stood on a puppet ledge,
     so that with the lamp in its starting position the shadow sits on the
     paper;
   - Bi Fang is one-legged in the classic, but Meshy gave it two legs side by
     side (a retry did the same). A small script deletes one leg, caps the
     hole and moves the other leg under the centre of the body.
4. **Game (three.js, code written with Claude):**
   - projective shadow renderer;
   - paper shader with fibre, burn and tear effects;
   - beast motion staged like shadow puppetry;
   - all music and sound synthesised in WebAudio (a plucked pentatonic line,
     plus the wooden clappers that open a shadow-play performance).

## Credits

- 3D beast models: generated with [Meshy](https://www.meshy.ai) (Meshy free
  plan outputs, licensed
  [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)). Changes:
  compressed (meshopt geometry, WebP textures, triangle count halved); Bi
  Fang's second leg removed and the remaining leg moved under the body. The
  models are in
  [`models/`](https://github.com/a350844866/shadow-bestiary/tree/gh-pages/models).
- Concept art: generated with OpenAI gpt-image.
- Code: written with Claude (Anthropic).
- Classical text: *Shan Hai Jing* (public domain). English renderings are our
  own.
- Sound: synthesised in code, no samples.

## Running it

The public repository (`gh-pages` branch) holds only the built site that GitHub
Pages serves. From the source checkout:

```bash
npm install
npm run dev      # development server
npm run build    # static site in dist/
```

The models live in `public/models/<id>.glb`. If one fails to load, the game
falls back to a procedural stand-in for that beast; add `?placeholders` to the
URL to see all the stand-ins.
