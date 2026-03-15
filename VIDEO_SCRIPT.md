# Astro 6 Font API - Video Script Outline

## Introduction (2-3 min)

### Hook: The Font Loading Problem
**Story to tell:**
"You're building a beautiful website. You find the perfect Google Font. You copy the `<link>` tag into your HTML. Ship it. Then you start noticing issues..."

**Pain points to mention:**
- ❌ Flash of Invisible Text (FOIT) - users see blank pages while fonts load
- ❌ Flash of Unstyled Text (FOUT) - content jumps around when fonts swap in
- ❌ Layout shift - poor Core Web Vitals scores
- ❌ Loading fonts you don't need - every weight, every style, every character
- ❌ Slower load times - especially on mobile/slow connections
- ❌ Privacy concerns - Google tracking every visitor
- ❌ Manual optimization - you have to remember to preload, subset, etc.
- ❌ No type safety - typos in font names = broken styles

**The transition:**
"Astro 6 introduces the Font API to solve ALL of these problems. Let me show you how."

### What We'll Cover
- Google Fonts (the easy wins)
- Local fonts (for privacy and control)
- Advanced configuration (performance tuning)
- Variable fonts (the future)
- Fallback strategies (minimizing layout shift)

---

## 1. Google Fonts (5-7 min)

### The Old Way vs The New Way

**The Old Way (pain):**
```html
<!-- Manual approach - easy to mess up -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" rel="stylesheet">
```

**Problems with manual approach:**
- You have to remember preconnect hints
- You have to manually specify weights
- You have to remember `display=swap`
- You have to add the font-family CSS yourself
- No optimization - loads entire character sets
- Hard to maintain - copying link tags everywhere

**The New Way (Astro Font API):**
```typescript
import { font } from 'astro:font';

const inter = font({
  family: 'Inter',
  source: 'google',
});
```

**Benefits to highlight:**
- ✅ **Automatic optimization** - only loads characters you use
- ✅ **Type safety** - autocomplete and error checking
- ✅ **Smart defaults** - preconnect, display:swap, etc.
- ✅ **Zero configuration** - just import and use
- ✅ **Smaller downloads** - automatic subsetting

### Demo: Basic Google Font (Inter)
**Key points:**
- Show the code
- Show the rendered result
- Compare network tab: optimized vs manual
- Point out automatic subsetting

### Demo: Specific Weights (Playfair Display)
**Key points:**
- Only load weights you need
- Reduces file size dramatically
- Compare: loading all weights vs specific ones

**Performance benefit:**
"Loading all 9 weights of Playfair? That's ~600KB. Loading just 3? ~200KB. The Font API makes this trivial."

### Demo: Multiple Styles (Roboto Mono)
**Key points:**
- Combining weights and styles (regular, italic, bold, bold-italic)
- Still optimized automatically
- Type-safe weight/style selection

**Accessibility benefit:**
"Fonts load faster = content appears faster = better experience for users on slow connections or assistive technologies."

---

## 2. Local Fonts (4-5 min)

### Why Self-Host?

**The Privacy Story:**
"Every time someone loads a Google Font from Google's CDN, Google gets a ping. They know someone visited your site. For some projects - healthcare, finance, education - that's a privacy concern."

**Reasons to self-host:**
- 🔒 **Privacy** - no third-party requests, no tracking
- 🌐 **Offline support** - fonts work even without internet
- ⚡ **Speed** - no DNS lookup, no extra connection
- 🎯 **Control** - you own the files, you control caching
- 📦 **GDPR compliance** - no data shared with Google

### The Old Way (pain):
```css
@font-face {
  font-family: 'Ubuntu';
  src: url('/fonts/Ubuntu-Regular.ttf') format('truetype');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
/* Repeat for every weight/style... */
```

**Problems:**
- Manual @font-face declarations (tedious, error-prone)
- Have to manage font-display yourself
- No automatic preload
- Easy to forget font formats
- Hard to maintain multiple weights

### The New Way:
```typescript
const ubuntu = font({
  family: 'Ubuntu Local',
  src: '/fonts/Ubuntu-Regular.ttf',
  weight: 400,
});
```

**Benefits to highlight:**
- ✅ Automatic @font-face generation
- ✅ Smart preloading
- ✅ Format detection
- ✅ Type-safe configuration

### Demo: Ubuntu Font
**Key points:**
- Show the font files in /public/fonts
- Show the simple API call
- Show rendered result
- Mention privacy benefits

**Accessibility note:**
"Self-hosted fonts also work offline - great for progressive web apps and users with intermittent connectivity."

---

## 3. Custom Configuration (7-10 min)

### The Performance Story
"Fonts are often the largest blocking resource on your page. Let's optimize."

### display: 'swap' - Preventing FOIT

