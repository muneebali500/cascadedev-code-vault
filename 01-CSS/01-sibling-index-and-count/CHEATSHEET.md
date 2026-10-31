# Modern CSS Cheat Sheet: `sibling-index()` & `sibling-count()`
> **Channel**: CascadeDev (`@cascadedev` • `https://www.youtube.com/@cascadedev`) • **Episode**: 01 (Modern CSS Series)  
> **GitHub Code Vault**: `https://github.com/muneebali500`  
> **Browser Compatibility**: Chrome & Edge 138+ ✅ | Safari 18.4+ ⚡ | Firefox In Active Development 🔧

---

## 💡 The Mental Model in 30 Seconds

### 🌍 Beat 1: Real-World Analogy (Stadium Sprint Track)
Imagine **5 sprint runners in lanes 1–5**. Instead of a coach taping a separate delay note to every single runner (`:nth-child(1)`, `:nth-child(2)`...), the stadium loudspeaker announces **one dynamic rule**: *"Wait = Your Lane Number × 100ms"*.

### ⚙️ Beat 2: Under the Hood (DOM Engine)
```
Parent Container (<ul class="nav-list">)
┌────────────────────────────────────────────────────────┐
│  [ Item 1 ]  sibling-index() = 1   sibling-count() = 5 │
│  [ Item 2 ]  sibling-index() = 2   sibling-count() = 5 │
│  [ Item 3 ]  sibling-index() = 3   sibling-count() = 5 │
│  [ Item 4 ]  sibling-index() = 4   sibling-count() = 5 │
│  [ Item 5 ]  sibling-index() = 5   sibling-count() = 5 │
└────────────────────────────────────────────────────────┘
```

- **`sibling-index()`**: Returns the 1-based integer index of the element among its siblings (1, 2, 3... N).
- **`sibling-count()`**: Returns the total number of sibling elements inside the shared parent container.
- **Zero JavaScript**: Evaluated dynamically by the browser's styling engine. If new DOM nodes are appended, styles recalculate instantly.

---

## 🚀 4 Essential Production Patterns

### 1. Automatic Staggered Entrance Animations
Replaces endless `:nth-child(1) { animation-delay: 100ms; }` boilerplate:
```css
.card {
  animation: slideFadeIn 0.5s cubic-bezier(0.16, 1, 0.3, 1) both;
  animation-delay: calc(sibling-index() * 120ms);
}
```

### 2. Equal Grid Columns & Progressive Opacity
Creates equal-width blocks that adapt when elements are added or removed, plus a progressive visual ramp:
```css
.step-block {
  width: calc(100% / sibling-count());
  opacity: calc(sibling-index() / sibling-count());
}
```

### 3. Dynamic OKLCH Color Wheel (360° Palette)
Evenly distributes sibling hues across the perceptual 360° OKLCH color space:
```css
.category-chip {
  --hue: calc((sibling-index() / sibling-count()) * 360deg);
  background: oklch(0.72 0.18 var(--hue));
  color: oklch(0.15 0.05 var(--hue));
}
```

### 4. Reverse Z-Index Overlapping Stack
Reverses default DOM stacking order so the first avatar or card sits on top without hardcoding z-indices:
```css
.avatar {
  margin-left: -18px;
  position: relative;
  z-index: calc(sibling-count() - sibling-index());
}
```

---

## 🛡️ Production Safe: Progressive Enhancement Fallback

Always wrap modern CSS math functions inside `@supports` for backward compatibility:

```css
/* Baseline fallback for older browsers */
.card {
  animation: slideFadeIn 0.5s ease both;
  animation-delay: 0ms;
}

/* Modern progressive enhancement */
@supports (animation-delay: calc(sibling-index() * 1ms)) {
  .card {
    animation-delay: calc(sibling-index() * 120ms);
  }
}
```

---

## 🎯 60-Second Viewer Coding Challenge

> **Challenge**: In a list of sibling bars, make the **middle item tallest**, with height tapering symmetrically towards both edges.
> 
> **How to submit**:
> 1. Open `starter-challenge.html` in your browser or paste into CodePen.
> 2. Write your 1-line `calc()` formula for `scale` or `height`.
> 3. Drop your solution in the YouTube comments under the video!
> 4. 🏆 **Community Hall of Fame**: The top 3 cleanest solutions will be pinned below the video and featured in our Community Hall of Fame!
