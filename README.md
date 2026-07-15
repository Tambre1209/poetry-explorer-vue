# Poetry Explorer
### Developer: Joshua Armendariz

A responsive, single page reading interface focused entirely on the typographic layout and structural preservation of classic literature. The application interacts with the public PoetryDB API to provide a clean, enjoyable reading experience.

---

## AI Collaboration Log

This repository was built using an iterative, AI collaborative workflow to maximize and augment the engineer's efficiency:

- **The Engineer:** Set the design goals, prototyped the UI layout in Figma, established the custom color palette, dictated the typographic guardrails, managed git staging operations, reviewed code from the AI Pair Programmer for correctness and intent, and performed manual end-to-end UX testing.
- **The AI Pair Programmer:** Assisted with rapid component scaffolding, bug fixes, feature development dictated by the Engineer, and helped manage the application state transitions.

---

## Implemented Features

### 1. In Memory Client Caching
During development it was discovered that there are frequent periods of high latency and timeout bottlenecks on the public PoetryDB endpoint. A global cache layer interceptor was built on the client side. Successful payloads are retained in memory. Cached search queries and specific poem views load under 2 milliseconds on subsequent clicks, minimizing external server reliance.

### 2. Typographic Scaling
Poems wrapping text to the following line breaks the structural intent of the author. The rendering engine measures both total line count and the exact character length of the longest verse line at runtime. The layout scales across four discrete inline font-size tiers to ensure wide lines never wrap awkwardly onto new rows on desktop viewports.

### 3. State-Aware Search Pagination
When viewing a poem from a search list, the application calculates index boundaries. It automatically suppresses the previous control on the first entry, suppresses the next control on the final entry, and includes a button that recalls the exact search parameter state directly from cache.

### 4. Random Discovery View
A sidebar link that targets and pulls individual arbitrary works from the archive database. This view completely bypasses standard search index logic and safely hides pagination headers.

### 5. Canvas Design Details
- **Top Panel Progress Indicator:** A minimal loading bar at the top of the canvas that flashes dynamically on asynchronous network calls.
- **Negative-Margin Hover Tracking:** Row highlights expand seamlessly via layout offsets, indicating selection without shifting text alignments.
- **Layered Dimension:** The reading canvas features a left-hand 24px vertical curve combined with a deep drop shadow to lift the text surface above the controller panel.

---

## A Note on State Management
State is managed natively through a centralized object in App.vue and passed down via prop drilling. Because the applciation component tree maxes out at three levels deep, this avoids needing a separate dedicated state library like Pinia. This can be refactored into a composable later if desired.

---

## Installation & Local Execution

1. Ensure Node.js is configured locally.
2. Clone the repository and navigate to the project directory.
3. Install the project dependencies:
   npm install
4. Start the local Vite development server:
   npm run dev
5. Open your browser to the local network port indicated in your terminal window.

