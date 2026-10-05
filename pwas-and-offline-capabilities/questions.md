### 1. How do you design a React application to meet Progressive Web App (PWA) standards?

**Answer:**
I start by ensuring the core PWA criteria are met:

- **Responsive UI** : Built with mobile-first design using CSS frameworks or media queries.
- **HTTPS** : Required for service workers and security.
- **Web App Manifest** : Defines app metadata (name, icons, theme color).
- **Service Worker** : Enables caching and offline support.

In `create-react-app`, much is scaffolded. I customize `manifest.json` and register the service worker:

```

// index.js
import * as serviceWorkerRegistration from './serviceWorkerRegistration';
serviceWorkerRegistration.register();

```

I also use Lighthouse to audit and verify compliance.

<br />

### 2. What’s your approach to implementing offline caching in a React app using service workers?

**Answer:**
I use a service worker to cache essential assets (HTML, CSS, JS) and optionally API responses:

```

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open('static-v1').then(cache =>
      cache.addAll(['/index.html', '/main.css', '/main.js'])
    )
  );
});

```

For API caching:

```

self.addEventListener('fetch', event => {
  if (event.request.url.includes('/api/')) {
    event.respondWith(
      fetch(event.request).catch(() => caches.match(event.request))
    );
  }
});

```

This ensures the app shell and some dynamic data are available offline.

<br />

### 3. How do you integrate push notifications into a React PWA?

**Answer:**

1. **Request Permission** using the Notifications API.
2. **Register for Push** with the Push API and service worker.
3. **Subscribe to a Push Service** (e.g., Firebase Cloud Messaging or VAPID server).
4. **Handle Push in SW** :

```

self.addEventListener('push', event => {
  const data = event.data.json();
  self.registration.showNotification(data.title, {
    body: data.body,
    icon: '/icons/icon-192.png'
  });
});

```

On the React side, I integrate the subscription logic and send it to the backend for storage.

<br />

### 4. What’s your strategy for ensuring a React app remains functional offline with dynamic data?

**Answer:**

- **Cache dynamic API data** using IndexedDB (via libraries like `idb`) or localStorage.
- Use **"Cache then network"** or **"Network falling back to cache"** strategy.
- Maintain a **queue** of write operations (e.g., form submissions) and sync them when online.

For writes, store the queued intent in **IndexedDB** before showing it as saved locally. Keep a stable operation ID across retries, with **server-side idempotency** to avoid applying the same write twice. A timeout does not prove that the server rejected the write: keep its status pending until an acknowledgement or status check confirms the outcome.

For a worked interview scenario, see [Offline Email Client — free question and reference answer](https://frontendatlas.com/system-design/offline-email-client), which covers persistent drafts, incremental sync, and queued sending.

Read-cache example (the write queue needs separate storage and synchronization):

```

// Fallback to cache on failure
const getData = async () => {
  try {
    const res = await fetch('/api/data');
    const json = await res.json();
    localStorage.setItem('cachedData', JSON.stringify(json));
    return json;
  } catch {
    return JSON.parse(localStorage.getItem('cachedData'));
  }
};

```

<br />

### 5. How do you test the offline capabilities of a React PWA across different devices and browsers?

**Answer:**

- Use **Chrome DevTools** → Application tab → Service Workers → Simulate offline.
- Perform **Lighthouse audits** for offline support.
- Manually disable Wi-Fi and test critical flows.
- Use **BrowserStack** or real devices for cross-browser testing.
- Test both cold start (fully offline) and warm start (cached session).

<br />

### 6. What’s your approach to optimizing the installability of a React PWA (e.g., manifest files)?

**Answer:**

- Ensure `manifest.json` is valid and linked in `index.html`:

```html
<link rel="manifest" href="/manifest.json" />
```

- Include required fields: `name`, `short_name`, `start_url`, `display`, `icons`, and `theme_color`.

```json
{
  "name": "My App",
  "short_name": "App",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

- Show install prompts using the `beforeinstallprompt` event.

<br />

### 7. How do you handle state management in a React PWA during network disruptions?

**Answer:**

- Use a local-first state strategy with libraries like **Zustand** , **Redux Persist** , or **Dexie.js** for syncing with IndexedDB.
- Detect online/offline using `navigator.onLine` or `window.addEventListener('online')`.
- Queue actions performed offline and reprocess them on reconnect.

Example:

```

window.addEventListener('online', () => {
  // Process queued actions
});

```

The goal is eventual consistency without blocking the UI.

<br />

### 8. What’s your process for debugging service worker issues in a React application?

**Answer:**

1. Open **DevTools → Application → Service Workers** to inspect lifecycle.
2. Use `console.log()` inside service worker and check DevTools → Console → Service Worker.
3. Validate the `fetch` and `install` events are firing.
4. Clear old caches during version upgrades.
5. Use **Lighthouse** to identify broken caching strategies or registration issues.

```

self.addEventListener('activate', event => {
  // Clean up old cache versions
});

```

<br />

### 9. How do you measure the performance impact of PWA features in a React app?

**Answer:**

- Use **Lighthouse** to measure performance, PWA compliance, and Time to Interactive (TTI).
- Monitor **service worker timing** with the Performance API.
- Track **cache hit rates** , and **network request fallbacks** using custom logging or analytics (e.g., Sentry, LogRocket).
- Use `web-vitals` to track metrics like FCP, LCP, and CLS.

<br />

### 10. How do you educate a team on building and maintaining a PWA with React?

**Answer:**

- Create a **shared doc or internal wiki** with PWA standards, patterns, and code snippets.
- Host **live workshops** or walkthroughs on:
  - Service workers
  - Caching strategies
  - Offline-friendly UX
- Use **Lighthouse** as a checklist.
- Provide **template repos** or boilerplates.
- Review PWA-specific code in PRs to ensure best practices.
