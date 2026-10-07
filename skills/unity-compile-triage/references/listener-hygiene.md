# UI Listener Hygiene Patterns
**Referenced from:** `skills/unity-compile-triage/SKILL.md` step 4, step 5, REFERENCES
**Source:** prompt_dump.txt, PROJECT_MEMORY.md, UI_SYSTEM_REPORT.md

## The 48≠47 Problem

**Measured:** 48 listeners registered, 47 expected at boot
**Root Cause:** Duplicate `AddListener` calls without corresponding `RemoveListener` in `OnDisable`
**Impact:** Memory leaks, double-invocation, RESTYLE spam

## Register-Once Pattern

```csharp
// WRONG — adds every enable
void OnEnable() {
    SomeEvent.OnAction += HandleAction;
}

// CORRECT — register once, remove on disable
bool _registered;
void OnEnable() {
    if (!_registered) {
        SomeEvent.OnAction += HandleAction;
        _registered = true;
    }
}
void OnDisable() {
    if (_registered) {
        SomeEvent.OnAction -= HandleAction;
        _registered = false;
    }
}
```

## Idempotent Restyle Pattern

```csharp
// WRONG — called every frame/theme change
void Update() {
    ApplyRestyle(); // Spams console
}

// CORRECT — boot + theme change only
bool _restyled;
void OnEnable() {
    if (!_restyled) {
        ApplyRestyle();
        _restyled = true;
    }
}
// Theme change event
void OnThemeChanged() {
    ApplyRestyle(); // Allow re-apply on theme change
}
```

## Boot Assertion

```csharp
[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]
static void AssertListenerCount() {
    int expected = 48; // Measured constant from PROJECT_MEMORY §1
    int actual = CountAllListeners(); // Implement via reflection/event inspection
    if (actual != expected) {
        Debug.LogError($"[ListenerHygiene] Count mismatch: {actual} != {expected}");
        // Dump listener IDs for forensics
        DumpListenerIds();
    }
}
```

## Listener Dump Forensics

```csharp
static void DumpListenerIds() {
    // Reflection to enumerate all UnityEvent listeners
    // Log: Event name | Listener count | Target object | Method name
    // Filter for duplicates (same target+method on same event)
}
```

## Verification Protocol

1. Fresh boot → export console → verify 0 RESTYLE lines
2. Play 60s → export console → verify 0 RESTYLE lines
3. Theme change → verify restyle runs once (not per frame)
4. Scene reload → verify listener count == 48
5. Log proof line in console: `[ListenerHygiene] Assertion passed: 48==48`

## Cross-References

- `skills/ui-listeners-hygiene/SKILL.md` — Full skill for listener management
- `UI_SYSTEM_REPORT.md` R2.1 — 34 listeners through one UIWire funnel (UIRoot.WIRE_EXPECTED)
- `PROJECT_MEMORY.md` L-148 — Dead log channel: execute_code console output untrustworthy