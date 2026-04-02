---
title: Index Page Component Behavior
draft: false
tags: 
aliases:
---
> Series of How to build my [[Quartz-Blog]]

`Path: quartz/components/`

## Overview
Two components (Explorer.tsx and RecentNotes.tsx) use conditional rendering based on the current page type to control what appears on the index (home) page.

## Components Behavior

### Explorer.tsx
```typescript
if (fileData.slug == "index") {
  return <></>
}
```
- Hides the file explorer on the index page
- Shows the explorer on all other pages
- Purpose: Keeps the home page clean without file navigation

### RecentNotes.tsx
```typescript
if (fileData.slug !== "index") {
  return <></>
}
```
- Shows recent notes only on the index page
- Hides the component on all other pages
- Purpose: Displays recent updates exclusively on the home page

## Implementation Pattern
Both components use the same pattern but with opposite conditions:
- Check `fileData.slug` to determine the current page
- Return empty fragment `<></>` to hide component
- Render full component when condition is not met

## Key Differences

| Component | Index Page | Other Pages |
|-----------|------------|-------------|
| Explorer  | Hidden     | Visible     |
| RecentNotes| Visible   | Hidden      |

## Benefits
1. **Clean Home Page**
   - No file explorer clutter
   - Focus on recent content

2. **Intuitive Navigation**
   - File explorer available when needed
   - Recent notes highlight on home page

3. **Simple Implementation**
   - Uses basic conditional rendering
   - Easy to modify or extend

## Technical Notes
- Uses TypeScript for type safety
- Relies on `fileData.slug` for page identification
- Consistent pattern across components