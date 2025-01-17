# Migration from v2

## Node Support

Vite no longer supports Node v12, which reached its EOL. Node 14.6+ is now required.

## Modern Browser Baseline change

The production bundle assumes support for modern JavaScript. By default, Vite targets browsers which support the [native ES Modules](https://caniuse.com/es6-module) and [native ESM dynamic import](https://caniuse.com/es6-module-dynamic-import) and [`import.meta`](https://caniuse.com/mdn-javascript_statements_import_meta):

- Chrome >=87
- Firefox >=78
- Safari >=13
- Edge >=88

A small fraction of users will now require using [@vitejs/plugin-legacy](https://github.com/vitejs/vite/tree/main/packages/plugin-legacy), which will automatically generate legacy chunks and corresponding ES language feature polyfills.

## Config Options Changes

- The following options that were already deprecated in v2 have been removed:

  - `optimizeDeps.keepNames` (switch to [`optimizeDeps.esbuildOptions.keepNames`](../config/dep-optimization-options.md#optimizedepsesbuildoptions))
  - `build.base` (switch to [`base`](../config/shared-options.md#base))
  - `alias` (switch to [`resolve.alias`](../config/shared-options.md#resolvealias))
  - `dedupe` (switch to [`resolve.dedupe`](../config/shared-options.md#resolvededupe))
  - `polyfillDynamicImport` (use [`@vitejs/plugin-legacy](https://github.com/vitejs/vite/tree/main/packages/plugin-legacy) for browsers without dynamic import support)

## Dev Server Changes

Vite optimizes dependencies with esbuild to both convert CJS-only deps to ESM and to reduce the number of modules the browser needs to request. In v3, the default strategy to discover and batch dependencies has changed. Vite no longer pre-scans user code with esbuild to get an initial list of dependencies on cold start. Instead, it delays the first dependency optimization run until every imported user module on load is processed.

To get back the v2 strategy, you can use [`optimizeDeps.devScan`](../config/dep-optimization-options.md#optimizedepsdevscan).

## Build Changes

In v3, Vite uses esbuild to optimize dependencies by default. Doing so, it removes one of the most significant differences between dev and prod present in v2. Because esbuild converts CJS-only dependencies to ESM, [`@rollupjs/plugin-commonjs`] is no longer used.

If you need to get back to the v2 strategy, you can use [`optimizeDeps.disabled: 'build'`](../config/dep-optimization-options.md#optimizedepsdisabled).

## SSR Changes

Vite v3 uses ESM for the SSR build by default. When using ESM, the [SSR externalization heuristics](https://vitejs.dev/guide/ssr.html#ssr-externals) are no longer needed. By default, all dependencies are externalized. You can use [`ssr.noExternal`](../config/ssr-options.md#ssrnoexternal) to control what dependencies to include in the SSR bundle.

If using ESM for SSR isn't possible in your project, you can set `ssr.format: 'cjs'` to generate a CJS bundle. In this case, the same externalization strategy of Vite v2 will be used.

## General Changes

- [Raw `import.meta.glob`](features.md#glob-import-as) switched from `{ assert: { type: 'raw' }}` to `{ as: 'raw' }`

- JS file extensions in SSR and lib mode now use a valid extension (`js`, `mjs`, or `cjs`) for output JS entries and chunks based on their format and the package type.

## Migration from v1

Check the [Migration from v1 Guide](https://v2.vitejs.dev/guide/migration.html) in the Vite v2 docs first to see the needed changes to port your app to Vite v2, and then proceed with the changes on this page.
