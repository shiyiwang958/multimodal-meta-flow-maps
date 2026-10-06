# Multimodal Meta Flow Maps — project page

Project page for *Scalable Inference-time Steering in Biological Design with Multimodal Meta Flow Maps*.

**Live at <https://shiyiwang958.github.io/multimodal-meta-flow-maps/>** · code at
<https://github.com/shiyiwang958/MultiMFM>

One HTML file plus images. No build step, no dependencies — open `index.html` in a browser to view
it locally.

```
index.html              the page
assets/
  teaser.jpg            Fig. 1  (figures/norway.pdf)
  method.png            Fig. 2  (figures/fig.pdf)
  dna_schematic.png     Fig. 3  (figures/dMFM_cyclizability_schematic_final_1.png)
  geom_fingerprint.png          (figures/geom_fingerprint.pdf)
  qm9_nfe.png                   (figures/qm9_property_steering.pdf)
  dna_scaling.png               (figures/dna_scaling_dmfm.pdf)
  og-image.jpg          1200x630 social preview
```

## Still to do

**Add the paper.** The hero's "Paper (PDF)" button points at `paper.pdf`, which is not in the repo
yet — it is the only dead link on the page. Drop the compiled PDF in at the root and push:

```sh
cp /path/to/paper.pdf .
git add paper.pdf && git commit -m "Add paper PDF" && git push
```

If you would rather link to arXiv, change that `href` in `index.html` instead.

## Deployment

GitHub Pages, from `main` / `/ (root)`. Pushing to `main` redeploys; it takes a minute or so. Any
static host works the same way — the page is self-contained apart from two CDN loads.

If you move the page to another URL, update `og:image`, `twitter:image`, `og:url` and the canonical
link in `<head>`; social previews need absolute URLs.

## Regenerating the figures

The images are rasterized from the paper's own figures, trimmed of whitespace and given a small
white border:

```sh
cd /path/to/dMFM
for f in norway fig geom_fingerprint qm9_property_steering dna_scaling_dmfm; do
  pdftocairo -png -singlefile -r 300 figures/$f.pdf /tmp/hi_$f
done
convert /tmp/hi_norway.png -background white -alpha remove -fuzz 2% -trim +repage \
  -bordercolor white -border 24 -resize '2000x>' -strip -quality 88 assets/teaser.jpg
```

Repeat with the matching output name for the others. Keep them in the 1800–2200 px range: the page
renders at most 1088 px wide, so that covers HiDPI without bloating the page.

## Notes

- Theme-aware (light, dark, and OS default). Colors come from the preprint class — slate ink,
  juniper, accent green — plus the crimson the plots use for multiMFM.
- Math renders with KaTeX; typefaces come from Google Fonts. Both load from a CDN with subresource
  integrity hashes. To make the page work offline, vendor `katex.min.css`, `katex.min.js`,
  `auto-render.min.js`, the KaTeX font directory and the two font families, then rewrite those
  `<link>` and `<script>` tags to local paths.
- Every number and claim on the page comes from `paper.tex` in the code repo. If a result changes,
  the tables here need updating too — search for `tablefig`.
