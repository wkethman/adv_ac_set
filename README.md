Automates the AC set point and HVAC mode based on time of day, home occupancy, whether windows or doors are open, and outdoor temperature.

## Changelog

### v2.2 (2026-11-08)
- Versioned, self-documenting `description` field (what the blueprint does +
  changelog), so the automation UI shows the version and how it works.

### v2.1 (2026-11-08)
- **Fix: manual override gate now works.** The condition assumed the optional
  `override_toggle` input arrives as an `{entity_id: ...}` mapping, but
  storage-mode blueprint automations pass a plain entity-id string, so the
  gate always evaluated to `true` and the override was silently ignored.
  The new `override_entity` variable normalizes string, mapping, and empty
  (omitted) input, and the condition now skips runs while the toggle is `on`.
- **Fix: weather-unavailable fallback.** If the weather entity is missing or
  its `temperature` attribute can't be parsed, the blueprint now falls back
  to the target climate device's current mode instead of assuming cooling
  (previous behavior forced cooling mode whenever weather data was
  unavailable).
- **Fix: whitespace-fragile temperature template.** The default branch
  rendered a multi-line if/else whose output carried surrounding
  whitespace; the setpoint is now precomputed in the single-expression
  `selected_temp` variable so the service receives a native float.

### v1.x
- Initial release: two-mode (heat/cool) setpoint controller with
  occupancy, window/door, and weather-forecast logic, plus the
  (previously broken) manual override gate.
