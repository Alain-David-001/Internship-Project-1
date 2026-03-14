# Pulse Workspace Pricing Demo

Small Vue 3 + Webpack + Sass frontend built for the internship task around tracing generated CSS back to source in WebStorm.

## Setup

```bash
npm install
```

## Run locally

```bash
npm run dev
```

The development server runs on `http://localhost:8080`.

## Production build

```bash
npm run build
```

## How the authored CSS is transformed

The project uses Vue single-file components with `lang="scss"` styles plus one global Sass entry file.

Build flow:

1. `src/main.js` imports `src/styles/main.scss` and the root Vue component.
2. `vue-loader` compiles `.vue` single-file components.
3. `sass-loader` turns Sass syntax into standard CSS and preserves source map data.
4. `css-loader` resolves CSS imports and continues the source map chain.
5. `mini-css-extract-plugin` extracts the final CSS into a real file in `dist/css/`.
6. Webpack emits matching source maps so the browser can map generated CSS back to the authored `.scss` and `.vue` style blocks.

This setup intentionally creates generated CSS that does not map 1-to-1 with the authored files because styles come from:

- global Sass in `src/styles/main.scss`
- scoped component styles in `src/App.vue`
- scoped component styles in `src/components/PlanCard.vue`
- Sass nesting
- Vue scoped-style selector rewriting

## Where to find generated CSS and source maps

After `npm run build`, look in:

- `dist/css/main.css`
- `dist/css/main.css.map`

The built JavaScript is emitted to:

- `dist/js/main.js`

## Suggested Step 2 target

The featured `Team` pricing card is the best inspection target for the follow-up report because its final appearance is influenced by:

- base card styles
- the `.plan-card--featured` modifier
- CSS custom properties for accent colors
- global Sass rules in `main.scss`
- scoped rules from both `App.vue` and `PlanCard.vue`
- hover and responsive overrides
