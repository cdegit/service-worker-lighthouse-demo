# Service Worker + Lighthouse Issue Demo

[Lighthouse](https://developers.google.com/web/tools/lighthouse/) includes two audits which are used to determine if your Progressive Web App (PWA) works offline. There are the `Current page responds with a 200 when offline` and `start_url responds with a 200 when offline` audits. However, I'm getting a false positive for these audits when using Lighthouse 5.1.0 and a very simple service worker setup. This repo is intended to demonstrate this issue.

This repo contains a tiny service worker that just overrides the default `fetch` behaviour, in order to just call `fetch`:
```
self.addEventListener('fetch', (event) => {
    event.respondWith(fetch(event.request))
})
```
It doesn't cache anything. With this service worker, this PWA *does not* work offline. If you go offline and refresh the page, the page will fail to load. However, the offline audits in Lighthouse show that this page *does* work offline, which is incorrect.

## Setup

```
// Install dependencies
npm i

// Start the express server
node index.js
```

## To reproduce the issue

- Start the express server
- Navigate to localhost:3000
- Use the Audit panel in Chrome Dev Tools or the Lighthouse Chrome Extension to run Lighthouse
- Review the results - the `Current page responds with a 200 when offline` and `start_url responds with a 200 when offline` audits have passed