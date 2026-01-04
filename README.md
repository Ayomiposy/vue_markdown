# Vue Markdown Editor

A real-time markdown editor built with Vue 3, TypeScript, and Tailwind CSS. Features a split-pane interface with live preview, syntax sanitization, and a real-time clock display.

## 📸 Preview 2

![Markdown Editor Preview](./screenshots/app-preview.png)

![Vue](https://img.shields.io/badge/Vue-3.x-4FC08D?logo=vue.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.x-38B2AC?logo=tailwind-css&logoColor=white)

## 📸 Preview 2

![Markdown Editor Preview](./screenshots/app-preview2.png)

## ✨ Features

- **Real-time Preview**: See your markdown rendered instantly as you type
- **Live Clock**: Real-time clock display in the preview header
- **Split-Pane Interface**: Side-by-side editor and preview
- **Secure Rendering**: XSS protection with DOMPurify sanitization
- **Beautiful Typography**: Powered by Tailwind Typography plugin
- **Type-Safe**: Built with TypeScript for better developer experience
- **Responsive Design**: Clean, modern UI with Tailwind CSS

## 🛠️ Tech Stack

- **Vue 3** - Progressive JavaScript framework with Composition API
- **TypeScript** - Type-safe development
- **Vite** - Next-generation frontend tooling
- **Tailwind CSS** - Utility-first CSS framework
- **marked** - Markdown parser and compiler
- **DOMPurify** - XSS sanitizer for HTML
- **Bun** - Fast JavaScript runtime and package manager

## 📋 Prerequisites

Before you begin, ensure you have installed:

- [Bun](https://bun.sh/) (v1.0 or higher)
- Node.js (v18 or higher) - optional, if not using Bun

## 🚀 Getting Started

### Installation

1. **Clone the repository**

```bash
git clone <your-repo-url>
cd vue-markdown-editor
```

2. **Install dependencies**

```bash
bun install
```

3. **Start the development server**

```bash
bun run dev
```

4. **Open your browser**
   Navigate to `http://localhost:5173` (or the port shown in your terminal)

### Build for Production

```bash
bun run build
```

The built files will be in the `dist` directory.

### Preview Production Build

```bash
bun run preview
```

## 📁 Project Structure

```
vue-markdown-editor/
├── src/
│   ├── assets/
│   │   └── main.css              # Tailwind CSS imports
│   ├── components/
│   │   ├── Editor.vue            # Markdown editor component
│   │   └── Preview.vue           # Markdown preview component
│   ├── App.vue                   # Root component
│   └── main.ts                   # Application entry point
├── public/                       # Static assets
├── index.html                    # HTML entry point
├── package.json                  # Project dependencies
├── tailwind.config.js            # Tailwind configuration
├── postcss.config.js             # PostCSS configuration
├── tsconfig.json                 # TypeScript configuration
└── vite.config.ts                # Vite configuration
```

## 🧩 Core Components

### App.vue

The root component that orchestrates the application layout:

- **State Management**: Uses Vue's `ref` to create reactive markdown text state
- **Two-Way Binding**: Implements `v-model` for seamless data flow between editor and preview
- **Layout**: Split-pane design with 50/50 width distribution

```typescript
const markedText = ref(""); // Reactive state for markdown content
```

### Editor.vue

The markdown input component:

- **Props**: Receives `modelValue` (the current markdown text)
- **Events**: Emits `update:modelValue` when text changes
- **Features**:
  - Full-height textarea with no outline on focus
  - Real-time input handling with TypeScript type safety
  - Clean, distraction-free writing interface

**Key Concept - v-model Implementation**:

```typescript
// Props define what data comes in
const props = defineProps<{ modelValue: string }>();

// Emits define what data goes out
const emit = defineEmits<{
  (e: "update:modelValue", value: string): void;
}>();
```

This pattern enables Vue's `v-model` directive to work seamlessly:

```vue
<Editor v-model="markedText" />
<!-- Equivalent to: -->
<Editor :modelValue="markedText" @update:modelValue="markedText = $event" />
```

### Preview.vue

The markdown rendering component:

- **Props**: Receives `markedText` (the markdown to render)
- **Computed Properties**: Uses Vue's `computed` for efficient re-rendering
- **Security**: Sanitizes HTML with DOMPurify to prevent XSS attacks
- **Real-time Clock**: Updates every second using `setInterval`
- **Lifecycle Hooks**: Properly manages interval cleanup to prevent memory leaks

**Key Concepts**:

1. **Computed Property for Efficient Rendering**:

```typescript
const renderedMarkdown = computed(() => {
  const rawHtml = marked.parse(props.markedText);
  return DOMPurify.sanitize(rawHtml as string);
});
```

- Only recalculates when `markedText` changes
- Caches the result for better performance

2. **Lifecycle Management**:

```typescript
onMounted(() => {
  interval = setInterval(() => {
    time.value = new Date().toLocaleTimeString();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(interval); // Prevent memory leaks
});
```

3. **Security with DOMPurify**:

- Prevents XSS attacks by sanitizing HTML
- Allows safe rendering of user-generated markdown

## 🎨 Styling

The project uses Tailwind CSS with the Typography plugin:

- **Utility Classes**: Responsive, mobile-first design
- **Typography Plugin**: Beautiful markdown rendering with prose classes
- **Custom Colors**: Blue theme for headers and accents
- **No Custom CSS**: Everything styled with Tailwind utilities

## 🔒 Security

- **XSS Protection**: All rendered HTML is sanitized with DOMPurify
- **Type Safety**: TypeScript catches errors at compile-time
- **Safe HTML Rendering**: Vue's `v-html` used only with sanitized content

## 🎯 Key Concepts Explained

### 1. Reactive State with `ref`

```typescript
const markedText = ref("");
```

Creates a reactive variable that Vue tracks. When it changes, all dependent components automatically update.

### 2. Two-Way Data Binding with `v-model`

```vue
<Editor v-model="markedText" />
```

Shorthand for passing data down (`:modelValue`) and listening for updates (`@update:modelValue`).

### 3. Computed Properties

```typescript
const renderedMarkdown = computed(() => {
  /* ... */
});
```

Cached reactive values that only recalculate when dependencies change - more efficient than methods.

### 4. Props and Emits (Component Communication)

- **Props**: Data flows down from parent to child
- **Emits**: Events bubble up from child to parent

### 5. Lifecycle Hooks

- `onMounted()`: Runs after component is added to DOM
- `onUnmounted()`: Runs before component is removed (cleanup)

### 6. Type Safety with TypeScript

```typescript
defineProps<{ markedText: string }>();
```

Ensures components receive the correct data types, catching errors early.

## 🧪 How It Works

1. User types in the **Editor** component
2. Input event triggers and emits new value via `update:modelValue`
3. **App.vue** updates the `markedText` ref
4. Vue's reactivity system detects the change
5. **Preview** component receives new prop value
6. Computed property recalculates:
   - Parses markdown to HTML with `marked`
   - Sanitizes HTML with `DOMPurify`
7. DOM updates with new rendered content

## 🤝 Contributing

Contributions are welcome! Feel free to:

- Report bugs
- Suggest new features
- Submit pull requests

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- [Vue.js](https://vuejs.org/) - The Progressive JavaScript Framework
- [marked](https://marked.js.org/) - Markdown parser
- [DOMPurify](https://github.com/cure53/DOMPurify) - XSS sanitizer
- [Tailwind CSS](https://tailwindcss.com/) - Utility-first CSS framework
- [Tailwind Typography](https://tailwindcss.com/docs/typography-plugin) - Beautiful typography

## 📞 Support

If you have any questions or need help, please open an issue in the repository.

---

**Happy Markdown Editing! ✍️**
