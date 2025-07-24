1. What are WebSockets and what are the alternatives?

WebSocket is a communication protocol that provides a full-duplex, persistent connection between a client (such as a web browser) and a server over a single TCP connection. 
Unlike HTTP, which is request-response based and typically requires the client to initiate every interaction, 
WebSocket allows both the client and server to send messages to each other at any time, enabling real-time, low-latency communication.


This is particularly useful for applications that require instant data updates, 
such as chat applications, online gaming, live sports updates, and collaborative tools.

Alternatives to WebSockets include:
* HTTP Long Polling: The client repeatedly requests updates from the server. 
    This simulates real-time communication but is less efficient and introduces more latency compared to WebSockets.
* Server-Sent Events (SSE): Allows servers to push updates to the client over a single HTTP connection, 
    but it is one-way (server-to-client only).
* WebRTC: Primarily designed for peer-to-peer communication (audio, video, and data), 
    often used in video conferencing apps like Zoom. It is more complex and suited for direct client-to-client communication rather than client-server.
* WebTransport: A newer protocol that enables low-latency, bidirectional communication over HTTP/3. 
    It is designed to be a modern alternative to WebSockets and is still being adopted.

Key differences:
With HTTP, the client must always initiate communication to receive new data.
With WebSocket, after the initial handshake, either the client or server can send messages at any time.
WebSocket is bidirectional and persistent, while alternatives like SSE are one-way, and long polling is less efficient.

2. What is the difference between HTTP/1.1, HTTP/2, and HTTPS?

HTTP stands for HyperText Transfer Protocol. HTTP/1.1, introduced in 1997, is a protocol for transferring data between a client (such as a browser) and a server. 
It uses a request-response model, where the client sends a request and waits for the server's response. 
HTTP/1.1 introduced persistent connections, allowing multiple requests and responses over the same TCP connection, but requests are still processed sequentially, which can cause delays (head-of-line blocking). 
Features like chunked transfer encoding and pipelining were added, but pipelining is rarely used in practice.

HTTP/2 is a major revision designed to improve performance and efficiency.
It uses a single, persistent TCP connection to multiplex multiple requests and responses simultaneously (multiplexing), reducing latency. 
Data is split into small packets called frames, which can be interleaved and prioritized. 
HTTP/2 also compresses headers to reduce overhead. This allows resources (like CSS, JS, images) to be loaded in parallel, improving page load times.

HTTPS stands for HyperText Transfer Protocol Secure. 
It is HTTP (any version) layered over SSL/TLS, providing encryption, data integrity, and authentication. 
HTTPS ensures that data exchanged between client and server is encrypted and cannot be easily intercepted or tampered with. 
The security is established through SSL/TLS handshakes and digital certificates, which verify the server's identity.

HTTP/3 is the latest version, built on top of QUIC (a protocol over UDP). 
It further reduces latency and improves performance, especially in unreliable network conditions, 
by eliminating head-of-line blocking at the transport layer and providing faster connection establishment.

3. Webpack VS Vite

Bundlers are tools that take web app source code (JS, CSS, assets e.t.c) and package them into optimized files that browser can efficiently load.

Why do we need bundlers?
* Module System: Browsers don't natively understand modern module systems (ES6 imports/exports, CommonJS)
* Performance: Loading hundreds of individual files is slow; bundlers combine them into fewer, optimized files
* Transformation: Convert modern code (TypeScript, JSX, SCSS) into browser-compatible code
* Optimization: Minification, tree-shaking (removing unused code), code splitting
* Asset Management: Handle images, fonts, and other assets

Webpack is relatively old, but powerful module bundler.
* Webpack bundles all files into one or more output bundles.
* Transforms code using loaders (Babel for JS, Sass for CSS, etc.)
* Optimizes with plugins (minification, code splitting etc.)
* Manages all types of assets (JS, CSS, images, fonts)

Webpack Characteristics:
* Configuration-heavy: Requires detailed setup via webpack.config.js
* Build-time focused: Optimizes everything during the build process
* Mature ecosystem: Extensive plugin and loader ecosystem
* Production-ready: Battle-tested for large applications

Vite is a modern build tool that focuses on speed and developer experience.
* Development: Uses native ES modules in the browser (no bundling during dev)
* Production: Uses Rollup under the hood for optimized production builds
* Transforms code on-demand during development
* Hot Module Replacement (HMR): Extremely fast updates during development

Vite Characteristics:
* Zero-config: Works out of the box with minimal setup
* Development-speed focused: Instant server start and lightning-fast HMR


4. Local Storage, Session Storage, and IndexedDB

In Web development, you can store data on the client-side using several storage mechanisms, each with different characteristics and use cases.