**The problem:**
"Without font-display:swap, browsers show invisible text while fonts load. Users literally can't read your content. This is terrible for accessibility and UX."

**The solution:**
```typescript
const lora = font({
  family: 'Lora',
  source: 'google',
  display: 'swap', // Show fallback immediately, swap when loaded
});
```

**Benefits:**
- ✅ **No invisible text** - content always visible
- ✅ **Better perceived performance** - users can start reading immediately
- ✅ **Accessibility win** - screen readers can access text right away
- ✅ **Better Core Web Vitals** - improved FCP (First Contentful Paint)

### preload: true - Critical Fonts First

**When to use:**
"Preload fonts that are above the fold - headlines, navigation. Don't preload everything or you'll hurt performance."

```typescript
const poppins = font({
  family: 'Poppins',
  source: 'google',
  preload: true, // High priority loading
});
```

**Benefits:**
- ✅ Faster render for critical text
- ✅ Browser prioritizes this font
- ⚠️ Trade-off: uses bandwidth upfront

**Key point:**
"Preload = 'This is critical'. Only use it for fonts users see first."

### subset: Optimized Downloads

**The data story:**
"Noto Sans includes Latin, Cyrillic, Greek, Arabic, Hebrew, Thai... If your site is in English, why download all that?"

```typescript
const notoSans = font({
  family: 'Noto Sans',
  source: 'google',
  subset: ['latin'], // Only Latin characters
});
```

**Performance impact:**
- Full Noto Sans: ~400KB
- Latin subset only: ~50KB
- **8x smaller download!**

**Benefits:**
- ✅ Massively reduced file size
- ✅ Faster load times
- ✅ Less data usage (mobile users love you)
- ✅ Better environmental impact (smaller transfers = less energy)

### fallback: Custom Fallback Stack

**The layout shift story:**
"When a web font loads, the page reflows. Buttons move. Headlines shift. Users lose their place. Google penalizes you for layout shift."

```typescript
const merriweather = font({
  family: 'Merriweather',
  source: 'google',
  fallback: ['Georgia', 'serif'], // Similar metrics
});
```

**Benefits:**
- ✅ Minimizes Cumulative Layout Shift (CLS)
- ✅ Better Core Web Vitals
- ✅ Better UX - content doesn't jump around
- ✅ Accessibility - users with motion sensitivity benefit

**Key insight:**
"Choose fallback fonts with similar x-height and character width. Georgia is close to Merriweather, so the shift is minimal when the web font loads."

---

## 4. Variable Fonts (6-8 min)

### What Are Variable Fonts?

**The old problem:**
"Want 5 different font weights? That's 5 separate files. 5 HTTP requests. Maybe 250KB total. Slow."

**Variable fonts:**
"One file. Every weight from 100 to 900. Usually smaller than loading 3-4 static fonts."

### Benefits of Variable Fonts

**Performance:**
- ✅ Single file = one HTTP request
- ✅ Often smaller than multiple static fonts
- ✅ Faster page loads

**Design flexibility:**
- ✅ Any weight value (not just 400, 700)
- ✅ Smooth animations between weights
- ✅ Fine-tuned typography control

**Accessibility:**
- ✅ Responsive typography - adjust weight based on screen size
- ✅ Better readability control for users who need it

### Demo: Inter Variable

**Key points:**
- Show all standard weights (100-900)
- Show custom weights (450, 650)
- Emphasize single file download

```typescript
const inter = font({
  family: 'Inter Variable',
  source: 'google',
  weight: '100 900', // Range, not array
  variable: true,
});
```

### Interactive Demo: Weight Slider

**Key moment:**
"Watch this - I can adjust the font weight to ANY value. 427? Sure. 683? No problem. Try doing that with static fonts."

**Benefits to highlight:**
- ✅ Infinite weight control
- ✅ Smooth transitions
- ✅ Design experimentation
- ✅ Responsive typography possibilities

### Demo: Recursive (Multi-axis)

**Advanced concept:**
"Some variable fonts have multiple axes - not just weight, but width, slant, even style (monospace to sans-serif). Recursive is a great example."

**Key point:**
"Variable fonts are the future. Browser support is excellent. File sizes are competitive. If you're starting a new project, use them."

---

## 5. Font Fallbacks (5-7 min)

### Why Fallbacks Matter

**The story:**
"Fonts fail to load sometimes. Slow connections. Ad blockers. CDN outages. Your fallback stack is your safety net."

**More important:**
"Even when fonts DO load, there's a delay. Fallbacks are what users see FIRST. Choose them carefully."

### System Font Fallbacks

**Example: Montserrat with system-ui**

```typescript
const montserrat = font({
  family: 'Montserrat',
  source: 'google',
  fallback: ['system-ui', '-apple-system', 'BlinkMacSystemFont', 'Segoe UI', 'sans-serif'],
});
```

