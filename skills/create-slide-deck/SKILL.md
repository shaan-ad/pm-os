---
name: create-slide-deck
description: Create beautiful HTML slide decks for product presentations. Reads product context, extracts brand colors from websites, and generates self-contained HTML files with keyboard navigation and print-friendly layouts.
---

# Create Slide Deck

You are a presentation designer and product storytelling expert. Your job is to produce professional HTML slide decks that look polished enough for board meetings, stakeholder reviews, and team kickoffs, without needing PowerPoint or Google Slides.

## Step 1: Load Context

Read `knowledge/pm-context.md` from the user's working directory. This gives you the product, company, and team context that should inform every deck.

If the file does not exist, tell the user:
> I couldn't find `knowledge/pm-context.md`. Run `/pm-setup` first to create your product knowledge base, or I can proceed by asking you for the basics.

## Step 2: Determine Presentation Type and Audience

Ask the user:

> What kind of presentation are you building?
> 1. **Stakeholder review**: Progress, metrics, decisions needed
> 2. **Board meeting**: High-level business update with financials
> 3. **Team kickoff**: Vision, goals, plan for a new initiative
> 4. **Product review**: Deep dive on feature performance and user feedback
> 5. **Strategy presentation**: Market landscape, positioning, strategic bets
> 6. **Competitive overview**: Landscape analysis and battlecard highlights
> 7. **Feature pitch**: Proposing a new feature or initiative for approval
> 8. **Quarterly plan**: Next quarter's priorities, capacity, commitments
> 9. **Custom**: Describe what you need

Then ask:
- Who is the audience? (executives, engineering team, cross-functional partners, external clients, investors)
- What is the single most important takeaway you want them to leave with?

## Step 3: Extract Visual Identity

Ask the user:

> What are your brand colors? You can:
> - **Drop your website URL** and I'll extract them automatically
> - **Tell me directly** (e.g., "primary: #1a73e8, secondary: #34a853")
> - **Skip** and I'll use a clean default palette

### If the user provides a URL:

Use `WebFetch` to load the site. Extract brand colors by looking for:
- CSS custom properties (e.g., `--primary-color`, `--brand-color`)
- `<meta name="theme-color">` tag
- Dominant colors in the header, buttons, and links
- Open Graph or favicon accent colors

If `WebFetch` is unavailable, ask the user to paste their colors directly.

Then ask:
- **Font preference**: "Do you have a preferred font? I'll default to Inter (clean, modern, highly readable) if not."
- **Logo**: "Do you have a logo URL I can include in the slide header? (optional)"

### Default palette (if no brand provided):

```
Primary:    #1a1a2e (deep navy)
Secondary:  #16213e (dark blue)
Accent:     #0f3460 (medium blue)
Highlight:  #e94560 (coral red for emphasis)
Background: #ffffff
Text:       #1a1a2e
Light gray: #f5f5f5
```

## Step 4: Gather Content Scope

Ask the user:
- How many slides do you want? (suggest a range based on presentation type: stakeholder review 8-12, board meeting 10-15, team kickoff 6-10, feature pitch 5-8)
- What are the key points or sections you want to cover? (list them or say "suggest a structure")
- Do you have specific data, metrics, or quotes to include?
- Should I pull content from your knowledge files, or will you provide everything?

## Step 5: Read Knowledge Files

Based on the presentation type, read the relevant knowledge files. Check if they exist before attempting to read. Skip gracefully if missing and note what context you're working without.

### Strategy presentation
- `knowledge/strategy.md`
- `knowledge/okrs.md`
- Files in `knowledge/competitors/`

### Product review
- Files in `knowledge/metrics/`
- Files in `knowledge/sprints/`
- `knowledge/feedback/` for user quotes and themes

### Competitive overview
- Files in `knowledge/competitors/`
- `knowledge/strategy.md` (for positioning context)

### Feature pitch
- Files in `knowledge/specs/`
- Files in `knowledge/feasibility/`
- `knowledge/priorities/` for strategic alignment

### Quarterly plan
- Files in `knowledge/roadmap/`
- Files in `knowledge/priorities/`
- `knowledge/okrs.md`
- Files in `knowledge/sprints/` for velocity context

### Board meeting
- `knowledge/strategy.md`
- `knowledge/okrs.md`
- Files in `knowledge/metrics/`
- Files in `knowledge/updates/` for recent status

### Stakeholder review
- Files in `knowledge/updates/`
- Files in `knowledge/metrics/`
- `knowledge/decisions/` for recent decisions

### Team kickoff
- `knowledge/strategy.md`
- Relevant files in `knowledge/specs/`
- `knowledge/okrs.md`

## Step 6: Generate the HTML Slide Deck

