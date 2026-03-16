### Enginerring Challenges 

During development of webx, several technical challenges emerged as the project evolved. These challenges primarily involved balancing visual interactivity with performance and maintainability.

### 1. Responsive Scroll Animations
#### Problem

The site includes **scroll-driven animations** where UI elements move and transition based on scroll progress.

During development, I discovered that animations tuned for desktop layouts behaved poorly on smaller screens. Elements would either animate too quickly, overlap incorrectly, or move outside the viewport.

This happened because scroll animation values such as:

- element positions
- animation phase timing
- exit offsets

were originally designed for a **single viewport size**.

#### Challenge

The challenge was to create a scroll animation system that remained **smooth and visually consistent across multiple screen sizes**, while avoiding excessive layout calculations or complex conditional logic inside components.

#### Solution

To solve this, I introduced custom responsive hooks that dynamically adjust animation parameters based on the current viewport size.

Two hooks were created:

- ```useIsMobile()``` — determines whether the viewport is below a defined breakpoint.
- ```useBreakPoint()``` — returns animation configuration values that adapt to the screen width.

Example:

```
export function useIsMobile(breakpoint = 639) {
  const [isMobile, setIsMobile] = React.useState(false);

  React.useEffect(() => {
    const update = () => setIsMobile(window.innerWidth < breakpoint);
    update();
    window.addEventListener("resize", update);
    return () => window.removeEventListener("resize", update);
  }, [breakpoint]);

  return isMobile;
}
```

The ```useBreakPoint()``` hook provides responsive values used by the animation system, such as:

- scroll phase timing
- component expansion heights
- animation exit positions

This allowed scroll animations to adapt dynamically to the viewport without requiring each component to manage its own responsive logic.

#### Outcome

By centralizing responsive animation parameters into reusable hooks, the scroll animation system became:

- more **predictable**
- easier to **maintain**
- consistent across **mobile, tablet, and desktop devices**

This approach also simplified component logic by separating **animation configuration from rendering logic**.

---

### 2. Refactoring Scroll Logic for Maintainability
#### Problem

The scroll animation system used in the services section became increasingly complex as more animation states were introduced. Each card in the stack required calculations for:

- scroll progress
- stacking phases
- exit positions
- responsive layout changes
- content opacity and scaling

Initially, much of this logic lived directly inside the component. As the animation grew more sophisticated, the component became difficult to read and maintain, at its peak the file had over 500 lines code. 

#### Challenge

The challenge was to **reduce complexity within the component while preserving the flexibility of the animation system**. Without refactoring, the component would become tightly coupled to the animation calculations, making future changes difficult.

#### Solution

To improve maintainability, the scroll animation logic was broken into smaller, reusable utility functions and hooks located in a dedicated utility module.

Instead of handling all calculations inside the component, responsibilities were separated into focused helpers such as:

- ```useDesktopScroll()``` – calculates scroll progress for desktop layouts
- ```useMobileScroll()``` – handles scroll progress logic for mobile layouts
- ```useDesktopY() / useMobileY()``` – compute card positioning during animations
- ```useDesktopHeight() / useMobileHeight()``` – manage expansion and collapse behavior

This approach allowed the main component to focus only on rendering and composition, while animation logic was handled by specialized utilities.

Example:

```
const desktopScroll = useDesktopScroll({
  progress,
  start: layout.start,
  end: layout.end,
  globalEnd: breakpoints.globalEnd,
});

const desktopY = useDesktopY({
  index,
  progress,
  cardProgress: desktopScroll.cardProgress,
});
```
#### Outcome

By separating animation calculations into dedicated utilities:

- the main component became **significantly easier to read**
- scroll logic became **reusable across components**
- animation behavior became **easier to debug and extend**
- reduced the file size by half

This refactor helped transform a complex animation implementation into a **modular and maintainable system**.

---

### 3. Synchronizing Multi-Phase Scroll Animations
#### Problem

The services section uses a stacked card animation where multiple cards animate in sequence as the user scrolls. Each card must transition through several animation phases, including:

- entering the stack
- expanding to reveal content
- collapsing and restacking
- responsive layout changes
- exiting the viewport

Because multiple cards are visible at once, their animations must remain **synchronized with scroll progress while still behaving independently**.

Without careful coordination, issues can occur such as:
- cards overlapping incorrectly
- animations triggering too early or too late
- inconsistent spacing between stacked elements

#### Challenge

To solve this, the scroll animation system was designed around **scroll progress mapping**. The global scroll progress value was divided into smaller ranges representing different animation phases

Each card calculates its own animation state based on:
- its position in the stack
- the global scroll progress
- predefined phase boundaries (start, expand, restack, exit)

These calculations are encapsulated in utility functions such as:

```
  const layout = getCardLayout(index, total);
```
This helper determines the scroll boundaries for each card, allowing animations to remain synchronized while still behaving independently.

Scroll progress is then used to drive motion values that control properties such as:
- ```translateY```
- ```scale```
- ```opacity```
- ```height```

#### Outcome

This approach allowed the stacked cards to animate smoothly as the user scrolls while maintaining consistent spacing and timing across all cards.

By mapping animation phases to scroll progress ranges, the animation system remains:
- predictable
- scalable to additional cards
- easier to reason about during future changes
