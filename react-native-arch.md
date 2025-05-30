
# React Native Modern Architecture: Key Concepts

## 1. Metro Bundler

**What is Metro Bundler and how does it work?**

Metro is the JavaScript bundler for React Native. It takes in an entry file and various JavaScript modules, then bundles them into a single file (or multiple files) to be loaded by the app. Metro optimizes for fast incremental builds and supports features like hot module reloading, source maps, and asset management.

---

## 2. Babel

**What is Babel?**

Babel is a JavaScript compiler that transforms modern JavaScript (ES6+) into backward-compatible versions for older environments. In React Native, Babel enables the use of the latest JavaScript features and JSX syntax.

---

## 3. useLayout vs useLayoutEffect & Layout Advantages in New Architecture

**useLayout vs useLayoutEffect: What advantages does the new architecture have with layouts?**

- `useLayoutEffect` is a React hook that fires synchronously after all DOM mutations, allowing you to read layout from the DOM and re-render synchronously.
- The new React Native architecture improves layout handling by making layout and effects synchronous, reducing flicker and improving performance.
- More: [Synchronous Layout and Effects](https://reactnative.dev/architecture/landing-page#synchronous-layout-and-effects)

---

## 4. Transition Support in New Architecture & React 18

**What are the new features? (Suspense, Transitions)**

- The new architecture supports React 18's concurrent features, including Suspense and Transitions.
- This enables smoother UI updates, better user experience, and improved performance.
- More: [Support for Concurrent Renderer and Features](https://reactnative.dev/architecture/landing-page#support-for-concurrent-renderer-and-features)

---

## 5. Async Bridge Between JavaScript and Native

**What is the async bridge?**

- The async bridge is the communication layer between JavaScript and native code in React Native.
- It serializes data and sends it across threads asynchronously, which can introduce latency.
- More: [State of React Native 2018 - Architecture](https://reactnative.dev/blog/2018/06/14/state-of-react-native-2018#architecture)

---

## 6. Serialization

**What does serialization mean?**

Serialization is the process of converting data structures or objects into a format that can be transmitted and reconstructed later. In React Native, this is used to send data between JavaScript and native code.

---

## 7. React Native Threading Model

**What are the three threads?**

- **JavaScript Thread:** Runs JS code and business logic.
- **UI Thread:** Handles rendering and user interactions.
- **Shadow Thread:** Calculates layout using Yoga.
- More: [Threading Model](https://reactnative.dev/architecture/threading-model)

---

## 8. Yoga

**What is Yoga?**

Yoga is an open-source layout engine that implements Flexbox for cross-platform layout calculations in React Native.

- More: [Yoga v2.0.0](https://github.com/facebook/yoga/releases/tag/v2.0.0)

---

## 9. JSI: Replacing the Async Bridge

**What is JSI?**

- JSI (JavaScript Interface) is a new, faster way for JavaScript to interact with native code, replacing the async bridge.
- It allows direct, synchronous, and asynchronous calls between JS and native, improving performance.
- More: [Fast JavaScript/Native Interfacing](https://reactnative.dev/architecture/landing-page#fast-javascriptnative-interfacing)

---

## 10. Fabric

**What is Fabric?**

Fabric is the new rendering system in React Native, designed for better performance, reliability, and cross-platform consistency.

- More: [Fabric Renderer](https://reactnative.dev/architecture/fabric-renderer)

---

## 11. Cross-Platform Implementation

**How does React Native achieve cross-platform implementation?**

- The new architecture enables more code sharing and consistency across platforms (iOS, Android, Web).
- More: [Cross-Platform Implementation](https://reactnative.dev/architecture/xplat-implementation)

---

## 12. View Flattening

**What is View Flattening?**

- View Flattening is an optimization that reduces the number of native views created, improving performance and memory usage.
- More: [View Flattening](https://reactnative.dev/architecture/view-flattening)

---

## 13. Hermes & Bundled Hermes

**What is Hermes? What is bundled Hermes?**

- Hermes is a lightweight JavaScript engine optimized for React Native on mobile.
- Bundled Hermes refers to shipping Hermes as part of the app bundle, improving startup time and performance.
- More: [Bundled Hermes](https://reactnative.dev/architecture/bundled-hermes)

---

## 14. CodeGen

**What is CodeGen?**

CodeGen is a tool that generates native code from JavaScript/TypeScript type definitions, enabling type-safe and efficient communication between JS and native modules.

---

## 15. Turbo Modules vs Native Modules

**What are Turbo Modules?**

- Turbo Modules are the next generation of native modules in React Native, leveraging JSI for faster, synchronous access.
- They improve startup time and performance compared to legacy native modules.

---

## 16. Fabric Native Modules

**What are Fabric Native Modules?**

- Fabric Native Modules are native modules designed to work seamlessly with the new Fabric renderer and Turbo Modules system, providing better performance and integration.
