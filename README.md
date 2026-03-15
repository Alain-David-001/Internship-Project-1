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

## How Authored CSS Becomes Final Browser CSS

The project uses global Sass from `src/styles/main.scss` and scoped Sass inside Vue single-file components such as `src/App.vue` and `src/components/PlanCard.vue`.

During the build:

1. `vue-loader` compiles Vue single-file components.
2. `sass-loader` transforms Sass into standard CSS.
3. `css-loader` resolves the CSS and keeps the source map chain.
4. `mini-css-extract-plugin` extracts the final generated CSS into a real file.
5. Webpack emits source maps for the generated CSS.

The final CSS does not correspond 1-to-1 with the original source files because it combines global Sass, scoped component styles, Sass nesting, and Vue scoped-style rewriting.

## Generated CSS and Source Maps

After running `npm run build`, the generated files are in:

- `dist/css/main.css`
- `dist/css/main.css.map`
