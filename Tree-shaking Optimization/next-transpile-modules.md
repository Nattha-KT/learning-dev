# next-transpile-modules

`next-transpile-modules` is a popular Next.js plugin used to transpile (or compile) external npm packages or monorepo packages that are written in modern JavaScript or TypeScript, especially if they are not pre-transpiled to ES5 or CommonJS. This plugin is particularly useful for working with monorepos or when integrating third-party libraries that use ES6+ syntax or TypeScript but aren't already transpiled, as Next.js typically skips transpiling dependencies in `node_modules` by default.

### Why Use `next-transpile-modules`?

1. **ES6+ and TypeScript in Packages**: Some packages in `node_modules` may contain modern JavaScript syntax (like ES6 modules or JSX) that Next.js can't understand directly, which causes build errors.
2. **Monorepo Projects**: In monorepo setups (like with Turborepo or Lerna), you often share local packages across multiple projects. `next-transpile-modules` allows these shared packages to be transpiled by Next.js so they work seamlessly across the monorepo.
3. **Tree-shaking Optimization**: Ensuring all code follows a consistent module format (like ES modules) can help improve tree-shaking and reduce bundle sizes.

### How to Use `next-transpile-modules`

1. **Install the Plugin**:
   ```bash
   yarn add next-transpile-modules
   # or
   npm install next-transpile-modules
   ```

2. **Configure in `next.config.js`**:
   Import and set up `next-transpile-modules` in your Next.js configuration file (`next.config.js`). You’ll specify the modules you want to transpile as an array.

   ```javascript
   const withTM = require('next-transpile-modules')([
     'your-module-1', 
     'your-module-2',
     // Add any other modules you need to transpile
   ]);

   module.exports = withTM({
     // any additional Next.js configuration
   });
   ```

3. **Run the Next.js App**:
   Start or build your Next.js application as usual. The specified modules will now be transpiled and compatible with your project.

This plugin helps solve compatibility issues when integrating modern JavaScript libraries, enhancing support for monorepo workflows, and ensuring that dependencies match Next.js’s module requirements.
