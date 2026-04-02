---
title: Quartz Update Graph View Setting
draft: false
tags: 
aliases:
---
> Series of How to build my [[Quartz-Blog]]

`Path: quartz/components/Graph.tsx`
## Modification Purpose
Modified the Graph component to display different types of graphs based on the page type:
- Show global graph by default on the index page
- Maintain local graph view on other pages
- Keep full-screen toggle functionality on all pages

## Code Changes

### Component Props
```typescript
// Before
const Graph: QuartzComponent = ({ displayClass, cfg }: QuartzComponentProps)

// After
const Graph: QuartzComponent = ({ displayClass, cfg, fileData }: QuartzComponentProps)
```

### Graph Container Logic
```typescript
// Before
<div id="graph-container" data-cfg={JSON.stringify(localGraph)}></div>

// After
const isIndexPage = fileData.slug === "index"
<div id="graph-container" data-cfg={JSON.stringify(isIndexPage ? globalGraph : localGraph)}></div>
```

## How It Works
- Uses `fileData.slug` to check if current page is index
- On index page: Displays global graph in the default container
- On other pages: Shows local graph as before
- Maintains existing button for full-screen toggle

## Key Features
- Same container size across all pages
- Consistent UI layout and positioning
- Full-screen toggle functionality preserved
- Smooth transition between graph types

## Configuration
Both graph types maintain their original settings:
```typescript
defaultOptions = {
  localGraph: {
    depth: 1,
    scale: 1.1,
    // other settings...
  },
  globalGraph: {
    depth: -1,
    scale: 0.9,
    // other settings...
  }
}
```

## Implementation Notes
- No changes to basic component structure
- Minimal code modifications required
- Preserves all existing functionality
- Easy to extend for future updates