# anagnor-images

**anagnor** is a node-based image-filter playground that runs entirely in the
browser — images are never uploaded to any server. Instead of tabs and
sections, each image process is a **block** you drag onto a canvas and wire
together; the output of one block flows along the connections into the next.

🌐 **Live page:** https://elivasquezhdz.github.io/anagnor-images/
(published from `docs/` via GitHub Actions on push to `main`)

The filter algorithms are ported from the
[image-filters](https://github.com/elivasquezhdz/image-filters) project; the
node-graph editor around them is new.

## How it works

- **Drag a block** from the top bar onto the canvas (or click it to drop one in).
- **Connect blocks** by dragging from a block's **right** dot (output) to another
  block's **left** dot (input).
- **Click a block** to reveal its parameters and adjust them — the preview updates
  live and flows downstream.
- **Pan** by dragging the empty canvas, **zoom** with the scroll wheel, and click a
  wire to remove it.
- Every block previews its own output, so you can **download a PNG** from any block.

## Blocks

| Block | Inputs | What it does |
|-------|:------:|--------------|
| 🖼️ **Image** | – | The source. Uses a built-in sample until you upload your own. |
| 🌈 **Chroma Shift** | 1 | Offsets two colour channels for an RGB-glitch / aberration look (recursive iterations). |
| 🪣 **Fill** | 1 | Replicates an edge row/column across a rectangular region — plus diagonal and combined-corner modes. |
| 🪞 **4-Way Collage** | 1 | Mirrored 2×2 mosaic sliced and interleaved into a kaleidoscope. |
| 🧛 **Vampire** | 1 | A full Lightroom-style develop preset (exposure, tone, colour grade, grain, vignette…). |
| 🧍 **Foreground / Background** | 1 | Segments the subject with [MediaPipe Selfie Segmentation](https://google.github.io/mediapipe/solutions/selfie_segmentation) (runs in-browser) and applies an operation to the **foreground** or the **background**: invert, collage, chroma, low-res, fill or blur. |
| 🧩 **Boxes** | 2–5 | Splits each image into an `N×N` grid and interleaves the cells between images. |
| 🎛️ **Channel Merge** | 1–6 | Tints each input by luminance into a colour layer (R/G/B/C/Y/M) and adds them (additive light). |
| 👁️ **Preview** | 1 | A larger preview + download for the end of a pipeline. |

> **Note:** the *Foreground / Background* block was called *Person / Background* in
> the original image-filters app. It's the same in-browser segmentation model.

## Structure

```
docs/index.html   → the app (published to GitHub Pages)
docs/vendor/selfie/ → MediaPipe Selfie Segmentation model, hosted locally
.github/workflows/pages.yml → deploys docs/ to GitHub Pages
```

## Local development

The site is a single static file with no build step:

```bash
cd docs && python3 -m http.server 8000
# open http://localhost:8000
```

## Publishing (GitHub Pages)

The site deploys automatically with GitHub Actions on every push to `main`
(see [`.github/workflows/pages.yml`](.github/workflows/pages.yml)). GitHub Pages
must be configured with the **GitHub Actions** source in the repository settings.