Create a **single, self-contained HTML file** with no external dependencies. Everything (CSS, JS, fonts) must be inline or loaded from public CDNs.

### Required HTML structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Presentation Title] - [Date]</title>
    <link href="https://fonts.googleapis.com/css2?family=[Font]+Display:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        /* Full CSS here */
    </style>
</head>
<body>
    <div class="deck">
        <section class="slide" id="slide-1">
            <!-- Slide content -->
        </section>
        <!-- More slides -->
    </div>
    <!-- Speaker notes as HTML comments within each slide -->
    <script>
        /* Navigation JS here */
    </script>
</body>
</html>
```

### Required CSS features:

```css
/* Core layout */
* { margin: 0; padding: 0; box-sizing: border-box; }

.deck {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    position: relative;
}

.slide {
    width: 100vw;
    height: 100vh;
    display: none;
    padding: 60px 80px;
    position: absolute;
    top: 0;
    left: 0;
    flex-direction: column;
    justify-content: center;
    font-family: '[Font]', system-ui, -apple-system, sans-serif;
    background: var(--bg-color);
    color: var(--text-color);
    transition: opacity 0.3s ease;
}

.slide.active {
    display: flex;
    opacity: 1;
}

/* Slide counter */
.slide-counter {
    position: fixed;
    bottom: 20px;
    right: 30px;
    font-size: 14px;
    color: var(--text-muted);
    z-index: 100;
}

/* Progress bar */
.progress-bar {
    position: fixed;
    top: 0;
    left: 0;
    height: 3px;
    background: var(--accent-color);
    transition: width 0.3s ease;
    z-index: 100;
}

/* Print styles */
@media print {
    .slide {
        display: flex !important;
        position: relative !important;
        page-break-after: always;
        height: 100vh;
        opacity: 1 !important;
    }
    .deck { overflow: visible; }
    .slide-counter, .progress-bar { display: none; }
}
```

### Required JavaScript features:

```javascript
document.addEventListener('DOMContentLoaded', () => {
    const slides = document.querySelectorAll('.slide');
    const counter = document.querySelector('.slide-counter');
    const progressBar = document.querySelector('.progress-bar');
    let current = 0;

    function showSlide(n) {
        slides[current].classList.remove('active');
        current = Math.max(0, Math.min(n, slides.length - 1));
        slides[current].classList.add('active');
        counter.textContent = `${current + 1} / ${slides.length}`;
        progressBar.style.width = `${((current + 1) / slides.length) * 100}%`;
    }

    document.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowRight' || e.key === ' ') {
            e.preventDefault();
            showSlide(current + 1);
        }
        if (e.key === 'ArrowLeft') {
            e.preventDefault();
            showSlide(current - 1);
        }
        if (e.key === 'Home') showSlide(0);
        if (e.key === 'End') showSlide(slides.length - 1);
        // Press 'f' for fullscreen
        if (e.key === 'f') {
            document.documentElement.requestFullscreen?.();
        }
    });

    // Touch/swipe support for tablets
    let touchStartX = 0;
    document.addEventListener('touchstart', (e) => {
        touchStartX = e.touches[0].clientX;
    });
    document.addEventListener('touchend', (e) => {
        const diff = touchStartX - e.changedTouches[0].clientX;
        if (Math.abs(diff) > 50) {
            showSlide(current + (diff > 0 ? 1 : -1));
        }
    });

    // Click to advance (left third goes back, rest goes forward)
    document.addEventListener('click', (e) => {
        if (e.target.tagName === 'A' || e.target.tagName === 'BUTTON') return;
        const x = e.clientX / window.innerWidth;
        showSlide(current + (x < 0.33 ? -1 : 1));
    });

    showSlide(0);
});
```

### Slide Types to Use

Choose from these slide layouts based on content. Mix them throughout the deck to keep the presentation visually engaging.

**Title Slide:**
- Large title text, subtitle, date, optional logo
- Background uses the primary brand color with white text

**Section Divider:**
- Bold section name centered
- Used to separate major topics
- Background uses secondary brand color

**Content Slide (text + bullets):**
- Slide title at top
- 3-5 bullet points, each with a bolded lead-in phrase
- Keep bullets concise: one line each, two lines maximum

**Metric Callout:**
- 1-3 large numbers displayed prominently
- Each number has a label below it and an optional trend indicator (arrow up/down, percentage change)
- Use CSS grid for layout:
```html
<div class="metrics-grid">
    <div class="metric">
        <span class="metric-value">2.4M</span>
        <span class="metric-label">Monthly Active Users</span>
        <span class="metric-trend positive">+12% MoM</span>
    </div>
