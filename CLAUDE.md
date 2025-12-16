# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

svelte-calendar is a date picker component library for Svelte 5. It provides Day, Month, and Year pickers with responsive design, keyboard/touch/scroll support, and both inline and popover modes.

## Development Commands

```bash
npm run dev          # Start development server
npm run build        # Build for production (outputs to docs/)
npm run package      # Build the library package
npm run check        # Run svelte-check type checking
npm run lint         # Check formatting (prettier) and linting (eslint)
npm run format       # Auto-format code with prettier
```

## Architecture

### Component Hierarchy

The library exports two main entry points:
- `Datepicker` - Popover-based date picker with trigger button
- `InlineCalendar` - Embedded calendar without popover

Both wrap the core `Calendar` component which switches between three picker views:
- `DayPicker` - Grid of days with infinite scroll
- `MonthPicker` - Grid of months
- `YearPicker` - Grid of years

### Store-Driven State

All calendar state is managed via `src/lib/stores/datepicker.js`. The store provides:
- Selected date, start/end boundaries
- Active view tracking (days/months/years)
- Date manipulation methods (`setDay`, `setMonth`, `setYear`, `add`)
- Selectability checking against boundaries
- Calendar page generation

Components access the store via Svelte context using keys from `src/lib/context.js`.

### Virtual/Infinite Grid System

For animation performance, the library uses virtualized grids:
- `InfiniteGrid` - Spring-animated infinite scrolling (used by DayPicker)
- `FiniteGrid` - Fixed-item grid variant
- `Grid` - Basic CSS grid wrapper

The infinite grid only renders visible cells plus a small buffer, calculating positions via spring physics.

### Theming

Themes are defined in `src/lib/config/theme.js` (light/dark presets). Theme values become CSS custom properties prefixed with `--sc-theme-`. The `Theme` and `ThemeProvider` components handle CSS variable injection.

### Key Directories

- `src/lib/components/` - All Svelte components
- `src/lib/stores/` - Svelte stores
- `src/lib/config/` - Configuration (defaults, themes, scroll settings)
- `src/lib/directives/` - Svelte actions (scrollable, autofocus, etc.)
- `src/lib/docs/` - Documentation site components and examples
- `src/routes/` - SvelteKit routes for documentation site

### Library Exports

Main exports from `src/lib/index.js`:
- Components: `Datepicker`, `InlineCalendar`, `Calendar`, `Popover`, `InfiniteGrid`, `FiniteGrid`, `Scrollable`, `Swappable`, `Theme`, `Crossfade`, `CrossfadeProvider`
- Utilities: `themes` (light/dark), `scrollable` directive

## Dependencies

- `dayjs` - Date manipulation and formatting
- `just-throttle` - Event throttling for scroll handling

## Build Output

- `npm run build` outputs the documentation site to `docs/` (deployed to GitHub Pages)
- `npm run package` outputs the library to `package/` for npm publishing
