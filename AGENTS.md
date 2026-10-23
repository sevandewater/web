# Working on this static site

Keep the site simple: directly editable HTML pages, a shared structure, and one
stylesheet. Consistency comes from following the same template and workflow.

## Structure and styling

- Keep public site files in `docs/`, with HTML pages directly inside it and all
  styles in `docs/styles.css`. Keep development, deployment, and repository
  documentation outside `docs/`.
- Do not add page-specific stylesheets, inline styles, page-specific CSS rules,
  content JSON, generators, frameworks, or build dependencies.
- Define a shared theme for `h1`, `h2`, `h3`, paragraphs, links, lists, navigation,
  and footer text. Equivalent elements must have the same font family, size,
  weight, line height, and spacing across pages.
- General body text uses the same paragraph style everywhere. Do not make the
  homepage's general text smaller or give article pages a separate body style.
- Use a small set of reusable layout styles. Follow the same page structure
  unless the content gives a concrete reason to differ. The homepage may need a
  different content arrangement; its shared typography, header, and footer still
  follow the site theme.
- Keep header and footer markup identical across pages, apart from the current
  navigation link's `aria-current` attribute.
- Use relative internal links and asset paths so pages can be reviewed locally.

## Workflow

1. Establish the shared typography and page structure before spreading changes
   across the site. Existing styles are not automatically an approved theme.
2. Implement and refine one reference page first. Start with `docs/index.html` for the
   current work. Review the rendered result until it is coherent before copying
   the pattern to the other pages.
3. Apply the established structure to every applicable page. Copy the shared
   header and footer exactly; vary the content rather than inventing new styles.
4. Make shared visual changes in `docs/styles.css` so every page receives them.
5. Replicate shared HTML changes, including navigation items, header, and footer,
   across all pages in the same change.
6. Review the affected pages again through actual navigation. Check desktop and
   mobile layouts and confirm relative links and assets still work.

## Review and reporting

- Compare equivalent text and shared elements visually at the same viewport and
  browser zoom. Check the homepage against an interior page, including Privacy.
- For a reported scale mismatch, verify the actual browser's saved zoom for the
  exact URLs. A fresh test profile does not reproduce existing zoom overrides.
  Check text, images, header height, and container width before changing CSS;
  do not compensate for a browser zoom difference with page-specific styles.
- Matching CSS declarations or a few computed-style measurements are supporting
  evidence, not proof of visual consistency. Verify font loading and the rendered
  result, including ordinary paragraphs rather than only headings or lead text.
- Investigate a reported mismatch before claiming it is fixed. Distinguish what
  was observed, what was measured, and what remains unresolved.
- Keep verification proportionate. Do not introduce a build or test framework for
  this small static site.

## Approved reference

The user approved the refined homepage as the reference for the other pages.
Reuse its typography from `styles.css`; do not redesign the theme during rollout.
The next page is Privacy. Compare its ordinary paragraphs against Home, including
rendered font size: the user previously reported smaller body text on Home.
Keep that visual comparison part of the review rather than assuming consistency.

The later screenshot mismatch was traced to Chrome's saved local-file zoom:
`index.html` had an 80% override; `privacy.html` used the 100% default. This was
verified in the browser's saved preferences. Review both pages at the same zoom
before changing the established typography. Browser preferences were not edited.
