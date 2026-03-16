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