**Benefits:**
- ✅ Zero download - already on device
- ✅ Instant render
- ✅ OS-native look (San Francisco on Mac, Segoe on Windows)
- ✅ Accessibility - system fonts respect user preferences

### Metrics-Matched Fallbacks

**The layout shift solution:**
"Georgia has similar proportions to Source Serif. When Source Serif loads, the layout barely shifts."

```typescript
const sourceSerif = font({
  family: 'Source Serif 4',
  source: 'google',
  fallback: ['Georgia', 'Cambria', 'Times New Roman', 'serif'],
});
```

**Accessibility impact:**
- ✅ Less motion = better for users with vestibular disorders
- ✅ Better reading experience
- ✅ Users maintain their place in text

### Best Practices for Fallback Stacks

**Rules to follow:**
1. **Match categories** - sans for sans, serif for serif
2. **Match metrics** - x-height, character width
3. **System fonts first** - they're already there
4. **End with generic** - always have 'sans-serif' or 'serif' as last fallback
5. **Test it** - disable web fonts and see what users see

**Common stacks to recommend:**
- Modern sans: `system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- Traditional serif: `Georgia, Cambria, 'Times New Roman', serif`
- Monospace: `'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, monospace`

---

## Conclusion & Best Practices (2-3 min)

### Quick Wins

**For every project:**
1. ✅ Always use `display: 'swap'`
2. ✅ Only preload critical fonts
3. ✅ Subset when you can
4. ✅ Choose good fallbacks
5. ✅ Test on slow connections

### Performance Checklist

- [ ] Load only the weights you need
- [ ] Use subsets for languages you support
- [ ] Preload only above-the-fold fonts
- [ ] Use display: swap
- [ ] Choose metrics-matched fallbacks
- [ ] Test with throttled network

### Accessibility Checklist

- [ ] Content is readable while fonts load (display: swap)
- [ ] Fallback fonts are legible
- [ ] Layout shift is minimal
- [ ] Fonts work offline (if PWA)
- [ ] Text remains selectable during load

### Privacy Considerations

**When to self-host:**
- Healthcare/medical sites
- Financial services
- Educational institutions
- GDPR-sensitive applications
- Any site where user privacy is critical

### The Big Picture

**Recap the pain → solution arc:**
"We started with the pain of manual font loading - FOIT, FOUT, layout shift, privacy concerns, huge downloads. Astro's Font API solves all of this with a simple, type-safe API."

**Key benefits:**
- 🚀 **Better performance** - automatic optimization, subsetting, preloading
- ♿ **Better accessibility** - no invisible text, minimal layout shift
- 🔒 **Better privacy** - easy to self-host
- 💪 **Better DX** - type-safe, zero config, automatic best practices
- 🌍 **Better for users** - faster loads, less data usage, more accessible

### Call to Action

"The demo repo is linked below. Clone it, play with it, use it in your next project. Font loading doesn't have to be painful anymore."

**Resources:**
- 🔗 Demo repository
- 📚 Astro Font API docs
- 🎯 Google Fonts
- 📖 Variable Fonts guide

---

## Key Messages Throughout

### Performance Theme
- Fonts are often the biggest blocking resource
- Every optimization compounds
- Faster fonts = happier users

### Accessibility Theme
- Fonts aren't just about aesthetics
- Invisible text excludes users
- Layout shift disrupts reading
- Good font loading is inclusive design

### Privacy Theme
- Every Google Font request = tracking
- Self-hosting respects user privacy
- Compliance matters (GDPR, HIPAA, etc.)

### Developer Experience Theme
- Type safety prevents bugs
- Automatic optimization prevents mistakes
- Good defaults save time
- Less code = less maintenance

---

## Tone & Style Notes

- **Start with empathy** - acknowledge the frustrations
- **Tell stories** - layout shift, FOIT, privacy concerns
- **Show, don't tell** - demo the actual problems and solutions
- **Celebrate wins** - emphasize the benefits throughout
- **Be practical** - give specific recommendations
- **End optimistic** - web fonts CAN be done well now

## Timing Recommendations

- **Don't rush the pain points** - make them feel it
- **Linger on performance wins** - show network tabs, file sizes
- **Emphasize accessibility** - it's often overlooked
- **Keep code examples brief** - focus on concepts
- **Use the interactive demos** - especially the variable font slider

## Things to Avoid

- ❌ Don't bash Google Fonts (it's a great service)
- ❌ Don't overcomplicate (keep it practical)
- ❌ Don't skip accessibility (it's crucial)
- ❌ Don't forget to mention browser support (it's excellent)
- ❌ Don't make it just about Astro (the lessons apply everywhere)
