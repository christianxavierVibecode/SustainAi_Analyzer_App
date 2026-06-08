# SustainAI Analyzer App - Architecture Documentation

## Project Overview

**SustainAI** is a modern web application built with Next.js that provides AI-powered solutions for sustainable development. The application aligns with UN Sustainable Development Goals (SDGs) and offers multiple analyzer tools including CV Analysis, Plant Disease Detection, and Waste Classification.

### Tech Stack
- **Framework**: Next.js 16 (React 19)
- **Language**: TypeScript (95.2%)
- **Styling**: Tailwind CSS 4 (4.7%)
- **UI Components**: Radix UI
- **Form Handling**: React Hook Form + Zod Validation
- **Charts**: Recharts
- **Animations**: Tailwind CSS Animations
- **Analytics**: Vercel Analytics
- **Package Manager**: pnpm 10.28.2

---

## Architecture Overview

The application follows a **modular component-based architecture** with clear separation of concerns:

```
SustainAi_Analyzer_App/
├── app/                          # Next.js App Router
│   ├── layout.tsx               # Root layout with providers
│   ├── page.tsx                 # Homepage composition
│   ├── globals.css              # Global styles
│   ├── api/                     # API routes
│   ├── dashboard/               # Dashboard routes
│   ├── CV-Analyzer/             # CV Analysis feature
│   ├── Plant-Disease-Analyzer/  # Plant Disease Analysis feature
│   ├── Waste-Analyzer/          # Waste Classification feature
│   ├── Privacy-Policy/          # Legal pages
│   └── Terms-of-Service/
├── components/                   # Reusable React components
│   ├── ui/                      # Base UI components (Radix UI)
│   ├── chat/                    # Chat interface components
│   ├── navbar.tsx               # Navigation
│   ├── hero-section.tsx         # Hero with canvas animations
│   ├── challenges-section.tsx   # Challenges presentation
│   ├── solutions-section.tsx    # Solutions showcase
│   ├── how-it-works-section.tsx # Feature explanation
│   ├── sdg-section.tsx          # SDG alignment
│   ├── team-section.tsx         # Team information
│   ├── cta-section.tsx          # Call-to-action
│   ├── footer.tsx               # Footer
│   ├── theme-provider.tsx       # Dark/Light theme
│   ├── language-switcher.tsx    # i18n switcher
│   ├── scroll-to-top-button.tsx # Scroll utility
│   └── navigation-progress.tsx  # Route loading indicator
├── hooks/                        # Custom React hooks
│   └── use-in-view.ts           # Intersection Observer hook
├── lib/                          # Utility functions & contexts
│   ├── language-context.tsx     # i18n context
│   ├── translations.ts          # Multi-language strings
│   └── utils.ts                 # Helper utilities
├── styles/                       # Global stylesheets
├── public/                       # Static assets
└── package.json                 # Dependencies & scripts

```

---

## Core Architecture Layers

### 1. **Presentation Layer (Components)**
The UI is composed of reusable, isolated components following React best practices:

- **Page Layout Components**: Navbar, HeroSection, Footer - provide page structure
- **Feature Sections**: Solutions, Challenges, HowItWorks - showcase capabilities
- **UI Components**: Base components from `components/ui/` using Radix UI for accessibility
- **Utility Components**: ScrollToTopButton, NavigationProgress, LanguageSwitcher

### 2. **State Management Layer (Context & Hooks)**
- **LanguageContext**: Manages i18n state with localStorage persistence
- **useInView Hook**: Intersection Observer pattern for scroll-triggered animations
- **React Hook Form**: Handles form state in analyzer features

### 3. **Utility Layer (lib/)**
- **Language Context**: Provides translation function `t()` globally
- **Translations**: Centralized i18n dictionary (English & Indonesian)
- **Utils**: Common helper functions (cn for Tailwind class merging)

