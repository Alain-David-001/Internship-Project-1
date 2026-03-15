# Align Demo

Small `Vue 3 + Webpack + Sass` frontend built for the internship task about tracing generated CSS back to source in WebStorm.

## Setup

```bash
npm install
```

## Run

```bash
npm run dev
```

The dev server runs on `http://localhost:8080`.

## Build

```bash
npm run build
```

## Live Demo

The project is also available on GitHub Pages:

- `https://alain-david-001.github.io/Internship-Project-1/`

## How Authored CSS Becomes Final Browser CSS

The project uses global SCSS from `src/styles/main.scss` and scoped SCSS inside Vue single-file components such as `src/App.vue` and `src/components/PlanCard.vue`.

During the build:

1. Webpack starts from `src/main.js`, which imports both the Vue app and the global SCSS file.
2. When Webpack encounters a `.vue` file, `vue-loader` parses the single-file component and extracts its `<style lang="scss">` blocks.
3. `sass-loader` compiles those SCSS styles into standard CSS.
4. For scoped component styles, Vue then rewrites the selectors with generated `[data-v-...]` attributes so they apply only to that component.
5. `css-loader` processes the resulting CSS and preserves the source-map chain.
6. `mini-css-extract-plugin` writes the combined stylesheet to `dist/css/main.css`.
7. With Webpack `devtool: "source-map"`, Webpack also emits `dist/css/main.css.map`.

The final CSS does not correspond 1-to-1 with the original source files because it combines global and component styles, transforms SCSS into CSS, and rewrites scoped selectors before the browser loads the generated stylesheet.

## Generated CSS and Source Maps

After running `npm run build`, the generated files are in:

- `dist/css/main.css`
- `dist/css/main.css.map`
