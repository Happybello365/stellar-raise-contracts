# MilestoneHighlights Component

## Overview

The `MilestoneHighlights` component displays campaign milestone achievements with visual highlights, progress indicators, and achievement badges. It provides a clear, accessible way to showcase funding progress and celebrate milestones.

## Features

- **Visual Progress Bar**: Displays current funding progress as a percentage
- **Milestone Markers**: Shows predefined milestone thresholds (25%, 50%, 75%, 100%)
- **Achievement Badges**: Indicates whether each milestone has been achieved
- **Keyboard Navigation**: Full keyboard accessibility with Enter and Space key support
- **ARIA Labels**: Comprehensive accessibility support for screen readers
- **XSS Protection**: Sanitizes all user-supplied strings

## Props

```typescript
interface MilestoneHighlightsProps {
  /** Campaign name to display */
  campaignName: string;
  
  /** Current funding progress (0-100) */
  currentProgress: number;
  
  /** Array of milestone definitions */
  milestones: Milestone[];
  
  /** Optional callback when a milestone is clicked */
  onMilestoneClick?: (milestone: Milestone) => void;
}

interface Milestone {
  /** Unique identifier */
  id: string;
  
  /** Display label for the milestone */
  label: string;
  
  /** Funding percentage threshold (0-100) */
  percentage: number;
  
  /** Whether the milestone has been achieved */
  achieved: boolean;
  
  /** Optional date when milestone was achieved */
  achievedAt?: Date;
}
```

## Usage

```tsx
import MilestoneHighlights from './milestone_highlights';

const milestones = [
  { id: 'm1', label: '25% Funded', percentage: 25, achieved: false },
  { id: 'm2', label: '50% Funded', percentage: 50, achieved: true, achievedAt: new Date() },
  { id: 'm3', label: '75% Funded', percentage: 75, achieved: false },
  { id: 'm4', label: 'Fully Funded', percentage: 100, achieved: false },
];

<MilestoneHighlights
  campaignName="My Campaign"
  currentProgress={50}
  milestones={milestones}
  onMilestoneClick={(milestone) => console.log('Clicked:', milestone)}
/>
```

## Security

### Input Sanitization

- **Campaign Name**: Sanitized to remove HTML tags and limit length to 100 characters
- **Milestone Labels**: Sanitized to prevent XSS attacks
- **Progress Values**: Clamped to [0, 100] range to prevent layout abuse

### No Dangerous APIs

- No `dangerouslySetInnerHTML` usage
- All content rendered as React text nodes
- Canvas drawing uses only hardcoded color palettes

## Accessibility

### ARIA Attributes

- Progress bar has `aria-valuenow`, `aria-valuemin`, `aria-valuemax`
- Region has `aria-label` for semantic structure
- Milestone items have `aria-pressed` to indicate state

### Keyboard Support

- Milestone items are focusable with `tabIndex={0}`
- Enter and Space keys trigger milestone click handlers
- Full keyboard navigation support

### Screen Reader Support

- Semantic HTML structure
- Descriptive ARIA labels
- Status announcements for progress changes

## Helper Functions

### `sanitizeMilestoneLabel(label: string): string`

Removes HTML tags and truncates to 100 characters.

```typescript
sanitizeMilestoneLabel("<script>alert('xss')</script>") // "scriptalertxssscript"
```

### `clampMilestoneProgress(value: number): number`

Clamps progress value to [0, 100] range.

```typescript
clampMilestoneProgress(150) // 100
clampMilestoneProgress(-10) // 0
```

### `isMilestoneAchieved(currentProgress: number, milestonePercentage: number): boolean`

Determines if a milestone is achieved based on current progress.

```typescript
isMilestoneAchieved(50, 25) // true
isMilestoneAchieved(25, 50) // false
```

### `getAchievementBadge(milestone: Milestone): string`

Generates achievement badge content.

```typescript
getAchievementBadge({ achieved: false }) // "Locked"
getAchievementBadge({ achieved: true, achievedAt: new Date() }) // "Achieved 3/29/2026"
```

## Styling

The component uses BEM naming convention for CSS classes:

- `.milestone-highlights` - Root container
- `.milestone-highlights__title` - Campaign title
- `.milestone-highlights__progress-bar` - Progress bar container
- `.milestone-highlights__progress-fill` - Progress fill element
- `.milestone-highlights__milestones` - Milestones container
- `.milestone-highlights__item` - Individual milestone item
- `.milestone-highlights__item--achieved` - Achieved milestone modifier
- `.milestone-highlights__marker` - Percentage marker
- `.milestone-highlights__label` - Milestone label
- `.milestone-highlights__badge` - Achievement badge

## Testing

The component includes comprehensive tests covering:

- **Helper Functions**: Sanitization, clamping, achievement detection
- **Rendering**: Component rendering, milestone display, progress bar
- **Accessibility**: ARIA attributes, keyboard navigation, screen reader support
- **Interactions**: Click handlers, keyboard events
- **Edge Cases**: Empty arrays, extreme values, special characters

Test coverage: **≥ 95%**

## Performance

- Uses `useMemo` to prevent unnecessary re-renders
- Efficient milestone enrichment logic
- Minimal DOM updates on prop changes

## Browser Support

- Modern browsers with ES6+ support
- Requires React 16.8+ (hooks)
- Accessible on all major screen readers

## Related Components

- `MilestoneFireworks` - Animated celebration overlay
- `CelebrationInsights` - Campaign celebration analytics
- `MilestonePersonalization` - Personalized milestone content