### 4. **Route Structure (App Router)**
- **Static Pages**: Homepage composition with multiple sections
- **Feature Routes**: CV-Analyzer, Plant-Disease-Analyzer, Waste-Analyzer
- **API Routes**: Backend handlers in `app/api/`
- **Legal Pages**: Privacy Policy, Terms of Service

---

## Core Flow Logic Examples

### 1. **Language Management Flow**

**File**: `lib/language-context.tsx`

```typescript
// Context Flow: Initialization → Storage → Translation
┌─────────────────────────────────────────────────────────┐
│ LanguageProvider Component                              │
├─────────────────────────────────────────────────────────┤
│ 1. On Mount:                                            │
│    - Check localStorage for saved language              │
│    - Default to 'en' if not found                       │
│                                                         │
│ 2. setLanguage() Action:                                │
│    - Update state                                       │
│    - Persist to localStorage                            │
│    - Re-render consuming components                     │
│                                                         │
│ 3. t() Translation Function:                            │
│    - Look up key in current language dict               │
│    - Return translated string or fallback key           │
└─────────────────────────────────────────────────────────┘

// Consumer Usage:
const { language, setLanguage, t } = useLanguage()
setLanguage('id')  // Switch to Indonesian
const title = t('hero.title')  // Get translated title
```

**Flow Sequence**:
```
User Switches Language
    ↓
setLanguage('id') called
    ↓
State Updated + localStorage.setItem('sustainai-language', 'id')
    ↓
All useLanguage() consumers re-render
    ↓
Component calls t('key') with new language
    ↓
Translation dictionary lookup (id → value)
    ↓
UI Updated with Indonesian text
```

**Key Implementation Details**:
- Uses `createContext()` + `useContext()` pattern
- `useEffect()` on mount loads language from localStorage
- `useCallback()` optimizes `setLanguage()` and `t()` functions
- Defaults to 'en' (English) if no stored preference
- Validates stored value against allowed languages ('en', 'id')

---

### 2. **Scroll-Triggered Animation Flow**

**File**: `hooks/use-in-view.ts`

```typescript
// IntersectionObserver Pattern: Element Detection → Animation Trigger
┌──────────────────────────────────────────────────────┐
│ useInView Hook                                       │
├──────────────────────────────────────────────────────┤
│ 1. Create Ref:                                       │
│    - Attach to DOM element                           │
│                                                      │
│ 2. Setup IntersectionObserver:                       │
│    - threshold: 0.1 (trigger at 10% visible)         │
│    - rootMargin: "0px" (adjustment area)             │
│    - triggerOnce: true (animate only once)           │
│                                                      │
│ 3. Detection Logic:                                  │
│    - Monitor when element enters viewport            │
│    - Set isInView = true                             │
│    - If triggerOnce: stop observing                  │
│    - Otherwise: toggle isInView on in/out            │
│                                                      │
│ 4. Cleanup:                                          │
│    - Disconnect observer on unmount                  │
└──────────────────────────────────────────────────────┘

// Consumer Usage Example:
const { ref, isInView } = useInView()
return (
  <div ref={ref} className={isInView ? 'animate-fadeIn' : ''}>
    Content animates when scrolled into view
  </div>
)
```

**Flow Sequence**:
```
Component Mounts
    ↓
useInView() creates IntersectionObserver on ref element
    ↓
User Scrolls
    ↓
Element enters 10% threshold
    ↓
isInView = true (triggers re-render)
    ↓
Conditional classes applied: animate-fadeIn, animate-slideUp, etc.
    ↓
Observer unobserves (triggerOnce = true)
    ↓
Animation runs once during scroll
```

**Performance Benefits**:
- Lazy animation triggering (only when visible)
- Single animation per element (triggerOnce)
- Browser-native Intersection Observer API (efficient)
- Automatic cleanup on component unmount
- Configurable threshold and margin options

---

### 3. **Page Composition Flow**

**File**: `app/page.tsx`

