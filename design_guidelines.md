# Design Guidelines: Discourse Q&A Assistant with RAG

## Design Approach

**Selected Approach:** Design System (Linear + Notion Hybrid)

**Justification:** This is a utility-focused productivity tool requiring clarity, efficiency, and information density. Drawing from Linear's precision and Notion's approachable data presentation creates the optimal balance for a technical Q&A interface.

**Key Principles:**
- Clarity over decoration: Every element serves retrieval and comprehension
- Information hierarchy: Distinguish questions, answers, and citations clearly
- Spatial efficiency: Maximize content density without clutter
- Trust through transparency: Citations and sources prominently displayed

---

## Core Design Elements

### A. Typography

**Font Families:**
- Primary: Inter (system-ui fallback) - Interface text, questions, metadata
- Content: System font stack - Answer content, citations
- Mono: JetBrains Mono - Technical data, scores, timestamps

**Hierarchy:**
- H1 (App Title): 24px, weight 600
- H2 (Section Headers): 18px, weight 600
- Body (Answers): 15px, weight 400, line-height 1.6
- Small (Metadata): 13px, weight 500
- Tiny (Timestamps): 12px, weight 400

---

### B. Layout System

**Spacing Primitives:** Tailwind units of 2, 4, 6, and 8
- Micro spacing: p-2, gap-2 (8px)
- Component spacing: p-4, gap-4 (16px)
- Section spacing: p-6, gap-6 (24px)
- Major spacing: p-8, gap-8 (32px)

**Layout Structure:**
```
┌─────────────────────────────────────────┐
│  Header (h-16, border-b)                │
├─────────────────────────────────────────┤
│                                         │
│  Main Content Area                      │
│  (flex-1, overflow-auto)                │
│                                         │
│  ┌───────────────────────────────────┐  │
│  │ Chat Messages Container           │  │
│  │ (max-w-4xl, mx-auto, p-6)        │  │
│  └───────────────────────────────────┘  │
│                                         │
├─────────────────────────────────────────┤
│  Input Area (sticky bottom, p-4)       │
│  (max-w-4xl, mx-auto)                  │
└─────────────────────────────────────────┘
```

---

### C. Component Library

#### 1. Header Component
- Full-width container with subtle border-b
- Left: Logo/Title with icon
- Right: Filter toggle button + Settings icon
- Height: h-16
- Padding: px-6

#### 2. Chat Message Components

**User Question Bubble:**
- Right-aligned, max-w-2xl
- Rounded-lg corners (rounded-2xl)
- Padding: p-4
- Timestamp below, text-sm
- Subtle elevation (shadow-sm)

**Assistant Answer Card:**
- Left-aligned, max-w-3xl
- Two-section card with border
- Top section: Answer text with generous line-height (1.6)
- Bottom section: Citations panel
- Padding: p-6
- Rounded corners: rounded-xl

#### 3. Citations Panel
**Layout:** Grid of citation cards
- Grid: grid-cols-1 md:grid-cols-2 gap-4
- Each citation card contains:
  - Topic title (font-medium, text-sm)
  - Excerpt (2-line clamp, text-sm)
  - Footer row: Author • Date • Relevance score badge
  - Hover state: Subtle elevation increase
- Padding: p-4 per card
- Border: border rounded-lg
- Clickable entire card → opens Discourse URL

**Relevance Score Badge:**
- Inline badge: rounded-full px-2 py-1
- Text: text-xs font-mono
- Shows score (0.87)

#### 4. Input Area
**Structure:**
- Sticky bottom container: sticky bottom-0
- Backdrop blur effect
- Inner container: max-w-3xl mx-auto
- Flex row: gap-2

**Components:**
- Text input: flex-1, rounded-lg, p-4, border
- Send button: Icon button, rounded-lg, p-4
- Toggle button group (Short/Detailed): Segmented control to the left of input
- Filter button: Ghost button with icon

#### 5. Loading States
- Skeleton loaders for answer cards
- Pulsing animation: animate-pulse
- Three skeleton citation cards
- Typing indicator: Three animated dots in assistant bubble

#### 6. Empty State (Initial)
- Centered content: max-w-xl mx-auto text-center
- Large icon (w-16 h-16)
- Heading: "Ask about the Reading Club"
- Subtext: "Search through 500+ discussions and posts"
- Example questions as clickable pills (flex flex-wrap gap-2)

#### 7. Filter Drawer (Optional Toggle)
- Slide-in from right
- Fixed overlay: fixed inset-y-0 right-0 w-80
- Filter options:
  - Date range slider
  - Author multi-select
  - Topic tags
- Apply/Clear buttons at bottom

#### 8. Error State
- Alert banner within message flow
- Icon + Error message
- Retry button
- Subtle red accent border

---

### D. Responsive Behavior

**Desktop (lg:):**
- Max-width containers: max-w-4xl
- Two-column citation grid
- Horizontal filter panel

**Tablet (md:):**
- Max-width: max-w-2xl
- Two-column citations
- Drawer for filters

**Mobile (base):**
- Full-width with px-4 padding
- Single-column citations
- Bottom sheet for filters

---

## Images

**Hero Section:** None - this is a chat application that should show chat interface immediately

**Icons:**
- Use Heroicons via CDN
- Search icon (magnifying glass) for input
- Sparkles icon for AI indicator
- External link icon for citations
- Filter icon for filter toggle
- Settings/cog for preferences

**Avatar Placeholders:**
- User avatar: Circle with initials or user icon
- Assistant avatar: AI/bot icon in circle

---

## Interaction Patterns

**Message Submission:**
- Enter key sends message
- Shift+Enter for new line
- Clear input after send
- Auto-scroll to latest message

**Citation Click:**
- Opens Discourse URL in new tab
- Entire card clickable
- External link icon indicates behavior

**Toggle Behavior:**
- Short/Detailed toggle: Segmented control
- Active state clearly indicated
- Updates apply to new queries only

**Scrolling:**
- Main chat area: overflow-y-auto
- Smooth scroll to new messages
- Scroll shadows at top/bottom when scrollable

---

## Accessibility Notes
- All interactive elements have focus states with ring-2 ring-offset-2
- Semantic HTML: main, header, article for messages
- ARIA labels on icon buttons
- Keyboard navigation for all features
- Sufficient contrast for text readability