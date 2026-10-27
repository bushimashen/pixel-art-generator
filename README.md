# Pixel Art Generator · 像素画生成器

> Turn any photo into pixel art or a mosaic poster. One HTML file, zero dependencies, all processing happens in your browser — your image never leaves it.

**English** | [简体中文](README.zh-CN.md)

<a href="https://bushimashen.github.io/pixel-art-generator/"><b>▶ Live demo</b></a>

## Features

- **10 style palettes** — classic pixel (auto-quantized), retro handheld (4-color green), arcade 16 (PICO-8), old-PC CGA, monochrome newspaper, perler-bead chart (MARD 291), full perler chart (ARTKAL 415), neon cyberpunk, warm sunset, glacier cool
- **Pixel grid** adjustable from 16 to 512, with avatar 32 / standard 64 / detailed 128 / poster 256 / extreme 512 presets
- **Color count** from 8 to 512 (used by the auto-quantize and grayscale modes)
- **Brightness / contrast** adjustable in real time
- **Floyd–Steinberg dithering** — photos and gradients blend far more naturally with it on
- **Export** at 1× / 4× / 8× / 12× / 16× PNG with `imageSmoothingEnabled = false`, so upscaled output stays crisp blocks
- Shows the colors actually used, handy when matching real materials (especially for perler-bead references)
- Import via click, drag-and-drop, or **Ctrl+V paste**

## Usage

Double-click `index.html`. No installation, no build, no network.

Prefer a local server?

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## Algorithm

1. **Downscale** — the source is drawn to canvas at the target grid size with bicubic interpolation, which already acts as area averaging / antialiasing.
2. **Brightness and contrast** — contrast uses the classic factor `cf = 259 * (c + 255) / (255 * (259 - c))`.
3. **Palette**:
   - `auto` quantization — builds a histogram at 4 bits per channel, keeps the top-K buckets and averages each one
   - `gray` — K evenly spaced levels across 0–255
   - everything else is a fixed palette
4. **Mapping** — nearest color by redmean-weighted distance (closer to human perception than plain RGB), cached by exact RGB for speed.
5. **Dithering** (optional) — Floyd–Steinberg error diffusion at 7/16, 3/16, 5/16, 1/16.

## About the color data

The built-in **MARD 291** and **ARTKAL 415** perler bead charts come from community-maintained datasets in
[Pindo](https://github.com/LunarXuan/Pindo) and [pindou-color-data](https://github.com/HansBug/pindou-color-data)
(brands do not publish official HEX values; these are community-verified approximations).

Because of that, this project is licensed **GPL-3.0**, matching the upstream data — see [LICENSE](LICENSE).

## License

GPL-3.0 (the bundled perler color charts originate from GPL-3.0 projects). You are free to use and modify the code;
if you redistribute it, keep the same license and credit the upstream data sources.
