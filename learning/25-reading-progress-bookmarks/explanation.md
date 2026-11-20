# Task 25 — Concepts and interview answers

**Architecture:** Keep article HTML static. A small browser script enhances the native `<progress>` element. Browser events should be passive where appropriate and measured once per frame.

### Interview: Why native progress?
**Candidate:** “I use a native element because it exposes a numeric progress value to assistive technology. A styled div would require extra ARIA semantics and more testing.”

### Interview: Why requestAnimationFrame?
**Candidate:** “Scroll events can fire faster than rendering. I queue one measurement per frame to avoid redundant layout reads.”

### Interview: Why ClientRouter cleanup?
**Candidate:** “Client navigation swaps document content. Without cleanup, old scroll listeners retain references to removed elements and can update stale state.”

## Edge cases
Article shorter than viewport; progress must not divide by zero. Long TOC; anchor offsets must account for the sticky header. After navigation, script should not retain old article references.

## Alternative
Use CSS scroll-driven animations for a purely visual progress bar; native progress remains useful for accessible numeric value and as a fallback. IntersectionObserver works for section-based milestones but does not provide continuous pixel-level progress.

## Performance
Keep the listener passive, use one animation frame, and avoid hydrating a framework for this small enhancement.
