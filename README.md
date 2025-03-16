# Tailwind CSS v4 Dark Mode Toggle with Astro

A simple example project demonstrating how to implement a dark/light theme toggle using Tailwind CSS v4's new custom variant feature in an Astro project.

![Light and Dark Mode Demo](https://github.com/Michinded/astro5-tailwindcss4-darkmode-toggle/blob/main/public/dark-light-theme.png)

## ✨ Features

- Simple toggle between light and dark themes
- Persists user theme preference using localStorage
- Smooth transitions between themes with CSS transitions
- Uses Tailwind CSS v4's new `@custom-variant` feature
- Respects user's system preference on first visit
- Prevents flash of incorrect theme on page load

## 🛠️ How It Works

The dark mode toggle is implemented using Tailwind CSS v4's custom variants:

```css
/* In global.css */
@import "tailwindcss";
@custom-variant dark (&:where(.dark, .dark *));
```

This allows us to use `dark:` variants in our components:

```html
<div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-white">
  Content that responds to dark mode
</div>
```

JavaScript is used to toggle the `dark` class on the HTML element and store the preference in localStorage:

```js
// Toggle theme on button click
themeToggle.addEventListener("click", () => {
  if (document.documentElement.classList.contains("dark")) {
    document.documentElement.classList.remove("dark");
    localStorage.theme = "light";
  } else {
    document.documentElement.classList.add("dark");
    localStorage.theme = "dark";
  }
});
```

## 🚀 Project Structure

```text
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── ThemeToggle.astro    # Dark mode toggle component
│   │   └── Welcome.astro        # Example component showing usage
│   ├── layouts/
│   │   └── Layout.astro         # Main layout with dark mode initialization
│   ├── pages/
│   │   └── index.astro          # Main page
│   └── styles/
│       └── global.css           # Tailwind config with dark mode setup
└── package.json
```

## 🧩 Installation

### Option 1: Use this repo as a template

1. Clone this repository
   ```bash
   git clone https://github.com/yourusername/tailwindcss4-darkmode-toggle.git
   cd tailwindcss4-darkmode-toggle
   ```

2. Install dependencies
   ```bash
   npm install
   ```

3. Start the development server
   ```bash
   npm run dev
   ```

### Option 2: Add to an existing Astro project

1. Install Tailwind CSS v4 in your Astro project
   ```bash
   npx astro add tailwind
   ```
   
   When prompted, accept installing the dependencies:
   ```
   npm i @tailwindcss/vite@^4.0.14 tailwindcss@^4.0.14
   ```

2. After installation, create or modify your `src/styles/global.css` file:
   ```css
   @import "tailwindcss";
   
   /* Custom variant for dark mode */
   @custom-variant dark (&:where(.dark, .dark *));
   ```

3. Create a `ThemeToggle.astro` component in your components directory:
   ```astro
   ---
   // src/components/ThemeToggle.astro
   ---
   
   <button id="theme-toggle" class="p-2 rounded-lg bg-gray-200 dark:bg-gray-700">
     <span class="dark:hidden">🌙</span>
     <span class="hidden dark:block">☀️</span>
   </button>
   
   <script>
     // Verify saved preference or use system preference
     document.addEventListener("DOMContentLoaded", () => {
       const themeToggle = document.getElementById("theme-toggle");
   
       // Apply saved theme
       document.documentElement.classList.toggle(
         "dark",
         localStorage.theme === "dark" ||
           (!("theme" in localStorage) &&
             window.matchMedia("(prefers-color-scheme: dark)").matches),
       );
   
       // Alter theme on click
       themeToggle?.addEventListener("click", () => {
         if (document.documentElement.classList.contains("dark")) {
           document.documentElement.classList.remove("dark");
           localStorage.theme = "light";
         } else {
           document.documentElement.classList.add("dark");
           localStorage.theme = "dark";
         }
       });
     });
   </script>
   ```

4. Update your layout to import the CSS and prevent flash of incorrect theme:
   ```astro
   ---
   // src/layouts/Layout.astro
   import '../styles/global.css';
   import ThemeToggle from '../components/ThemeToggle.astro';
   ---
   
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width" />
       <title>Your Site Title</title>
       
       <!-- Prevent flash of incorrect theme -->
       <script is:inline>
         // Apply theme immediately
         document.documentElement.classList.toggle(
           "dark",
           localStorage.theme === "dark" || 
           (!("theme" in localStorage) && window.matchMedia("(prefers-color-scheme: dark)").matches)
         );
       </script>
     </head>
     <body class="bg-white dark:bg-gray-900 transition-colors duration-300">
       <header class="p-4 flex justify-between items-center">
         <h1 class="text-xl font-bold text-gray-800 dark:text-white">Your Site</h1>
         <ThemeToggle />
       </header>
       <main>
         <slot />
       </main>
     </body>
   </html>
   ```

5. Now you can use `dark:` variants in your components:
   ```astro
   <div class="bg-white dark:bg-gray-800 p-4 rounded-lg">
     <h2 class="text-gray-900 dark:text-white">This text changes with the theme</h2>
     <p class="text-gray-600 dark:text-gray-300">So does this paragraph.</p>
   </div>
   ```

## 🧞 Commands

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |

## 🎨 Customization

You can customize the theme toggle appearance by modifying the `ThemeToggle.astro` component. The current implementation uses simple sun/moon emoji icons, but you can replace these with custom SVG icons or any other elements.

You can also extend the dark theme by adding more `dark:` variants to your components:

```html
<button class="bg-blue-500 dark:bg-blue-700 hover:bg-blue-600 dark:hover:bg-blue-800 text-white px-4 py-2 rounded">
  Button with dark mode styles
</button>
```

## 📚 Learn More

- [Tailwind CSS v4 Documentation](https://tailwindcss.com/docs)
- [Dark Mode in Tailwind CSS](https://tailwindcss.com/docs/dark-mode)
- [Astro Documentation](https://docs.astro.build)

## 📝 License

MIT

---

Made with ❤️ using [Astro](https://astro.build) and [Tailwind CSS](https://tailwindcss.com).