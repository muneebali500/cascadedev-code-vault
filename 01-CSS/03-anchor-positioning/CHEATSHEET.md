# Modern CSS Cheat Sheet: Anchor Positioning (`anchor()`, `position-anchor`, `position-try-fallbacks`, `@position-try`)
> **Channel**: CascadeDev (`@cascadedev` • `https://www.youtube.com/@cascadedev`)  
> **GitHub Code Vault**: [github.com/muneebali500/cascadedev-code-vault](https://github.com/muneebali500/cascadedev-code-vault)  
> **Browser Compatibility**: Chrome & Edge 125+ ✅ | Safari 26+ ⚡ | Firefox In Active Development 🔧 (+ `@oddbird/css-anchor-positioning` Polyfill)

---

## 💡 The Mental Model in 30 Seconds

### 🌍 Beat 1: Real-World Analogy (Boat & Harbor Dock Tether)
Imagine a **floating boat (`.tooltip`)** tied to a **harbor dock post (`.btn`)** with a smart mooring rope:
1. You give the dock post a unique ID badge (`anchor-name: --dock`).
2. You tie the boat's rope to that dock (`position-anchor: --dock`) and park `10px` north of the post (`bottom: calc(anchor(top) + 10px)`).
3. If the tide pushes the boat against the north harbor wall, the boat automatically swings around to the south side of the post (`position-try-fallbacks: flip-block`) without snapping the rope.

### ⚙️ Beat 2: Under the Hood (Browser DOM Layout Engine)
```
DOM Tree (Can live in completely separate parent containers!)
┌──────────────────────────────────────────────────────────────────┐
│  <button class="btn">          anchor-name: --cta;               │
│     (Registered in Browser CSS Anchor Coordinate Registry)       │
└───────────────────────┬──────────────────────────────────────────┘
                        │  Live 60fps Compositor Tether (0 KB JS)
                        ▼
┌──────────────────────────────────────────────────────────────────┐
│  <div class="tooltip" popover> position-anchor: --cta;           │
│     bottom: calc(anchor(top) + 10px);                            │
│     justify-self: anchor-center;                                 │
│     position-try-fallbacks: flip-block, flip-inline;             │
└──────────────────────────────────────────────────────────────────┘
```

- **`anchor-name`**: Registers an element in the browser's CSS Anchor Registry using a dashed ident (`--my-anchor`).
- **`position-anchor`**: Tethers a positioned element (`position: fixed` or `position: absolute`) to a named anchor anywhere in the DOM.
- **`anchor()`**: Resolves to the physical/logical coordinate of the anchor's edge (`top`, `bottom`, `left`, `right`, `center`, `inside`, `outside`).
- **`anchor-size()`**: Reads the anchor's live dimensions (`width`, `height`, `inline`, `block`) for responsive sizing.
- **`position-try-fallbacks`**: Instructs the browser layout engine to test fallback positions (`flip-block`, `flip-inline`, or custom `@position-try` blocks) whenever the element would overflow the viewport.

---

## 🚀 4 Essential Production Patterns

### 1. Basic Centered Tooltip Tether (`anchor-name` + `anchor()`)
Tethers a fixed tooltip `10px` above a trigger button and centers it horizontally:
```css
.trigger-btn {
  anchor-name: --cta;
}

.tooltip {
  position: fixed;
  position-anchor: --cta;
  bottom: calc(anchor(top) + 10px);
  justify-self: anchor-center;
}
```

### 2. Automatic Viewport Collision Flipping (`position-try-fallbacks`)
Flips the tooltip from top to bottom (`flip-block`) or left to right (`flip-inline`) automatically when scrolling near viewport edges:
```css
.tooltip {
  position: fixed;
  position-anchor: --cta;
  bottom: calc(anchor(top) + 10px);
  justify-self: anchor-center;
  position-try-fallbacks: flip-block, flip-inline;
}
```

### 3. Dynamic Width Matching & Custom `@position-try` Layouts
Matches the trigger's exact width with `anchor-size(width)`, and switches to a right-side dock when vertical space runs out:
```css
.dropdown-menu {
  position: fixed;
  position-anchor: --search-box;
  top: calc(anchor(bottom) + 8px);
  left: anchor(left);
  width: anchor-size(width);
  position-try-fallbacks: flip-block, --side-dock;
}

@position-try --side-dock {
  top: anchor(top);
  left: calc(anchor(right) + 12px);
  width: 240px;
}
```

### 4. Zero-JS Native HTML `[popover]` + Anchor Positioning
Combines the Top Layer, click-outside light dismiss, and `Esc` key handling of HTML `popover` with CSS Anchor Positioning:
```css
/* HTML: <button class="profile-btn" popovertarget="user-menu">Profile</button> */
/* HTML: <div id="user-menu" popover>...</div>                                  */

.profile-btn {
  anchor-name: --profile-btn;
}

#user-menu[popover] {
  inset: auto; /* Reset browser's default viewport centering */
  position: fixed;
  position-anchor: --profile-btn;
  top: calc(anchor(bottom) + 8px);
  right: anchor(right);
  position-try-fallbacks: flip-block;
}
```

---

## 🛡️ Production Safe: Progressive Enhancement (`@supports`)

Always provide a clean baseline fallback and wrap anchor positioning inside `@supports (anchor-name: --a)`:

```css
/* Baseline fallback for legacy browsers */
.tooltip {
  position: absolute;
  bottom: calc(100% + 10px);
  left: 50%;
  transform: translateX(-50%);
}

/* Modern CSS Anchor Positioning enhancement */
@supports (anchor-name: --a) {
  .trigger-btn {
    anchor-name: --cta;
  }
  .tooltip {
    position: fixed;
    position-anchor: --cta;
    bottom: calc(anchor(top) + 10px);
    left: auto;
    transform: none;
    justify-self: anchor-center;
    position-try-fallbacks: flip-block, flip-inline;
  }
}
```

---

## 🎯 60-Second Viewer Coding Challenge

> **Challenge**: Tether the `.corner-badge` notification pill so it overlaps the **top-right corner** of `.inbox-btn` (`8px` overlap vertically, `12px` overlap horizontally), and automatically flips both vertically and horizontally when the button is dragged into any viewport corner!
>
> **How to submit**:
> 1. Open `starter-challenge.html` in your browser or CodePen.
> 2. Write your CSS rules inside the `TODO` block of `.inbox-btn` and `.corner-badge`.
> 3. Drag the button into all 4 corners of the preview box to verify it flips cleanly!
> 4. Drop your CSS snippet in the YouTube comments!
> 5. 🏆 **Community Hall of Fame**: The top 3 cleanest solutions will be pinned below the video and featured in our Community Hall of Fame!
