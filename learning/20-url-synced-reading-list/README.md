# 20 — URL-synced Reading List

**Goal:** Add a shareable “Read later” experience to the **existing** Astro resource library. No new app, React dependency, or server required.

**Study order:** [task.md](task.md) → [explanation.md](explanation.md) → [solution.md](solution.md).

**Main skills:** Astro component props and bundled client scripts; browser History API; localStorage; event delegation; Astro ClientRouter lifecycle; container queries; accessible status updates.

**Estimated time:** 90–150 minutes. **Difficulty:** intermediate.

**Previous work:** Reuse the resource collection/cards from Tasks 01–03 and the existing ClientRouter from Task 07. Do not replace their routing or filtering.

**Deliverable:** A resource card “Save” control, a reading-list tray, and a Copy share link button. Use a `?saved=` URL parameter to share selected resource IDs; persisted personal selections remain local.
