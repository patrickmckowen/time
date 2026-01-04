# Baby Tracker App

## Project Overview

A focused iOS app helping parents track their child's sleep and eating times. The core value proposition is answering "when does my child need to sleep/eat next?" through fast logging and at-a-glance status.

**Design philosophy:** The app is the control panel (logging), widgets/Live Activities are the dashboard (checking). Users should rarely need to open the app.

## Tech Stack

- **UI:** SwiftUI
- **Persistence:** SwiftData
- **Target:** iOS 26+
- **Architecture:** MVVM with SwiftData models as source of truth

## Code Conventions

### Swift Style
- Use Swift 6 concurrency patterns
- Prefer value types where appropriate
- Use `@Observable` macro for view models
- Keep views thin—logic belongs in view models or model extensions

### SwiftData
- Models are the single source of truth
- Use `@Query` in views for reactive data
- Avoid unnecessary fetch requests—trust SwiftData's change tracking

### File Organization
```
App/
├── Models/          # SwiftData models
├── Views/           # SwiftUI views
├── ViewModels/      # @Observable view models
├── Components/      # Reusable UI components
└── Utilities/       # Extensions, helpers
```

### Naming
- Views: `[Feature]View.swift` (e.g., `TimerView.swift`)
- Models: Singular nouns (e.g., `Session.swift`, `Child.swift`)
- View Models: `[Feature]ViewModel.swift`

## Key Architecture Decisions

1. **Single active timer:** Only one session can be in-progress at a time (per child, when multi-child support is added).

2. **Session as core entity:** Everything revolves around the `Session` model—sleep and eat are types within a unified model, not separate entities.

3. **Calculated duration:** Duration is always derived from `endTime - startTime`, never stored directly.

4. **Progressive disclosure:** Conditional fields (bottle type, amount) only appear when relevant selections are made.

## Future Considerations (Not in V0.1)

- Multi-child support (Child becomes top-level entity)
- Widgets and Live Activities
- "When next" predictive logic
- Notifications/reminders
- Data export
