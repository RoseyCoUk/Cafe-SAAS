# Tailwind CSS Documentation

## Overview
Tailwind CSS is a utility-first CSS framework for rapidly building custom user interfaces. It works by scanning all of your HTML files, JavaScript components, and any other templates for class names, generating the corresponding styles and then writing them to a static CSS file.

## Core Concepts

### Utility-First Approach
Tailwind promotes a utility-first methodology where you build complex components by composing small, single-purpose utility classes directly in your markup.

```html
<!-- Traditional CSS approach -->
<div class="card">
  <h2 class="card-title">Title</h2>
  <p class="card-content">Content</p>
</div>

<!-- Tailwind utility-first approach -->
<div class="max-w-sm mx-auto bg-white rounded-xl shadow-lg p-6">
  <h2 class="text-xl font-semibold text-gray-900">Title</h2>
  <p class="text-gray-600 mt-2">Content</p>
</div>
```

### Responsive Design
Tailwind uses a mobile-first breakpoint system with prefixes to apply styles at different screen sizes:

```html
<!-- Responsive width: 16 on mobile, 32 on medium screens, 48 on large screens -->
<img class="w-16 md:w-32 lg:w-48" src="..." />

<!-- Responsive grid: 2 columns on mobile, 3 on small screens and up -->
<div class="grid grid-cols-2 sm:grid-cols-3">
  <!-- ... -->
</div>
```

#### Default Breakpoints
- `sm`: 640px (40rem)
- `md`: 768px (48rem)
- `lg`: 1024px (64rem)
- `xl`: 1280px (80rem)
- `2xl`: 1536px (96rem)

#### Breakpoint Ranges
You can apply styles within specific breakpoint ranges:

```html
<!-- Apply flex only between md and xl breakpoints -->
<div class="md:max-xl:flex">
  <!-- ... -->
</div>
```

### Dark Mode
Tailwind supports dark mode using the `dark:` variant:

```html
<div class="bg-white dark:bg-slate-800">
  <h1 class="text-black dark:text-white">Hello World</h1>
</div>
```

## Building Components

### Simple Component Example
Here's a complete card component built with Tailwind utilities:

```jsx
<div className="mx-auto flex max-w-sm items-center gap-x-4 rounded-xl bg-white p-6 shadow-lg outline outline-black/5 dark:bg-slate-800 dark:shadow-none dark:-outline-offset-1 dark:outline-white/10">
  <img className="size-12 shrink-0" src="/logo.svg" alt="Logo" />
  <div>
    <div className="text-xl font-medium text-black dark:text-white">ChitChat</div>
    <p className="text-gray-500 dark:text-gray-400">You have a new message!</p>
  </div>
</div>
```

### Complex Responsive Component
Building a responsive profile card:

```jsx
<div className="mx-auto max-w-sm space-y-2 rounded-xl bg-white px-8 py-8 shadow-lg ring ring-black/5 @sm:flex @sm:items-center @sm:space-y-0 @sm:gap-x-6 @sm:py-4">
  <img
    className="mx-auto block h-24 rounded-full @sm:mx-0 @sm:shrink-0"
    src="/avatar.jpg"
    alt="Profile"
  />
  <div className="space-y-2 text-center @sm:text-left">
    <div className="space-y-0.5">
      <p className="text-lg font-semibold text-black">Erin Lindford</p>
      <p className="font-medium text-gray-500">Product Engineer</p>
    </div>
    <button className="rounded-full border border-purple-200 px-4 py-1 text-sm font-semibold text-purple-600 hover:border-transparent hover:bg-purple-600 hover:text-white active:bg-purple-700">
      Message
    </button>
  </div>
</div>
```

## Advanced Features

### Arbitrary Values
Use square brackets to apply custom values not in your theme:

```html
<!-- Custom background color -->
<button class="bg-[#316ff6]">Sign in with Facebook</button>

<!-- Complex grid layout -->
<div class="grid grid-cols-[24rem_2.5rem_minmax(0,1fr)]">
  <!-- ... -->
</div>

<!-- CSS calc() functions -->
<div class="max-h-[calc(100dvh-(--spacing(6)))]">
  <!-- ... -->
</div>

<!-- CSS variables -->
<div class="[--gutter-width:1rem] lg:[--gutter-width:2rem]">
  <!-- ... -->
</div>
```

### Dynamic Class Generation
Tailwind can detect and generate classes from dynamic strings:

