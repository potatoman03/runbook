---
name: explain
description: Generate visual explanations of code, architecture, or concepts. Creates HTML presentations, ASCII diagrams, and interactive visualizations. Use for learning unfamiliar code or explaining to others.
argument-hint: [topic|file|--slides|--ascii|--interactive]
user-invocable: true
allowed-tools: Read, Glob, Grep, Write
---

# Explain: Visual Learning

Have Claude generate a visual HTML presentation explaining unfamiliar code. It makes surprisingly good slides!
Ask Claude to draw ASCII diagrams of new protocols and codebases to help you understand them.
— Claude Code team tips

## Activation

When user invokes `/explain [topic]` with optional format flags:

## Output Formats

### ASCII Diagrams (`/explain --ascii [topic]`)

Generate clear ASCII art diagrams:

**System Architecture**
```
┌─────────────────────────────────────────────────────────────┐
│                        Load Balancer                         │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│   Web Server  │    │   Web Server  │    │   Web Server  │
│    (Node.js)  │    │    (Node.js)  │    │    (Node.js)  │
└───────┬───────┘    └───────┬───────┘    └───────┬───────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
                    ┌─────────────────┐
                    │   Redis Cache   │
                    └────────┬────────┘
                              │
                    ┌────────▼────────┐
                    │   PostgreSQL    │
                    │    Database     │
                    └─────────────────┘
```

**Data Flow**
```
User Input ──► Validation ──► Transform ──► Database
                  │                            │
                  ▼                            ▼
              Error ◄─────────────────── Response
```

**State Machine**
```
            ┌──────────────────────────────────────┐
            │                                      │
            ▼                                      │
    ┌───────────────┐   authenticate   ┌──────────┴────┐
    │   Anonymous   │ ───────────────► │   Logged In   │
    └───────────────┘                  └───────────────┘
            ▲                                  │
            │           logout                 │
            └──────────────────────────────────┘
```

### HTML Slides (`/explain --slides [topic]`)

Generate a self-contained HTML presentation:

```html
<!DOCTYPE html>
<html>
<head>
  <title>[Topic] Explained</title>
  <style>
    /* Clean, readable slide styles */
    .slide {
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding: 2rem;
    }
    .slide h1 { font-size: 3rem; }
    .slide code {
      background: #f5f5f5;
      padding: 1rem;
      border-radius: 8px;
    }
    /* Navigation */
    .nav { position: fixed; bottom: 1rem; right: 1rem; }
  </style>
</head>
<body>
  <div class="slide" id="slide1">
    <h1>Understanding [Topic]</h1>
    <p>A visual guide to how [topic] works</p>
  </div>

  <div class="slide" id="slide2">
    <h2>The Core Concept</h2>
    <pre><code>// Key code example</code></pre>
    <p>Explanation of what this does...</p>
  </div>

  <!-- More slides... -->

  <script>
    // Simple keyboard navigation
    let current = 0;
    const slides = document.querySelectorAll('.slide');
    document.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowRight') current = Math.min(current + 1, slides.length - 1);
      if (e.key === 'ArrowLeft') current = Math.max(current - 1, 0);
      slides[current].scrollIntoView({ behavior: 'smooth' });
    });
  </script>
</body>
</html>
```

Save to `.claude/explanations/[topic].html` and open in browser.

### Interactive Explanation (`/explain --interactive [topic]`)

Generate HTML with interactive elements:
- Collapsible sections
- Hover tooltips on code
- Step-through animations
- Clickable diagram elements

### Default: Markdown with ASCII

If no format specified, combine markdown explanation with ASCII diagrams inline.

## What to Explain

### Code Files
```
/explain src/auth/middleware.ts
```
- Purpose and responsibility
- Input/output flow
- Key functions and their roles
- How it connects to rest of system

### Architecture
```
/explain architecture
/explain --ascii system design
```
- High-level component diagram
- Data flow between services
- Key boundaries and interfaces

### Protocols/Algorithms
```
/explain OAuth2 flow
/explain --ascii how rate limiting works
```
- Step-by-step process
- State transitions
- Edge cases and error paths

### Concepts
```
/explain dependency injection in this codebase
```
- What it is
- Why it's used here
- Examples from the actual code

## Example Session

```
User: /explain --slides src/services/payment.ts

Claude: Creating presentation for payment service...

Generated: .claude/explanations/payment-service.html

## Payment Service Explained

**Slide 1: Overview**
The payment service handles all transaction processing...

**Slide 2: Flow Diagram**
[ASCII diagram of payment flow]

**Slide 3: Key Functions**
- `processPayment()` - Main entry point
- `validateCard()` - Card validation logic
- `settleTransaction()` - Finalization

**Slide 4: Error Handling**
[Diagram of error states and recovery]

**Slide 5: Integration Points**
[How it connects to Stripe, database, notification service]

Open .claude/explanations/payment-service.html in your browser
to view the interactive presentation.
```
