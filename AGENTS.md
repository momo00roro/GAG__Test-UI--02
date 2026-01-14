# AGENTS.md - Development Guidelines for SiteFlow

This file contains build commands, code style guidelines, and conventions for agentic coding agents working in this repository.

## Project Overview

SiteFlow is a modern SaaS landing page built with:
- **HTML5** with semantic markup
- **Tailwind CSS** via CDN for styling
- **Alpine.js** for lightweight interactivity
- **Inter** font family from Google Fonts
- **GSAP** for advanced animations (product/pricing pages)

## Build Commands

This is a static HTML project with no build system. Commands are for development and validation:

### Development
```bash
# Serve locally (any static server)
python -m http.server 8000
# or
npx serve .
# or
live-server --port=3000
```

### Validation
```bash
# HTML validation
npx html-validate *.html

# Accessibility audit
npx axe *.html

# Performance audit
npx lighthouse http://localhost:8000 --chrome-flags="--headless"
```

### No Single Test Command
This project has automated visual testing instead of unit tests. Validate changes by:
1. Visual inspection in browser
2. Responsive testing at breakpoints: 320px, 768px, 1024px, 1440px
3. Accessibility testing with axe DevTools

## Code Style Guidelines

### HTML Structure
- Use semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`)
- Include proper meta tags and viewport settings
- Use `lang="en"` on html element
- Add `scroll-smooth` class to html for smooth scrolling
- Structure content with proper heading hierarchy (h1 → h2 → h3)

### Tailwind CSS Conventions

#### Colors
Use semantic color palette defined in Tailwind config:
```javascript
colors: {
  primary: { 50: '#eff6ff', 500: '#3b82f6', 600: '#2563eb', 700: '#1d4ed8' },
  secondary: { 900: '#0f172a' },
  accent: { 500: '#f97316', 600: '#ea580c' }
}
```

#### Typography
- **Font Family**: Inter (`font-family: 'Inter', sans-serif`)
- **Headings**: Use `font-extrabold` or `font-bold` with tight tracking
- **Body Text**: `text-slate-600` for secondary, `text-slate-900` for primary
- **Line Height**: `leading-relaxed` for body text readability

#### Spacing & Layout
- **Containers**: `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`
- **Section Padding**: `py-24` for major sections, `py-12` for smaller
- **Grid**: Use `grid-cols-1 md:grid-cols-3` for responsive layouts
- **Gaps**: `gap-8` for section spacing, `gap-4` for card spacing

#### Component Patterns
```html
<!-- Cards -->
<div class="bg-white p-8 rounded-2xl shadow-soft hover:shadow-xl transition-shadow border border-slate-100">
  
<!-- Buttons -->
<a href="#" class="inline-flex items-center justify-center px-8 py-4 text-base font-bold text-white transition-all bg-accent-500 rounded-full hover:bg-accent-600 hover:shadow-lg">
  
<!-- Navigation -->
<nav class="fixed w-full z-50 glass-nav transition-all duration-300">
```

### Interactivity (Alpine.js)
- Use `x-data` for component state
- Mobile menu: `x-data="{ mobileMenuOpen: false }"`
- Use `x-show` and `x-collapse` for mobile menu animations
- Add `x-cloak` and CSS `[x-cloak] { display: none !important; }`

### Custom CSS
Keep custom CSS minimal and organized:
```css
/* Glass morphism effect */
.glass-nav {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(226, 232, 240, 0.8);
}

/* Custom shadows */
.shadow-soft {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.02), 0 2px 4px -1px rgba(0, 0, 0, 0.02);
}

.shadow-glow {
  box-shadow: 0 0 20px rgba(37, 99, 235, 0.15);
}
```

### Animation Guidelines
- **Transitions**: Use `transition-all duration-300` for smooth state changes
- **Hover Effects**: Add `transform hover:-translate-y-1` for lift effect
- **Loading States**: Use `animate-pulse` for skeleton loading
- **GSAP**: Only use for complex scroll-based animations (product/pricing pages)

### Responsive Design
- **Mobile-First**: Base styles for mobile, add `md:`, `lg:` breakpoints
- **Breakpoints**: 640px (sm), 768px (md), 1024px (lg), 1280px (xl)
- **Touch Targets**: Minimum `min-h-[44px] min-w-[44px]` for mobile buttons
- **Text Scaling**: `text-4xl lg:text-7xl` for hero headings

### Accessibility Standards
- **Contrast**: Minimum 4.5:1 for normal text, 3:1 for large text
- **Focus States**: Always include `focus:ring-2 focus:ring-primary-500`
- **Screen Readers**: Use `sr-only` class for hidden labels
- **Keyboard Navigation**: Ensure all interactive elements are keyboard accessible
- **Reduced Motion**: Add `motion-reduce:animate-none` for motion-sensitive users

### Performance Optimization
- **CDN Resources**: Use Tailwind, Alpine.js, and Google Fonts via CDN
- **Image Optimization**: Add `loading="lazy"` to below-fold images
- **Font Loading**: Use `font-display: swap` via Google Fonts
- **CSS Purging**: Not needed with CDN approach, but keep classes minimal

### File Organization
```
/
├── index.html          # Main landing page
├── product.html        # Product features page
├── pricing.html        # Pricing plans page
├── solutions.html      # Solutions page
├── enterprise.html     # Enterprise page
├── assets/             # Images and static assets
└── .shared/           # UI/UX workflow tools
```

### Naming Conventions
- **Files**: kebab-case (e.g., `product.html`, `pricing.html`)
- **CSS Classes**: Use Tailwind utilities, custom classes use kebab-case
- **IDs**: Use for Alpine.js targets, kebab-case format
- **Images**: Descriptive names with underscores (e.g., `product_hero_bg.png`)

### Error Handling
- **404 Page**: Custom `404.html` with navigation back to site
- **Broken Links**: Check all internal links point to valid pages
- **Image Fallbacks**: Use proper alt text and aspect ratios

### Git Workflow
- **Branch Naming**: `feature/feature-name` or `fix/issue-name`
- **Commit Messages**: Use conventional commits (`feat:`, `fix:`, `docs:`)
- **File Changes**: Review HTML changes for semantic correctness
- **Visual Testing**: Required for all UI changes

## UI/UX Workflow Integration

This project includes the UI/UX Pro Max workflow for design decisions:

```bash
# Search for design guidance
python3 .shared/ui-ux-pro-max/scripts/search.py "keyword" --domain domain

# Available domains: product, style, typography, color, landing, chart, ux, prompt
# Default stack: html-tailwind
```

Use this workflow when:
- Adding new pages or sections
- Choosing color palettes or typography
- Implementing responsive layouts
- Following accessibility best practices

## Quality Checklist

Before delivering changes, verify:
- [ ] Semantic HTML5 structure
- [ ] Responsive at all breakpoints
- [ ] Accessibility compliance (axe testing)
- [ ] Proper focus states and keyboard navigation
- [ ] Consistent spacing and typography
- [ ] Smooth transitions and hover states
- [ ] Image optimization and lazy loading
- [ ] Cross-browser compatibility (Chrome, Firefox, Safari)
- [ ] Mobile touch target sizing
- [ ] Performance optimization (Lighthouse score > 90)