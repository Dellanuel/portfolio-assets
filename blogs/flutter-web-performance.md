## The Problem

My portfolio site took 8 seconds to reach first meaningful paint. For a developer portfolio — a site that is supposed to demonstrate competence — this was embarrassing. Here is exactly what I did to cut it to under 4 seconds.

## 1. Deferred Imports (Code Splitting)

This was the single biggest win. By default, Flutter web compiles everything into one massive `main.dart.js`. Deferred imports split your code into chunks loaded on demand.

```dart
import 'package:portfolio/features/about/about.dart'
    deferred as about;
import 'package:portfolio/features/projects/projects.dart'
    deferred as projects;
```

Each page now loads its own JavaScript chunk only when navigated to. Initial bundle size dropped by 40%.

The key is wrapping deferred widgets properly:

```dart
DeferredWidget(
  libraryLoader: about.loadLibrary,
  childBuilder: () => about.AboutPage(),
);
```

## 2. Image Optimization

Network images were the second biggest culprit. Three fixes:

- **Serve WebP** instead of PNG where possible — 30% smaller at equivalent quality
- **Lazy load below-fold images** — only the hero section image loads immediately
- **Shimmer placeholders** — perceived performance matters as much as real performance

## 3. Font Loading Strategy

Google Fonts downloads fonts at runtime by default, causing a flash of unstyled text. The fix: preconnect to the Google Fonts CDN in your `index.html`:

```html
<link rel="preconnect"
  href="https://fonts.googleapis.com">
<link rel="preconnect"
  href="https://fonts.gstatic.com" crossorigin>
```

## 4. Avoid Unnecessary Rebuilds

This is less about load time and more about runtime smoothness. Two patterns I enforce:

**Const constructors everywhere possible.** If a widget does not depend on runtime data, make it `const`. Flutter can skip rebuilding const widgets entirely.

**Break large build methods into smaller widgets.** A 200-line `build()` method means the entire tree rebuilds when any state changes. Smaller widgets localize rebuilds.

## 5. Tree Shake Icon Fonts

By default, Flutter includes the entire Material Icons font (1.5MB). If you only use 20 icons, that is a lot of waste. Add `--tree-shake-icons` to your build command.

## Measuring What Matters

Do not optimize blindly. Chrome DevTools Performance and Lighthouse tabs tell you exactly where time is spent. My optimization priority was always:

1. **Network waterfall** — what is downloading and when
2. **JavaScript execution** — what is running and for how long
3. **Render blocking** — what is preventing paint

The numbers do not lie. Profile first, then optimize.
