# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

This is an Xcode project. Use `xcodebuild` from the command line:

```bash
# Build for iOS Simulator
xcodebuild -scheme time -destination 'platform=iOS Simulator,name=iPhone 16' build

# Run unit tests
xcodebuild -scheme time -destination 'platform=iOS Simulator,name=iPhone 16' test

# Run UI tests
xcodebuild -scheme time -destination 'platform=iOS Simulator,name=iPhone 16' -only-testing:timeUITests test

# Run a single test
xcodebuild -scheme time -destination 'platform=iOS Simulator,name=iPhone 16' -only-testing:timeTests/timeTests/testExample test
```

## Architecture

This is a SwiftUI iOS app using SwiftData for local persistence.

**Core Components:**
- `timeApp.swift` - App entry point, configures the shared `ModelContainer` for SwiftData
- `ContentView.swift` - Main UI with a `NavigationSplitView` for list/detail layout
- `Item.swift` - SwiftData `@Model` class representing stored items

**Data Layer:**
- Uses SwiftData with a `ModelContainer` configured for disk persistence
- The `@Query` macro fetches items reactively in views
- Model context is accessed via `@Environment(\.modelContext)`

**Capabilities (from entitlements):**
- CloudKit integration enabled (iCloud sync ready)
- Push notifications configured (development environment)
