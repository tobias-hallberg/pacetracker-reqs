# Open questions and known defects

Decisions and defects found while writing `PACE-SRS` and `PACE-STS` in October 2026. When a question is settled, update the requirement and remove the question from this file.

## Questions for the owner

None at the moment.

## Known defects in the app

These are for the app backlog. **Checked** means I read the code myself. **Reported** means a review agent found it from the code, and I haven't confirmed it independently. Nothing here was run on a device.

| # | Defect | Where | Requirement | Confidence |
|---|---|---|---|---|
| 1 | Live pace reads about 10 % fast: a steady 4 m/s shows 227 s/km instead of 250, because the oldest sample's distance is included. `PaceWindowTest` fails because of this. | `PaceWindow.kt:22-23` | PACE-SRS-50 | Checked |
| 2 | Start replaces a running session, reachable by pressing Back from the Active screen. | `HomeViewModel.kt:57`, `LiveSessionController.kt:22-23` | PACE-SRS-188 | Checked |
| 3 | The backup rules don't cover the preferences file. | `data_extraction_rules.xml:3-10` | PACE-SRS-152 | Checked |
| 4 | A schema version with no migration erases every workout. | `DatabaseModule.kt:30` | PACE-SRS-190 | Checked |
| 5 | The data size row measures only the main database file. | `SettingsViewModel.kt:66-67` | PACE-SRS-174 | Checked |
| 6 | The notification always shows kilometres. | `NotificationHelper.kt:26-32` | PACE-SRS-29 | Reported |
| 7 | A session is held only in memory, so process death loses it. | `LiveSessionHolder.kt` | PACE-SRS-41 | Reported |
| 8 | A failed workout save has no exception handler, so full storage ends the app rather than offering a retry. | `SummaryViewModel.kt:28-32` | PACE-SRS-39 | Reported |
| 9 | A location permission revoked during a session crashes the process. | `TrackingService.kt:174-181` | PACE-SRS-43 | Reported |
| 10 | After a sticky restart with no session, the notification stays and the service never stops. | `TrackingService.kt:68-71` | PACE-SRS-42 | Reported |
| 11 | The previous heart rate comes back after a reconnect, reaching the display and the recorded data. | `ReconnectingHeartRateMonitor.kt:51` | PACE-SRS-88 | Reported |
| 12 | A route with exactly 1 location fix may crash every screen showing a map. Not confirmed against the map library. | `MapLibreComposeView.kt:74-77` | PACE-SRS-101 | Reported |
| 13 | The map style is set again on each new location fix, which may flicker and reset panning. | `MapLibreComposeView.kt:69-83` | PACE-SRS-100 | Reported |
| 14 | Stats groups months in UTC but days in local time, so a workout near midnight can land in the wrong month. | `WorkoutDao.kt:94,108` | PACE-SRS-126 | Reported |
| 15 | A workout with 0 m stores a peak pace of 0, which hides the real monthly peak pace. | `WorkoutDao.kt:100` | PACE-SRS-131 | Reported |
| 16 | Export reports success when no output stream is available, and coordinates can be written in exponent notation, which GPX 1.1 doesn't allow. | `SettingsViewModel.kt:165-169`, `GpxExporter.kt` | PACE-SRS-173, PACE-SRS-157 | Reported |
| 17 | The release build is signed with the debug key, so it can't be published as is. | `build.gradle.kts:45` | n/a | Reported |
