# Melon Jelly Ninja

A Fruit Ninja–style slicing game starring wobbly, translucent watermelon jelly. Slices of jelly are tossed into the air; swipe through them to cut them into pieces, and stay away from the bombs.

**Play it:** `https://zipolitana.github.io/melon-jelly-ninja` 

It's a single HTML file with no build step and no dependencies.

## How to play

- **Swipe** (mouse drag or touch) across the jelly while it's in the air. Every swipe cuts whatever it crosses, and you can cut the pieces again.
- **+1** for every slice you cut. Cut **3 or more in one swipe** for a combo bonus.
- **Don't drop them.** Miss three whole slices and the game is over.
- **Avoid the bombs.** They start showing up a few waves in. Slice one and it explodes, blasts the jelly across the screen, and ends the game.
- Waves get denser and quicker the longer you last.
- Your best score is saved in your browser.

Keyboard: `Space` pauses, `R` restarts, `Enter` starts from the menu or game-over screen.

## Requirements

The game renders with **WebGPU**, so it needs a browser that supports it, such as recent desktop **Chrome** or **Edge**. In other browsers you'll see a "needs WebGPU" message instead of the game.

WebGPU only works on secure origins, so open it over `https://` (GitHub Pages is fine) or `http://localhost`.

## Run it locally

No install needed. From the folder containing `index.html`:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## Deploy on GitHub Pages

1. Put `index.html` at the root of this repo.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, set the source to **Deploy from a branch**, choose your main branch and the `/ (root)` folder, and save.
4. After a minute or so the game is live at `https://<your-username>.github.io/<repo-name>/`.

## How it's built

- **Rendering:** WebGPU with WGSL shaders. The jelly is drawn as a translucent volume, with refraction based on how much jelly each view ray passes through, plus seeds, bubbles, soft shadows and contact shading.
- **Physics:** a soft-body simulation on a tetrahedral mesh, solved with XPBD in plain JavaScript on the CPU. The smooth render surface is attached to the simulation mesh, so it deforms with it.
- **Cutting:** a swipe is turned into a cutting plane. The affected pieces are split into convex halves, which are re-meshed and handed their parent's motion. Mesh building is spread over several frames so cuts don't freeze the game.
- **Effects and bombs:** a 2D canvas layer on top of the scene draws the slash trail, juice, splatter, explosions and the bombs.

## Known limitations

- All slices share one shape, and colour changes per wave rather than per fruit.
- Bombs are 2D sprites drawn over the scene, so they always appear in front of the jelly.
- Slices that land stay on the floor until the next wave clears them.
- Each cut takes a moment to appear on slower machines, because the new pieces are built in the background.
- The physics uses fewer substeps than a pure soft-body demo would, so the jelly is a little less wobbly than it could be.

## License

MIT
