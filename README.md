# Astro 6 Font API Demo

Comprehensive examples demonstrating Astro 6's Font API with various font loading scenarios.

## 🎯 Purpose

This demo project showcases the Astro Font API with real-world examples for video tutorials and documentation.

## 📚 Examples Included

### 1. **Google Fonts** (`/google-fonts`)
- Basic Google Font loading
- Specific weight selection
- Multiple weights and styles
- Examples: Inter, Playfair Display, Roboto Mono

### 2. **Local Fonts** (`/local-fonts`)
- Self-hosted font files
- Multiple weight variants
- Benefits of local hosting
- Examples: Ubuntu font family

### 3. **Custom Configuration** (`/custom-config`)
- `display: 'swap'` for better performance
- `preload: true` for critical fonts
- `subset` for optimized downloads
- `fallback` font stacks
- Examples: Lora, Poppins, Noto Sans, Merriweather

### 4. **Variable Fonts** (`/variable-fonts`)
- Full weight range support
- Fine-tuned typography control
- Multi-axis variable fonts
- Interactive weight slider demo
- Examples: Inter Variable, Recursive

### 5. **Font Fallbacks** (`/fallbacks`)
- System UI fallback stacks
- Metrics-matched fallbacks
- Best practices for minimizing layout shift
- Common fallback patterns
- Examples: Montserrat, Source Serif 4

## 🚀 Getting Started

```bash
# Install dependencies
npm install

# Start dev server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## 📖 Font API Basics

```typescript
import { font } from 'astro:font';

// Basic usage
const myFont = font({
  family: 'Inter',
  source: 'google',
});

// With options
const customFont = font({
  family: 'Montserrat',
  source: 'google',
  weight: [400, 700],
  display: 'swap',
  preload: true,
  subset: ['latin'],
  fallback: ['system-ui', 'sans-serif'],
});

// Use in templates
<p class:list={[myFont.class]}>Styled text</p>
```

## 🎨 Key Features Demonstrated

- ✅ Multiple font sources (Google, local files)
- ✅ Weight and style variations
- ✅ Performance optimizations (preload, display, subset)
- ✅ Variable font support
- ✅ Fallback strategies
- ✅ Interactive examples
- ✅ Code snippets for each pattern

## 📁 Project Structure

```
/
├── public/
│   └── fonts/           # Local font files
│       ├── Ubuntu-Regular.ttf
│       └── Ubuntu-Bold.ttf
├── src/
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       ├── index.astro           # Home / Navigation
│       ├── google-fonts.astro    # Google Fonts examples
│       ├── local-fonts.astro     # Local fonts examples
│       ├── custom-config.astro   # Configuration options
│       ├── variable-fonts.astro  # Variable fonts
│       └── fallbacks.astro       # Fallback strategies
└── README.md
```

## 🎥 Video Tutorial Use

Each page is designed to be screen-recorded for tutorial purposes:
- Clear code examples
- Visual demonstrations
- Real-world use cases
- Performance considerations

## 🔗 Resources

- [Astro Font API Documentation](https://docs.astro.build/en/guides/fonts/)
- [Google Fonts](https://fonts.google.com/)
- [Variable Fonts Guide](https://web.dev/variable-fonts/)

## 📝 Notes

- All examples use Astro 6+ Font API
- Fonts are optimized for performance
- Code is production-ready
- Demonstrates best practices

## 🛠️ Tech Stack

- Astro 6
- TypeScript (strict mode)
- Google Fonts
- Local font files

---

Created for CIP (Coding in Public) video tutorial on Astro Font API.
