# Streaming

Streaming is a strategy that allows for progressive UI rendering from the server.

Work is divided into chunks and streamed to the client as soon as it is ready.

This enables the client to see parts of the page immediately, before the entire content has finished rendering.

Streaming significantly improves the initial page loading performance and the rendering of UI elements that rely
on slower data fetches, which would otherwise block the rendering of the entire route.

## How to use Streaming
To use Streaming, wrap the component using the `<Suspense>` component which is imported from `'react'` and Next.js will
take care of the rest.