```typescript
// Component Assembly Pattern: Modular Section Loading
┌────────────────────────────────────────────────────┐
│ HomePage Component                                 │
├────────────────────────────────────────────────────┤
│ Structure:                                         │
│ 1. Navbar (sticky header)                          │
│ 2. HeroSection (with canvas animations)            │
│ 3. ChallengesSection (problem definition)          │
│ 4. VisionSection (goal/mission)                    │
│ 5. SolutionsSection (feature showcase)             │
│ 6. HowItWorksSection (workflow)                    │
│ 7. SDGSection (sustainability alignment)           │
│ 8. TeamSection (team info)                         │
│ 9. CTASection (call-to-action)                     │
│ 10. Footer                                         │
│ 11. ScrollToTopButton (utility)                    │
│                                                    │
│ Each section has:                                  │
│ - Unique id for anchor links (#challenges, etc)   │
│ - useInView hook for scroll animations             │
│ - useLanguage hook for i18n                        │
│ - Responsive design with Tailwind                  │
└────────────────────────────────────────────────────┘

// Anchor Navigation Flow:
User clicks "Learn More" → href="#solutions"
    ↓
Browser scrolls to #solutions
    ↓
SolutionsSection becomes visible (crosses 10% threshold)
    ↓
useInView triggers (isInView = true)
    ↓
Animations play (fade in, slide up)
```

**Architecture Pattern**:
- **Composition Pattern**: Each section is a standalone, reusable component
- **Semantic HTML**: Proper `<section>` with unique `id` for accessibility
- **Progressive Enhancement**: Works without JavaScript (fallback to CSS)
- **Separation of Concerns**: Navigation separate from content sections

---

### 4. **Layout Provider Pattern**

**File**: `app/layout.tsx`

```typescript
// Root Layout: Provider Architecture
┌──────────────────────────────────────────────────┐
│ RootLayout Component                             │
├──────────────────────────────────────────────────┤
│ Providers Hierarchy:                             │
│                                                  │
│ <html>                                           │
│   <body>                                         │
│     <LanguageProvider>                           │
│       Makes useLanguage() available globally    │
│                                                  │
│       <Suspense>                                 │
│         <NavigationProgress />                   │
│       </Suspense>                                │
│                                                  │
│       {children}  // All pages rendered here     │
│                                                  │
│       <Toaster />  // Global toast notifications │
│     </LanguageProvider>                          │
│                                                  │
│     <Analytics />  // Vercel analytics tracking  │
│   </body>                                        │
│ </html>                                          │
│                                                  │
│ Benefits:                                        │
│ - Single source of language state                │
│ - Toast notifications available everywhere       │
│ - Navigation progress bar for all routes         │
│ - Analytics tracking on all pages                │
└──────────────────────────────────────────────────┘
```

**Flow Sequence**:
```
App Starts
    ↓
RootLayout mounts providers
    ↓
LanguageProvider initializes (checks localStorage)
    ↓
Page component renders inside providers
    ↓
All child components can use:
  - useLanguage() for i18n
  - useInView() for scroll animations
  - toast() for notifications
  - Navigation progress on route changes
```

**Provider Benefits**:
- **Centralized Configuration**: Single point for app-wide setup
- **Reusable Context**: LanguageProvider available to all routes
- **Suspense Boundary**: Safely handles async components
- **Global Error Handling**: Toaster for user notifications
- **Analytics**: Vercel integration for monitoring

---

### 5. **Hero Section Canvas Animation Flow**

**File**: `components/hero-section.tsx`