```jsx
export default function Button({ size, children }) {
  let sizeClasses = {
    md: "px-4 py-2 rounded-md text-base",
    lg: "px-5 py-3 rounded-lg text-lg",
  }[size];

  return (
    <button type="button" className={`font-bold ${sizeClasses}`}>
      {children}
    </button>
  );
}
```

### CSS Variables with Tailwind
Set and use CSS variables dynamically:

```jsx
export function BrandedButton({ buttonColor, buttonColorHover, textColor, children }) {
  return (
    <button
      style={{
        "--bg-color": buttonColor,
        "--bg-color-hover": buttonColorHover,
        "--text-color": textColor,
      }}
      className="bg-(--bg-color) text-(--text-color) hover:bg-(--bg-color-hover) ..."
    >
      {children}
    </button>
  );
}
```

## Customization

### Custom Breakpoints
Define custom breakpoints in your CSS:

```css
@import "tailwindcss";

@theme {
  --breakpoint-xs: 30rem;
  --breakpoint-2xl: 100rem;
  --breakpoint-3xl: 120rem;
}
```

```html
<div class="grid xs:grid-cols-2 3xl:grid-cols-6">
  <!-- ... -->
</div>
```

### Custom Container Sizes
Add custom container sizes:

```css
@import "tailwindcss";

@theme {
  --container-8xl: 96rem;
}
```

```html
<div class="@container">
  <div class="flex flex-col @8xl:flex-row">
    <!-- ... -->
  </div>
</div>
```

### Custom Components with @layer
Create reusable component classes:

```css
@import "tailwindcss";

@layer components {
  .btn-primary {
    border-radius: calc(infinity * 1px);
    background-color: var(--color-violet-500);
    padding-inline: --spacing(5);
    padding-block: --spacing(2);
    font-weight: var(--font-weight-semibold);
    color: var(--color-white);
    box-shadow: var(--shadow-md);
    &:hover {
      @media (hover: hover) {
        background-color: var(--color-violet-700);
      }
    }
  }
}
```

### Custom Utilities
Add custom utility classes:

```css
@utility tab-4 {
  tab-size: 4;
}
```

## State Variants

### Hover, Focus, and Active States
```html
<button class="bg-blue-500 hover:bg-blue-700 focus:ring-4 focus:ring-blue-300 active:bg-blue-800">
  Button
</button>
```

### Form States
```html
<input class="border-gray-300 focus:border-blue-500 focus:ring-blue-500 invalid:border-red-500" />
```

### Group Hover
```html
<div class="group">
  <img class="group-hover:opacity-75" />
  <h3 class="group-hover:text-blue-600">Title</h3>
</div>
```

## Layout Examples

### Flexbox Layout
```html
<div class="flex items-center justify-between p-4">
  <div class="flex items-center space-x-4">
    <img class="w-10 h-10 rounded-full" />
    <div>
      <h4 class="font-semibold">John Doe</h4>
      <p class="text-gray-500">Developer</p>
    </div>
  </div>
  <button class="px-4 py-2 bg-blue-500 text-white rounded">
    Follow
  </button>
</div>
```

### Grid Layout
```html
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  <div class="bg-white p-6 rounded-lg shadow">Card 1</div>
  <div class="bg-white p-6 rounded-lg shadow">Card 2</div>
  <div class="bg-white p-6 rounded-lg shadow">Card 3</div>
</div>
```

### Avatar List Example
```jsx
<div className="bg-white">
  <div className="mx-auto w-72 px-8 py-6 sm:w-96 sm:px-12 sm:py-8">
    <div className="flex items-center space-x-2 text-base">
      <h4 className="text-base font-semibold text-slate-900">Contributors</h4>
      <span className="rounded-full bg-slate-100 px-2 py-1 text-xs font-semibold text-slate-700">204</span>
    </div>
    <div className="mt-3 flex -space-x-2 overflow-hidden">
      <img className="inline-block h-12 w-12 rounded-full ring-2 ring-white" src="/avatar1.jpg" alt="" />
      <img className="inline-block h-12 w-12 rounded-full ring-2 ring-white" src="/avatar2.jpg" alt="" />
      <img className="inline-block h-12 w-12 rounded-full ring-2 ring-white" src="/avatar3.jpg" alt="" />
      <img className="inline-block h-12 w-12 rounded-full ring-2 ring-white" src="/avatar4.jpg" alt="" />
      <img className="inline-block h-12 w-12 rounded-full ring-2 ring-white" src="/avatar5.jpg" alt="" />
    </div>
    <div className="mt-3 text-sm font-medium">
      <a href="#" className="text-blue-500">+ 198 others</a>
    </div>
  </div>
</div>
```

