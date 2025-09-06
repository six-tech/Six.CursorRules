# Astro.js Development Best Practices

This directory contains comprehensive rules for developing modern, performant Astro.js websites with a focus on static generation, minimal JavaScript usage, and optimal performance. Leverage Astro's unique island architecture and multi-framework support for building fast, maintainable web applications.

## 🚀 [Astro Coding Style Guide](astro-coding-style.mdc)

**Use this when you're:**
- Developing Astro.js websites with focus on performance and static generation
- Implementing component development with Astro's island architecture
- Setting up routing, content management, and SEO optimization
- Configuring build processes and deployment strategies
- Integrating multiple frameworks (React, Vue, Svelte) in a single project
- Optimizing for Core Web Vitals and performance metrics

**These rules ensure:**
- ⚡ **Optimal performance** with static generation and minimal JavaScript
- 🏗️ **Proper project structure** following Astro's recommended patterns
- 🧩 **Component composition** using Astro's island architecture and multi-framework support
- 🔄 **Client directives** judiciously applied for partial hydration
- 📄 **Content management** with Markdown/MDX and content collections
- 🎨 **Responsive styling** with scoped CSS and modern CSS features
- 🔍 **SEO optimization** with proper meta tags and canonical URLs
- 🔌 **Framework integration** using Astro's multi-framework capabilities
- 📦 **Build optimization** for production deployment and asset management

## 🎯 Key Astro.js Development Principles

### Island Architecture
- Components are static by default with selective interactivity
- Use client directives (`client:load`, `client:idle`, `client:visible`) strategically
- Minimize JavaScript bundle size through partial hydration

### Performance First
- Prioritize static generation over client-side rendering
- Leverage Astro's built-in optimizations for images and assets
- Implement proper lazy loading and code splitting

### Multi-Framework Support
- Integrate React, Vue, Svelte, and Solid.js components seamlessly
- Choose the right framework for specific use cases
- Maintain consistent development experience across frameworks

## 📋 Quick Reference

| Concern | Best Practice | Key Rule |
|---------|---------------|----------|
| 🏗️ Project Structure | Use recommended Astro structure with src/pages/, src/components/, src/layouts/ | [Project Structure](astro-coding-style.mdc#astro-project-structure) |
| 🧩 Component Development | Prefer .astro files, use TypeScript interfaces, avoid inline JS | [Component Development](astro-coding-style.mdc#component-development) |
| 🔄 Client Hydration | Use `client:idle` for non-critical, `client:visible` for scroll-triggered | [Performance Optimization](astro-coding-style.mdc#performance-optimization) |
| 📄 Content Management | Use Markdown/MDX with frontmatter and content collections | [Content Management](astro-coding-style.mdc#content-management) |
| 🎨 Styling | Scoped styles in .astro files, global styles in layouts | [Styling](astro-coding-style.mdc#styling) |
| 🔍 SEO | Use `<head>` tag and SEO component pattern | [SEO and Meta Tags](astro-coding-style.mdc#seo-and-meta-tags) |
| 🔌 Integrations | Official Astro integrations for better compatibility | [Integrations and Plugins](astro-coding-style.mdc#integrations-and-plugins) |

## 🔗 Integration with Other Rules

- For **TypeScript development**, see [TypeScript Coding Style](../typescript/typescript-coding-style.mdc)
- For **general web development**, combine with project-specific Cursor Rules
- For **documentation standards**, see [Documentation Rules](../../documentation/README.md)
- For **CI/CD integration**, see [CI/CD Rules](../../ci-cd/README.md)

## 📚 Astro.js Resources

- [Astro Official Documentation](https://docs.astro.build/)
- [Astro Islands Architecture](https://docs.astro.build/en/concepts/islands/)
- [Astro Integration Directory](https://astro.build/integrations/)
- [Astro Performance Best Practices](https://docs.astro.build/en/guides/server-side-rendering/#performance)

## 🎯 When to Use Astro.js

Choose Astro.js when you need:

- **Static sites** with dynamic content islands
- **Multi-framework applications** in a single project
- **Performance-critical applications** with minimal JavaScript
- **Content-focused websites** with Markdown/MDX support
- **SEO-optimized applications** with server-side rendering
- **Developer experience** with modern tooling and TypeScript support

Astro excels at delivering fast, SEO-friendly websites while maintaining the flexibility to add interactivity exactly where needed.