**Local Storage**
Local Storage provides persistent storage that survives browser restarts and system reboots.
* **Data Types**: Only stores strings (you need JSON.stringify/parse for objects)
* **Capacity**: ~5-10MB per origin (varies by browser)
* **Persistence**: Data persists until explicitly cleared by user or code
* **Scope**: Shared across all tabs/windows of the same origin
* **Synchronous API**: Blocking operations that can affect performance
* **Security**: Not encrypted, accessible via JavaScript (vulnerable to XSS)
* **Use Cases**: User preferences, theme settings, simple app state

```javascript
// Local Storage API
localStorage.setItem('key', 'value');
const value = localStorage.getItem('key');
localStorage.removeItem('key');
localStorage.clear();
```

**Session Storage**
Session Storage provides temporary storage that lasts only for the browser session.
* **Data Types**: Only strings (same as localStorage)
* **Capacity**: ~5-10MB per origin (similar to localStorage)
* **Persistence**: Data is cleared when the tab is closed
* **Scope**: Isolated per tab/window (not shared between tabs)
* **Synchronous API**: Same blocking behavior as localStorage
* **Security**: Same security concerns as localStorage
* **Use Cases**: Form data, temporary UI state, single-session workflows

```javascript
// Session Storage API (identical to localStorage)
sessionStorage.setItem('key', 'value');
const value = sessionStorage.getItem('key');
```

**IndexedDB**
IndexedDB is a low-level, NoSQL database for storing large amounts of structured data.
* **Data Types**: Can store objects, arrays, files, blobs, and most JavaScript types
* **Capacity**: Much larger (~50MB to several GB, depending on available disk space)
* **Persistence**: Data persists until explicitly deleted or browser storage is cleared
* **Scope**: Shared across tabs of the same origin
* **Asynchronous API**: Non-blocking operations using promises/callbacks
* **Security**: More secure than localStorage, supports encryption
* **Transactions**: ACID-compliant transactions for data integrity
* **Indexing**: Supports indexes for efficient querying
* **Use Cases**: Offline data caching, large datasets, complex app state, PWA data storage

```javascript
// IndexedDB API (more complex)
const request = indexedDB.open('MyDatabase', 1);
request.onsuccess = (event) => {
  const db = event.target.result;
  const transaction = db.transaction(['store'], 'readwrite');
  const store = transaction.objectStore('store');
  store.add({ id: 1, name: 'John', email: 'john@example.com' });
};
```

**Comparison Table**

| Feature | Local Storage | Session Storage | IndexedDB |
|---------|---------------|-----------------|-----------|
| **Persistence** | Until cleared | Until tab closed | Until explicitly deleted |
| **Capacity** | ~5-10MB | ~5-10MB | ~50MB to several GB |
| **Data Types** | Strings only | Strings only | Any serializable data |
| **API Type** | Synchronous | Synchronous | Asynchronous |
| **Performance** | Can block UI | Can block UI | Non-blocking |
| **Querying** | Key-value only | Key-value only | Complex queries with indexes |
| **Transactions** | No | No | Yes (ACID) |
| **Browser Support** | Excellent | Excellent | Good (IE10+) |

**Important Considerations for Mid-Level Developers:**

1. **Storage Quotas**: Browsers implement storage quotas per origin. Use `navigator.storage.estimate()` to check available space.

2. **Storage Events**: localStorage changes trigger `storage` events across tabs:
```javascript
window.addEventListener('storage', (e) => {
  console.log('Storage changed:', e.key, e.oldValue, e.newValue);
});
```

3. **Error Handling**: Storage operations can fail (quota exceeded, private browsing):
```javascript
try {
  localStorage.setItem('key', 'value');
} catch (e) {
  if (e.name === 'QuotaExceededError') {
    // Handle storage quota exceeded
  }
}
```

4. **Performance**: Avoid storing large strings in localStorage/sessionStorage as it blocks the main thread.

5. **Privacy Modes**: Storage behavior differs in private/incognito mode - sessionStorage works, localStorage may be limited.

6. **Security Best Practices**:
   - Never store sensitive data (passwords, tokens) in localStorage/sessionStorage
   - Sanitize data before storing to prevent XSS
   - Consider encryption for sensitive data in IndexedDB

**When to Use Each:**
- **localStorage**: User preferences, settings, simple app state that should persist
- **sessionStorage**: Temporary form data, wizard steps, single-session state
- **IndexedDB**: Offline-first apps, large datasets, complex data relationships, PWAs


5. React Router Dom

React Router DOM is the standard library for implementing routing in React applications. 
It enables navigation between different components/pages while maintaining a Single Page Application (SPA) experience without full page reloads.

## Core Concepts and Architecture

**Client-Side Routing**
React Router implements client-side routing, where URL changes are handled by JavaScript rather than server requests.
* **SPA Behavior**: Page doesn't reload, only components change
* **History API**: Uses browser's History API to manage URL changes
* **Component-Based**: Routes render React components
* **Declarative**: Routes are defined using JSX components