```typescript
// Canvas Animation Pattern: Setup → Loop → Cleanup
┌───────────────────────────────────────────────────────┐
│ HeroSection Component with Canvas Animation          │
├───────────────────────────────────────────────────────┤
│ 1. Canvas Setup:                                      │
│    - Get 2D context from canvas ref                   │
│    - Resize canvas to window dimensions               │
│    - Listen for window resize events                  │
│                                                       │
│ 2. Particle System:                                   │
│    - Create 80 particles with:                        │
│      * Random x, y position                           │
│      * Random velocity (vx, vy)                       │
│      * Random size (1-3px)                            │
│      * Random opacity (0.2-0.7)                       │
│                                                       │
│ 3. Wave Animation (3 layers):                         │
│    - Sin wave calculation: height + sin(x + offset)   │
│    - Dual frequency sine for organic effect           │
│    - Gradient color: green → cyan transition          │
│    - Stacked layers with opacity variation            │
│                                                       │
│ 4. Animation Loop (requestAnimationFrame):            │
│    - Clear canvas each frame                          │
│    - Draw 3 wave layers                               │
│    - Update particles position (wrap edges)           │
│    - Draw particles as circles                        │
│    - Increment wave offset for movement               │
│    - ~60 FPS smooth animation                         │
│                                                       │
│ 5. Cleanup:                                           │
│    - Cancel animation frame on unmount                │
│    - Remove resize listener                           │
└───────────────────────────────────────────────────────┘

// Animation Details:
Wave: sin(x * 0.01 + offset) * 50 + sin(x * 0.02) * 30
  - Creates smooth undulating motion
  - Multiple frequencies create complexity
  - Offset increases each frame (animation)
  
Particles: Random walk with edge wrapping
  - Continuous velocity movement
  - X < 0 → X = canvas.width (wrap around)
  - Y < 0 → Y = canvas.height (wrap around)
```

**Flow Sequence**:
```
Component Mounts
    ↓
useEffect initializes canvas context
    ↓
Create 80 particles with random properties
    ↓
Start requestAnimationFrame loop
    ↓
Each Frame (60 FPS):
    - Clear canvas
    - Calculate 3 wave layers from math functions
    - Update particle positions (add velocity)
    - Draw waves with gradient
    - Draw particles as semi-transparent circles
    - Increment wave offset
    ↓
User resizes window
    ↓
Resize event → update canvas dimensions
    ↓
Animation continues at new resolution
    ↓
Component unmounts
    ↓
Cancel animation frame + cleanup listeners
```

**Technical Implementation**:
- **requestAnimationFrame**: Syncs with browser refresh rate (smooth 60 FPS)
- **Canvas 2D Context**: Direct pixel manipulation for performance
- **Gradient Colors**: Linear gradient from green → cyan for visual appeal
- **Wave Algorithms**: Sine wave with multiple frequencies for complexity
- **Memory Management**: Cleanup event listeners and animation frames

---

### 6. **Form Handling Flow (Analyzer Features)**

**Pattern**: React Hook Form + Zod Validation

```typescript
// Analyzer Form: Data Collection → Validation → Submission
┌──────────────────────────────────────────────────┐
│ Analyzer Form Component                          │
├──────────────────────────────────────────────────┤
│ 1. Form Setup:                                   │
│    - useForm() from react-hook-form              │
│    - zodResolver() for validation schema         │
│    - Define validation rules                     │
│                                                  │
│ 2. Form Rendering:                              │
│    - Controlled inputs (register fields)         │
│    - Real-time validation feedback               │
│    - Error display                               │
│                                                  │
│ 3. On Submit:                                    │
│    - Validate all fields against schema          │
│    - If invalid: show errors, stop               │
│    - If valid: prepare data payload              │
│    - Send to API endpoint                        │
│    - Loading state during request                │
│    - Success/error toast notification            │
│                                                  │
│ 4. Result Display:                               │
│    - Parse API response                          │
│    - Render analysis/prediction results          │
│    - Option to export or share                   │
└──────────────────────────────────────────────────┘
```

**Validation Pipeline**:
```
User Input
    ↓
Form Submit (onSubmit handler)
    ↓
Zod Schema Validation
    ├─ Field type checking
    ├─ Length/format validation
    └─ Custom validations
    ↓
If Errors: Display error messages to user
    ↓
If Valid: Prepare FormData or JSON payload
    ↓
API Request (fetch/axios)
    ↓
Handle Response (success/error)
    ↓
Display Results or Error Toast
```

