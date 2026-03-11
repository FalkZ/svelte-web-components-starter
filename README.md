# Svelte Web Components Starter

A starter template for building web components with Svelte 5. Web components let you ship framework-independent UI that works in any HTML page or framework.

Check out `src/modal-example.svelte` for a full example. It defines a `<modal-example>` custom element with reactive props (`open`, `title`) and uses the Invoker API so external buttons can control it via `commandfor` and `command` attributes.

You can see it in action in `index.html`:

## Getting started

```sh
bun install
bun run dev
```

## Build

```sh
bun run build
```

This builds an ESM bundle of your web components.

## Preview

```sh
bun run preview
```

This serves a plain HTML page that loads the built bundle so you can test your components outside of dev mode.