**Router Types**
* **BrowserRouter**: Most common, uses HTML5 History API, clean URLs
* **HashRouter**: Uses URL hash (#), works with static hosting
* **MemoryRouter**: For testing or React Native, no URL changes

## React Router v6 Key Features

**Route Matching and Ranking**
V6 introduced intelligent route ranking that prioritizes more specific routes:
* Static segments have highest priority (`/users/new`)
* Dynamic segments have medium priority (`/users/:id`)
* Splat routes have lowest priority (`/users/*`)
* Order doesn't matter - React Router automatically ranks by specificity

**Nested Routes and Outlets**
* Parent routes can have child routes
* `<Outlet />` component renders child routes
* Enables shared layouts and complex routing structures

## Essential Hooks and Components

**Navigation Hooks**
* **useNavigate()**: Programmatic navigation, replaces useHistory
* **useLocation()**: Current location info (pathname, search, state)
* **useParams()**: Extract URL parameters from dynamic routes
* **useSearchParams()**: Query string management (get/set URL params)

**Link Components**
* **Link**: Standard navigation without page reload
* **NavLink**: Link with active state styling capabilities
* **Form**: Enhanced form with action integration (v6.4+)

## Advanced Features for Mid-Level Developers

**Data Loading with Loaders (v6.4+)**
* Loaders run before component renders
* Built-in loading states and error handling
* Automatic request cancellation prevents race conditions
* Access loaded data with `useLoaderData()`

**Form Handling with Actions**
* Actions handle form submissions
* Work with standard HTML forms or React Router Form component
* Return responses or redirects
* Access action results with `useActionData()`

**Route Protection and Authentication**
* Use loaders to check authentication before rendering
* Redirect unauthorized users to login
* Store intended destination for post-login redirect
* Role-based access control through loader logic

**Code Splitting and Lazy Loading**
* Component-level: `lazy(() => import('./Component'))`
* Route-level: `lazy` property in route config (v6.4+)
* Use `<Suspense>` for loading states
* Enables bundle splitting and performance optimization

## Error Handling and Boundaries

**Error Elements**
* Route-specific error handling with `errorElement`
* Global error boundaries for unhandled errors
* Access error info with `useRouteError()`
* Different error types: 404, loader errors, component errors

## Performance and Optimization

**Navigation State Management**
* `useNavigation()` provides loading/idle states
* Show loading indicators during route transitions
* Handle form submission states

**Prefetching Strategies**
* Hover-based prefetching for better UX
* Programmatic prefetching of likely routes
* Route-based code splitting optimization

## Common Patterns and Best Practices

**Layout Routes Pattern**
* Shared layouts for different app sections
* Authentication layouts vs app layouts
* Nested layout hierarchies

**Modal Routes with Background Location**
* Modals that preserve background route
* URL-driven modal state
* Proper browser back button behavior

**Search Params Management**
* Sync component state with URL params
* Preserve filters/search in URL
* Clean URL state management

## Interview-Critical Concepts

**V5 vs V6 vs V7 Differences**
* **Switch → Routes**: Routes component replaces Switch (v6)
* **Exact prop removed**: Routes are exact by default (v6)
* **Route ranking**: Intelligent route matching vs order-dependent (v6)
* **Nested routes**: Built-in support with Outlet (v6)
* **Hook-based navigation**: useNavigate vs useHistory (v6)
* **Data loading**: Built-in loaders and actions (v6.4+)
* **Relative routing**: Improved relative path resolution (v6)
* **React 18+ required**: Minimum React version is 18 (v7)
* **Turbopack support**: Enhanced development performance (v7)
* **Improved TypeScript**: Better type inference and safety (v7)
* **Framework integration**: Better Next.js migration patterns (v7)
* **Enhanced loaders**: More efficient data loading strategies (v7)
* **Streaming SSR**: Improved server-side rendering capabilities (v7)
* **Vite optimization**: Better Vite integration and HMR (v7)

**Common Gotchas**
* **Relative paths**: Understanding `../` vs `/parent` behavior
* **Index routes**: `<Route index />` for default child routes
* **Trailing slashes**: React Router normalizes them automatically
* **Case sensitivity**: Routes are case-sensitive by default
* **Special characters**: URL encoding in path parameters
* **Router recreation**: Don't recreate router instances conditionally

**Performance Considerations**
* **Route-based code splitting**: Split by routes, not individual components
* **Preload strategies**: Hover, focus, viewport-based prefetching
* **Loader race conditions**: Built-in request cancellation
* **Bundle optimization**: Role-based chunking strategies
* **Memory leaks**: Proper cleanup in useEffect hooks

**Security and Authentication**
* **Route guards**: Loader-based vs component-based protection
* **Token refresh**: Automatic token renewal during navigation
* **Role-based access**: Granular permission checking
* **Redirect preservation**: Maintaining intended destination after login

**Modern React Router Patterns**
* **Data router**: createBrowserRouter vs legacy BrowserRouter
* **Route objects**: Configuration-based routing
* **Loader composition**: Reusable data loading logic
* **Error boundaries**: Comprehensive error handling strategies
* **Form workflows**: Modern form handling with actions

