---
name: browser-vision
version: 1.0.0
category: devops
description: Browser screenshot analysis — when to use browser_vision vs text snapshot, CAPTCHA handling, visual verification, and annotation.
tags: [browser, vision, screenshot, captcha, visual]
---

# browser_vision Tool

Best practices for taking and analyzing screenshots via the browser_vision tool.

## When to Use

- CAPTCHAs or visual verification challenges
- Complex page layouts that text snapshot misses
- Visual confirmation of UI changes
- Charts, graphs, images content extraction

## Tool Syntax

```python
browser_vision(question="What does the CAPTCHA show?", annotate=True)
```

## Workflow

### 1. Navigate to page
```python
browser_navigate(url="https://example.com/page")
```

### 2. Take annotated screenshot
```python
browser_vision(question="Describe the login form layout", annotate=True)
```

### 3. If CAPTCHA detected
```python
browser_vision(question="What characters do you see in this CAPTCHA?")
```

## Error Handling

If browser_vision fails (screenshot unavailable, page not loaded):
1. Verify the page is loaded with `browser_snapshot()` first
2. If page navigation is needed: call `browser_navigate()` with the URL
3. If screenshot still fails: fall back to `browser_snapshot()` for text content
4. For CAPTCHA: if vision fails, try describing what the text snapshot shows as a fallback

## Anti-patterns

- Using browser_vision for simple text content → use browser_snapshot instead
- Not setting annotate=True for complex layouts
