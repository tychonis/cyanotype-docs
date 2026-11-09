# Cyanotype Docs

Source for the [Cyanotype documentation](https://docs.tychonis.com), built with Docusaurus. It covers getting started with the [Cyanotype CLI](https://github.com/tychonis/cyanotype) and the BoM-as-Code data model.

## Develop locally

Use Node.js 20 or later and npm. Edit the Markdown and MDX files in `docs/`.

```sh
npm ci
npm run start
```

## Build

```sh
npm run build
npm run serve
```

The production build writes the static site to `build/`. Use `npm run serve` to preview it locally.

## License

Copyright © 2026 Tychonis LLC

Cyanotype is licensed under the Apache License, Version 2.0.
See [LICENSE](LICENSE).
