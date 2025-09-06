---
description: This file provides guidelines for developing Astro.js websites with best practices for component development, routing, content management, and performance optimization.
globs: *.js, *.ts, *.md, *.mdx, *.css, *.astro
alwaysApply: false
---

# Cursor Rules File: Best Practices for developing Astro.js websites

**Role Definition:**
- Javascript/Typescript Developer
- Astro.js Developer
- Starlight Developer (documentation library for Astro.js)
- Front-end Engineer
- UI/UX Designer

## General

### Description

Astro.js websites must be developed with a focus on performance, static generation, and minimal JavaScript usage. This
guide provides comprehensive best practices for building modern, fast, and maintainable Astro.js applications using the
framework's unique features like partial hydration, island architecture, and multi-framework support.

### Requirements

- Write concise, technical responses with accurate Astro examples
- Leverage Astro's partial hydration and multi-framework support effectively
- Prioritize static generation and minimal JavaScript for optimal performance
- Use descriptive variable names and follow Astro's naming conventions
- Organize files using Astro's file-based routing system

## Astro Project Structure

- Use the recommended Astro project structure:

```
  - src/
    - components/
    - layouts/
    - pages/
    - styles/
  - public/
  - astro.config.mjs
```

## Component Development

- Create `.astro` files for Astro components.
- Use framework-specific components (React, Vue, Svelte, Solid.js) when necessary.
- Implement proper component composition and reusability.
- Use Astro's component props for data passing.
- Leverage Astro's built-in components like `<Markdown />` when appropriate.

### Component Best Practices

```astro
---
// Good: Proper component structure with TypeScript
export interface Props {
  title: string;
  description?: string;
}

const { title, description } = Astro.props;
---

<div class="card">
  <h2>{title}</h2>
  {description && <p>{description}</p>}
</div>

<style>
  .card {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 1rem;
  }
</style>
```

```astro
---
// Avoid: Inline JavaScript in templates
const items = ['apple', 'banana', 'cherry'];
---

<ul>
  {items.map(item => `<li>${item}</li>`).join('')} <!-- Don't do this -->
</ul>
```

```astro
---
// Good: Use proper JSX expressions
const items = ['apple', 'banana', 'cherry'];
---

<ul>
  {items.map(item => <li>{item}</li>)}
</ul>
```

## Routing and Pages

- Use Astro's file-based routing system in the `src/pages/` directory.
- Implement dynamic routes using `[...slug].astro` syntax.
- Use `getStaticPaths()` for generating static pages with dynamic routes.
- Implement proper 404 handling with a `404.astro` page.

## Content Management

- Use Markdown `.md` or MDX `.mdx` files for content-heavy pages.
- Leverage Astro's built-in support for frontmatter in Markdown files.
- Implement content collections for organized content management.

## Styling

- Use Astro's scoped styling with `<style>` tags in `.astro` files.
- Leverage global styles when necessary, importing them in layouts.
- Leverage Astro's built-in CSS variables for theming and styling.
- Implement responsive design using CSS custom properties and media queries.

## Performance Optimization

- Minimize use of client-side JavaScript; leverage Astro's static generation.
- Use the client:* directives judiciously for partial hydration:
  - client:load for immediately necessary interactivity
  - client:idle for non-critical interactivity
  - client:visible for components that should hydrate when visible
- Implement proper lazy loading for images and other assets.
- Use Astro's built-in asset optimization features.

### Client Directive Usage

```astro
---
// Good: Use client:idle for non-critical interactive components
---

<div>
  <button>Static Button</button>
  <InteractiveCounter client:idle />
</div>
```

```astro
---
// Good: Use client:visible for components that appear on scroll
---

<div>
  <HeavyChart client:visible />
</div>
```

```astro
---
// Avoid: Overusing client:load (increases initial bundle size)
---

<div>
  <StaticHeader client:load /> <!-- Unnecessary hydration -->
  <StaticFooter client:load /> <!-- Unnecessary hydration -->
</div>
```

## Data Fetching

- Use `Astro.props` for passing data to components.
- Implement `getStaticPaths()` for fetching data at build time.
- Use `Astro.glob()` for working with local files efficiently.
- Implement proper error handling for data fetching operations.

## SEO and Meta Tags

- Use Astro's `<head>` tag for adding meta information.
- Implement canonical URLs for proper SEO.
- Use the `<SEO>` component pattern for reusable SEO setups.

## Integrations and Plugins

- Use Astro integrations for extending functionality (e.g., `@astrojs/image`).
- Implement the proper configuration for integrations in astro.config.mjs.
- Use Astro's official integrations when available for better compatibility.

## Build and Deployment

- Optimize the build process using Astro's build command.
- Implement proper environment variable handling for different environments.
- Implement proper CI/CD pipelines for automated builds and deployments.

## Styling with Tailwind CSS (OPTIONAL - if the user asks for it)

- Integrate Tailwind CSS with Astro `@astrojs/tailwind`

### Tailwind CSS Best Practices

- Use Tailwind utility classes extensively in your Astro components.
- Leverage Tailwind's responsive design utilities (sm:, md:, lg:, etc.).
- Use Tailwind's color palette and spacing scale for consistency.
- Implement custom theme extensions in `tailwind.config.cjs` when necessary.
- Never use the `@apply` directive

## Accessibility

- Ensure proper semantic `HTML` structure in Astro components.
- Implement `ARIA attributes` where necessary.
- Ensure keyboard navigation support for interactive elements.

## Key Conventions

1. Follow Astro's Style Guide for consistent code formatting.
2. Use TypeScript for enhanced type safety and developer experience.
3. Implement proper error handling and logging.
4. Leverage Astro's RSS feed generation for content-heavy sites.
5. Use Astro's Image component for optimized image delivery.

## Performance Metrics

- Prioritize Core Web Vitals (LCP, FID, CLS) in development.
- Use Lighthouse and WebPageTest for performance auditing.
- Implement performance budgets and monitoring.

## Other Resources

Refer to Astro's official documentation for detailed information on components, routing, and integrations for best
practices.

# End of Cursor Rules File