## Common Utility Classes

### Spacing
- Padding: `p-4`, `px-2`, `py-3`, `pt-1`, `pr-2`, `pb-3`, `pl-4`
- Margin: `m-4`, `mx-auto`, `my-2`, `mt-4`, `mr-2`, `mb-3`, `ml-1`

### Typography
- Font size: `text-xs`, `text-sm`, `text-base`, `text-lg`, `text-xl`, `text-2xl`
- Font weight: `font-thin`, `font-normal`, `font-medium`, `font-semibold`, `font-bold`
- Text color: `text-gray-500`, `text-blue-600`, `text-red-500`
- Text alignment: `text-left`, `text-center`, `text-right`

### Colors
- Background: `bg-white`, `bg-gray-100`, `bg-blue-500`
- Text: `text-black`, `text-white`, `text-gray-600`
- Border: `border-gray-300`, `border-blue-500`

### Layout
- Display: `block`, `inline`, `flex`, `grid`, `hidden`
- Position: `relative`, `absolute`, `fixed`, `sticky`
- Flexbox: `flex-col`, `flex-row`, `items-center`, `justify-between`
- Grid: `grid-cols-3`, `gap-4`, `col-span-2`

### Sizing
- Width: `w-full`, `w-1/2`, `w-64`, `w-auto`
- Height: `h-full`, `h-screen`, `h-64`, `h-auto`
- Max width: `max-w-sm`, `max-w-md`, `max-w-lg`, `max-w-xl`

### Border and Shadow
- Border: `border`, `border-2`, `rounded`, `rounded-lg`, `rounded-full`
- Shadow: `shadow-sm`, `shadow`, `shadow-lg`, `shadow-xl`

### Dynamic Viewport Units
- Height: `h-svh` (100svh), `h-lvh` (100lvh), `h-dvh` (100dvh)
- Min height: `min-h-svh`, `min-h-lvh`, `min-h-dvh`

## Best Practices

### 1. Component Composition
Build complex components by composing simple utilities:

```html
<!-- Good: Composed utilities -->
<div class="flex items-center space-x-4 p-4 bg-white rounded-lg shadow">
  <!-- content -->
</div>

<!-- Avoid: Overly specific component classes -->
<div class="marketing-card">
  <!-- content -->
</div>
```

### 2. Responsive Design
Start with mobile and add larger breakpoints:

```html
<!-- Mobile first, then tablet and desktop -->
<div class="text-center sm:text-left lg:text-right">
  <!-- content -->
</div>
```

### 3. Semantic HTML
Use proper HTML elements with Tailwind styling:

```html
<!-- Good: Semantic button -->
<button class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
  Click me
</button>

<!-- Avoid: Div styled as button -->
<div class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 cursor-pointer">
  Click me
</div>
```

### 4. Consistent Spacing
Use a consistent spacing scale:

```html
<!-- Good: Consistent spacing -->
<div class="space-y-4">
  <div class="p-4">Item 1</div>
  <div class="p-4">Item 2</div>
</div>
```

### 5. Color System
Use a consistent color palette:

```html
<!-- Good: Consistent color usage -->
<div class="bg-blue-50 border border-blue-200 text-blue-900">
  <!-- content -->
</div>
```

## Plugin System

### Adding Responsive Variants to Components
```javascript
// Using array shorthand
plugin(function ({ addComponents }) {
  addComponents({
    '.card': {
      // styles
    }
  }, ['responsive'])
})

// Using object syntax
plugin(function ({ addComponents }) {
  addComponents({
    '.card': {
      // styles
    }
  }, {
    variants: ['responsive']
  })
})
```

### Responsive Components in CSS
```css
@layer components {
  @responsive {
    .card {
      /* styles */
    }
  }
}
```

## Performance Tips

1. **Purging**: Tailwind automatically removes unused styles in production
2. **Class Detection**: Ensure class names are complete strings for proper detection
3. **Component Extraction**: Use `@layer components` for frequently repeated patterns
4. **JIT Mode**: Just-in-time compilation generates styles on-demand

## Migration and Upgrades

When upgrading Tailwind CSS:
1. Check the changelog for breaking changes
2. Update your configuration file if needed
3. Test responsive breakpoints
4. Verify custom components still work
5. Update any custom plugins

---

*This documentation covers the essential concepts and patterns for working with Tailwind CSS. For the most up-to-date information, always refer to the official [Tailwind CSS documentation](https://tailwindcss.com).*