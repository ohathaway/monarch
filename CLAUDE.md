# Claude Development Context

This file contains important context and commands for Claude Code to work effectively with this Monarch TTRPG Terminal project.

## Project Overview

This is a Nuxt 4 application that recreates a TTRPG (tabletop role-playing game) terminal interface for the Mothership RPG system. The app serves as a digital contractor selection terminal where players choose missions and receive contract codes for their game master.

**Key Context:**
- The original HTML wireframe is located at `/doc/mothership_boot_screen.html`
- This wireframe was created by the user's son-in-law using Claude Chat
- The goal is to faithfully recreate this wireframe as a dynamic Nuxt 4 application
- Future plans include integrating with a cloud-hosted Strapi CMS for dynamic content
- The project uses the full Cloudflare platform suite via NuxtHub

## Technology Stack

- **Framework**: Nuxt 4 (compatibility version 4)
- **Frontend**: Vue 3 with Composition API
- **Styling**: TailwindCSS via @nuxt/ui
- **Build Tool**: Vite (via Nuxt)
- **Package Manager**: pnpm
- **Hosting**: Cloudflare Pages via NuxtHub
- **Backend**: Cloudflare Workers
- **Future CMS**: Strapi (cloud-hosted)

## Development Commands

```bash
# Install dependencies
pnpm install

# Start development server
pnpm dev

# Build for production
pnpm build

# Deploy to Cloudflare via NuxtHub
pnpm deploy

# Preview deployment locally
pnpm preview

# Lint code (ESLint with stylistic rules)
pnpm lint

# Type checking
npx vue-tsc --noEmit
```

## Key Files and Structure

```
monarch/
├── app/                    # Nuxt 4 app directory structure
├── server/                 # API routes and server-side logic
├── doc/                    # Contains the original HTML wireframe
│   └── mothership_boot_screen.html  # Original wireframe to recreate
├── nuxt.config.ts          # Nuxt configuration with Cloudflare settings
└── package.json            # Dependencies and scripts
```

## Important Implementation Notes

1. **Wireframe Recreation**: The HTML file at `/doc/mothership_boot_screen.html` contains the complete terminal interface that needs to be recreated as Vue components.

2. **Terminal Aesthetics**: The design features:
   - Green text on black background (#00ff00 on #000)
   - Courier Prime monospace font
   - Typewriter animations with realistic delays
   - Loading bars and boot sequence animations
   - ASCII art logo for "MONARCH"

3. **Interactive Features**:
   - Boot sequence animation
   - Mission selection interface
   - Contract code generation
   - Easter egg content
   - Responsive design

4. **Mission Types** (from wireframe):
   - VR Dead - Virtual Reality Systems Maintenance
   - Another Bug Hunt - Colonial Site Inspection
   - Gradient Descent - Deep Space Survey Mission
   - Owe My Soul - Employee Equity Program

## Nuxt 4 Configuration Notes

The project uses:
- `compatibilityVersion: 4` for Nuxt 4 features
- `@nuxthub/core` for Cloudflare integration
- `@nuxt/ui` for TailwindCSS and components
- `workers: true` in hub config for Cloudflare Workers

## ESLint Configuration

- Single quotes preferred
- No comma dangle
- Stylistic rules enforced

## Future Development Plans

1. **Phase 1** (Current): Static recreation of wireframe
2. **Phase 2**: Dynamic content integration with Strapi CMS
3. **Phase 3**: Multi-player features and advanced functionality

## Testing and Deployment

- Use `pnpm lint` before committing changes
- Deploy via `pnpm deploy` uses NuxtHub for Cloudflare deployment
- Preview deployments available via `pnpm preview`

## Common Tasks

When working on this project, you'll likely need to:

1. **Convert HTML to Vue**: Transform the static HTML wireframe into Vue components
2. **Implement animations**: Recreate the typewriter and loading animations
3. **Add interactivity**: Handle mission selection and contract generation
4. **Style with Tailwind**: Use Nuxt UI and TailwindCSS for styling
5. **Manage state**: Handle application state for terminal interface

## Git Workflow

- Main branch: `master`
- Keep commits focused and descriptive
- Test locally before deploying to Cloudflare