---

## Data Flow Summary

### Global State Management
```
┌─────────────────┐
│ LanguageContext │
├─────────────────┤
│ language: 'en'  │
│ setLanguage()   │
│ t() function    │
└────────┬────────┘
         │
    Provided to all components
         │
    Available via useLanguage()
```

### Component Communication
```
Layout (Root)
├── Navbar
│   └── LanguageSwitcher (writes to context)
├── Page Content
│   ├── HeroSection (reads language)
│   ├── SolutionsSection (scroll animation + i18n)
│   └── Other Sections (same pattern)
└── Footer
    └── Language display based on context
```

---

## Performance Optimizations

1. **Code Splitting**: Dynamic imports for heavy components
2. **Image Optimization**: Next.js Image component for static assets
3. **CSS-in-JS**: Tailwind CSS for minimal bundle size
4. **Component Memoization**: useCallback/useMemo for expensive operations
5. **Intersection Observer**: Lazy animation triggering (useInView)
6. **RequestAnimationFrame**: Smooth 60 FPS canvas animations
7. **Canvas Optimization**: Efficient particle system and wave rendering
8. **Lazy Loading**: Components load only when needed

---

## Internationalization (i18n) Architecture

```
lib/translations.ts
├── translations.en: { hero: { title: "...", ... }, ... }
└── translations.id: { hero: { title: "...", ... }, ... }

lib/language-context.tsx
├── Manages current language state
├── Persists to localStorage
└── Provides t() function for key lookup

components/language-switcher.tsx
├── UI to change language
└── Calls setLanguage() in context
```

**Usage in components**:
```typescript
const { t, language } = useLanguage()
<h1>{t('hero.title')}</h1>
```

**Supported Languages**:
- English (en)
- Indonesian (id)

---

## Best Practices Implemented

✅ **Type Safety**: Full TypeScript with strict mode  
✅ **Component Isolation**: Single responsibility per component  
✅ **Accessibility**: Radix UI components (WCAG compliant)  
✅ **Performance**: requestAnimationFrame, lazy animations  
✅ **Code Organization**: Clear folder structure and naming  
✅ **Error Handling**: Try-catch, validation with Zod  
✅ **State Management**: Context API for global state  
✅ **Styling**: Utility-first CSS with Tailwind  
✅ **Responsive Design**: Mobile-first approach  
✅ **i18n Support**: Multi-language from the ground up  
✅ **Memory Management**: Proper cleanup of event listeners  
✅ **Browser APIs**: Native Intersection Observer, Canvas  

---

## Getting Started

### Installation
```bash
pnpm install
```

### Development
```bash
pnpm dev
```
Runs on `http://localhost:3000`

### Build
```bash
pnpm build
pnpm start
```

### Linting
```bash
pnpm lint
```

---

## Future Architecture Considerations

1. **API Integration**: Add backend service for analyzer AI models
2. **Database**: User data persistence (PostgreSQL + Prisma)
3. **Authentication**: NextAuth for user management
4. **Caching**: SWR or React Query for data fetching
5. **Testing**: Jest + Playwright for unit and E2E tests
6. **Monitoring**: Error tracking (Sentry integration)
7. **CDN**: Cloudflare for static asset delivery
8. **Real-time Features**: WebSocket for live analysis updates

---

## Contributing

When adding new features:
1. Create components in `components/` with clear responsibility
2. Use `lib/language-context.tsx` for i18n strings
3. Implement responsive design with Tailwind
4. Use TypeScript for type safety
5. Follow existing component patterns
6. Test on mobile and desktop
7. Update this documentation with architectural changes

---

**Last Updated**: 2026-06-08  
**Tech Stack Version**: Next.js 16, React 19, TypeScript 5  
**Repository**: christianxavierVibecode/SustainAi_Analyzer_App
