# sabbiracoustic1006.github.io

Personal site of Sabbir Ahmed. Plain HTML and one CSS file. No build step.
GitHub Pages serves this folder as it is.

## Pages

| Page | File |
| --- | --- |
| Home | `index.html` |
| Research (publications) | `publications/index.html` |
| Projects | `projects/index.html` |
| Writing | `blogs/index.html` and one file per post |

## Design

- All colours and fonts are at the top of `assets/styles/site.css` (section "1. Tokens").
- Each new page has `<html class="sa">` and loads `site.css`. The header and footer are written in each page.
- Older pages (blog posts, Ms. Pac-Man page) still use Bootstrap. They load the header and footer from `assets/html/`.

## How to change things

- New paper: copy one `<li class="paper">` block in `publications/index.html`. For the home page, do the same in the "Selected research" section of `index.html`.
- New job or degree: copy one `<li class="row">` block in `index.html`.
- New CV: replace `assets/cv/Sabbir_CV.pdf`. Keep the file name.
- Date in the footer: search for "Last updated".

## See it on your Mac

Run this inside the site folder:

```
python3 -m http.server 8765
```

Then open http://127.0.0.1:8765/