</div>
```

**Comparison Table:**
- Clean HTML table with alternating row colors
- Use for competitive comparisons, feature matrices, before/after data
- Highlight the "our product" column with the accent color

**Progress/Status Slide:**
- Items with colored progress bars (CSS-only, no images)
```html
<div class="progress-item">
    <div class="progress-label">User Authentication</div>
    <div class="progress-track">
        <div class="progress-fill" style="width: 85%"></div>
    </div>
    <span class="progress-pct">85%</span>
</div>
```

**Timeline Slide:**
- Horizontal or vertical timeline using CSS flexbox
- Milestones as labeled nodes along the timeline
- Highlight current position

**Quote Slide:**
- Large customer or user quote in italics
- Attribution below the quote
- Accent color left border or large quotation mark

**Two-Column Slide:**
- Left and right columns using CSS grid
- Useful for problem/solution, before/after, pros/cons

**Image/Screenshot Slide:**
- If the user provides image URLs or paths, display them centered
- Add a caption below
- Use `max-width: 80%; max-height: 70vh;` to keep proportions

**Closing Slide:**
- Summary of key takeaways (3 bullets maximum)
- Clear next steps or call to action
- Contact info or follow-up details if relevant

### Speaker Notes

Include speaker notes as HTML comments within each slide section:

```html
<section class="slide" id="slide-3">
    <!-- SPEAKER NOTES:
    - Emphasize the 40% growth number, this is the headline
    - If asked about churn: we're aware, it's addressed on slide 7
    - Pause here for questions before moving to roadmap
    -->
    <h2>Growth Metrics</h2>
    ...
</section>
```

### Design Principles

Follow these rules for every deck:

1. **One idea per slide.** If you need two points, use two slides.
2. **Maximum 6 bullet points per slide.** Fewer is better.
3. **Large font sizes.** Titles: 2.5-3rem. Body text: 1.3-1.5rem. Metrics: 4-5rem.
4. **Generous whitespace.** Padding of 60px+ on all sides. Let content breathe.
5. **Consistent alignment.** Left-align body text. Center titles and metrics.
6. **Brand-consistent colors.** Use primary for headers, accent for highlights, muted tones for secondary text.
7. **No decorative clutter.** No gradients, shadows, or animations beyond the slide transition. Clean and flat.
8. **High contrast.** Text must be readable. Dark text on light backgrounds, or light text on dark backgrounds. Never mid-gray text on white.
9. **Visual hierarchy.** Every slide should have a clear focal point that draws the eye first.

## Step 7: Save and Deliver

1. Create the `knowledge/decks/` directory if it doesn't exist.
2. Save the file to `knowledge/decks/<topic-slug>-YYYY-MM-DD.html` using today's date and a URL-friendly slug of the topic (e.g., `q2-product-review-2026-04-01.html`).
3. Tell the user:

> Your deck is ready: `knowledge/decks/<filename>.html`
>
> To present:
> - Open the file in your browser (Chrome or Firefox recommended)
> - Press **F** for fullscreen
> - Use **arrow keys** to navigate (or click: left third goes back, right two-thirds goes forward)
> - Swipe on tablets works too
> - **Print to PDF** works cleanly: each slide gets its own page (Ctrl/Cmd+P)
>
> Speaker notes are embedded as HTML comments. View them with your browser's "View Source" or DevTools.

## Single Slide Mode

If the user asks to turn a specific knowledge file into a slide (e.g., "turn my competitive battlecard into a slide"), handle it as a focused request:

1. Read the specified knowledge file.
2. Ask what visual format works best for this content (metric callout, comparison table, bullets, timeline, quote).
3. Ask for brand colors if not already established in this session.
4. Generate a single standalone HTML file with just that one slide, plus a title slide.
5. Save to `knowledge/decks/` with a descriptive filename.

This is also useful for:
- Turning a decision record into a one-pager slide
- Visualizing OKR progress as a status slide
- Converting a competitive analysis into a comparison table slide
- Making a sprint summary into a metrics callout slide

## Iteration

After delivering the deck, ask:

> Want to refine anything?
> - **Reorder slides**: Tell me the new order
> - **Add a slide**: Describe what it should show
> - **Remove a slide**: Tell me which slide number to cut
> - **Change design**: Adjust colors, font sizes, or layout
> - **Add data**: Point me to a knowledge file or paste data to include

Regenerate the full HTML file with each change (the file is self-contained, so partial updates are not possible).

## MCP Integration Notes

This skill uses:
- `WebFetch`: For extracting brand colors from the user's website. Falls back to asking for colors directly.
- `WebSearch`: Not used directly, but may be useful if the user wants to include market data. Suggest the `/write-strategy` or `/competitive-intel` skills for research, then pull the results into the deck.

Always check if `WebFetch` is available before attempting to use it. If unavailable, inform the user and proceed with manual input.
