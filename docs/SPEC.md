# V0.1 Spec — Timer Core

## Scope

Build the foundational timer functionality: starting, stopping, and editing sessions. Single-child assumption (no child picker yet).

## Data Model

### Session

| Property | Type | Notes |
|----------|------|-------|
| `id` | UUID | Auto-generated |
| `type` | `SessionType` enum | `.eat` or `.sleep` |
| `startTime` | Date | Default: now. Editable (can backdate). |
| `endTime` | Date? | Nil while timer running. Editable after stop. |
| `eatMethod` | `EatMethod` enum? | `.nursing` or `.bottle`. Only when `type == .eat`. |
| `bottleType` | `BottleType` enum? | `.breastMilk` or `.formula`. Only when `eatMethod == .bottle`. |
| `bottleAmount` | Double? | Ounces. Only when `eatMethod == .bottle`. |

### Enums

```swift
enum SessionType: String, Codable {
    case eat
    case sleep
}

enum EatMethod: String, Codable {
    case nursing
    case bottle
}

enum BottleType: String, Codable {
    case breastMilk
    case formula
}
```

### Computed Properties

```swift
var duration: TimeInterval? {
    guard let endTime else { return nil }
    return endTime.timeIntervalSince(startTime)
}

var isActive: Bool {
    endTime == nil
}
```

## Screens

### 1. Timer Screen (Primary)

**Entry point of the app.**

#### States

**A. No active timer**
- Prominent "Start" button
- Defaults ready: type = Eat, method = Nursing

**B. Timer running**
- Elapsed time display (live updating)
- Type toggle: Eat ↔ Sleep
- If Eat selected:
  - Method toggle: Nursing ↔ Bottle
  - If Bottle selected:
    - Bottle type picker: Breast Milk | Formula
    - Amount input (ounces, numeric)
- Editable start time (date/time picker, constrained to past)
- "Stop" button

**C. Timer just stopped (confirmation state, optional)**
- Summary of session
- Editable end time (must be after start time)
- "Save" / "Done" to finalize

#### Behavior Notes

- Changing type from Eat → Sleep clears eat-specific fields
- Changing method from Bottle → Nursing clears bottle-specific fields
- Start time cannot be in the future
- End time must be after start time

### 2. Timeline Screen (Secondary)

**Chronological log of all past sessions.**

#### Display

- List sorted by `startTime` descending (most recent first)
- Each row shows:
  - Type icon (eat/sleep)
  - Start time
  - Duration (formatted: "1h 23m" or "45m")
  - For eat sessions: method + bottle details if applicable

#### Interactions

- Tap row → Edit session (navigate to edit screen or modal)

### 3. Edit Session Screen/Modal

**Modify a past session.**

- Same fields as Timer Screen, but:
  - Both start and end time are editable
  - End time required (session is already completed)
- Delete option
- Validation: end time must be after start time

## User Flows

### Start and Complete a Timer

1. User opens app → Timer Screen (no active timer)
2. Taps "Start" → Timer begins, defaults applied
3. (Optional) Adjusts type, method, bottle details, start time
4. Taps "Stop" → Timer stops, end time recorded
5. (Optional) Edits end time
6. Session saved to timeline

### Edit a Past Session

1. User navigates to Timeline
2. Taps a session row
3. Edit screen appears with current values
4. User modifies fields
5. Saves → Returns to timeline with updated data

## Out of Scope for V0.1

- Multi-child support
- Widgets / Live Activities
- Notifications
- "When next" predictions
- Data export / sync
- Onboarding

## Open Questions

- Should we show the active timer on Timeline screen as a pinned item?
- Confirmation step after stopping—worth it or unnecessary friction?
