# Pacemark requirements overview

This document accompanies `PACE-SRS`. It holds the background an SRS needs that isn't itself a requirement: purpose, scope, context, users, limitations, assumptions and definitions. Nothing here is binding. Only the `shall` statements in `PACE-SRS` are requirements.

## Purpose

`PACE-SRS` states what Pacemark does for its user, and `PACE-STS` states how each requirement is checked. Together they let a maintainer or contributor decide whether a change is correct, and see what is built, what is intended and what is undecided.

## Scope

Pacemark is an Android app for recording runs, walks and hikes. It records a route and live metrics, stores workouts on the device, shows history and statistics, and exports the saved workouts as a backup that it can restore. It has no account and no cloud service.

These documents cover observable behaviour. They don't cover code structure, or the visual design, which lives in `design_handoff_pacemark/` in the app repository. Where the two differ, `PACE-SRS` wins: the handoff still describes goals, audio cues and a choice of hero metric, all of which the owner removed from the product. Debug-only features are out of scope.

**Not in v1:** the lock gesture, the Interval activity and its setup, workout notes and sharing from Summary.

## Product perspective

Pacemark stands alone on the user's phone. It interacts with:

| Interface | Used for | Required? |
|---|---|---|
| Location, through Google Play services | Route, distance and pace | Yes |
| Step counter sensor | Step count | No |
| Bluetooth Heart Rate Service 1.0 sensors | Heart rate | No |
| ANT+ heart rate sensors, through the ANT+ service app | Heart rate | No |
| Map provider, over the internet | Map styles and tiles | No; without a map key the app shows a placeholder |
| Android storage picker | Saving and opening the export archive | Only for export and import |
| GPX 1.1 readers such as training services | Reading exported workouts | Only for export |

## Product functions

- Recording a session, with pause, resume and finish, also in the background and from the notification.
- Calculating distance, pace, splits, elevation gain, calories and steps.
- Pairing a heart rate sensor, and recording heart rate.
- Showing the route on a map.
- Listing past workouts, showing their details, and deleting them.
- Showing weekly and monthly statistics and personal records.
- Exporting the saved workouts as a backup, and restoring them.
- Settings for units, map style and appearance.

## Privacy

Your data, like your tracks and logs, doesn't leave the device unless you export it. The app has no account, no analytics and no crash reporting, and it excludes its database from Android backup and device transfer.

Maps are the one thing to keep in mind. The app requests map tiles from the map provider over the internet. Those requests carry no workout data, but they show the provider your IP address and the area on screen, which for a recorded route is roughly where you went. Location fixes come from Google Play services, whose own data handling follows the device's Google settings and isn't controlled by the app.

## Users

People who run, walk or hike with a phone. They use the app outdoors, in motion and often with the phone locked, so live values have to read at a glance and recording has to continue unattended. No technical knowledge is assumed. Heart rate sensor owners are a smaller group. Some users are comfortable reading a GPX file, but the app doesn't depend on that.

## Limitations

- **Google Play services:** location comes only from Google Play services, so a device without them can't record a session. F-Droid isn't planned for v1, because its main repository doesn't accept the non-free libraries the app depends on.
- **Language:** the app's own text is in English only. Text drawn by Android follows the device language.
- **Theme:** the app is dark-only today. A light theme is accepted but not built.
- **Single device:** there is no sync. The app exports only the latest workout today. The accepted export and import move the saved workouts between devices through one archive.
- **Body weight:** the app has no user profile, so calories assume a body mass of 70 kg and are an estimate.

## Assumptions and dependencies

- The user grants the fine location permission. Without it the app can't record.
- Android keeps a foreground service running during a session. Aggressive vendor battery management can break this.
- Map features depend on a map key supplied when the app is built, and on the map provider's service.
- ANT+ features depend on the ANT+ service app being installed on the phone.
- Quality targets are measured on the reference device, a Sony Xperia XQ-DE54.

## Licensing

The app is licensed under GPL-3.0-or-later. It uses Apache-2.0 libraries, which are incompatible with GPLv2-only, so version 3 is the lowest possible. It also links two binary-only libraries that aren't free software, Google Play services and the ANT+ Android Plugin Library, so `LICENSE-EXCEPTION` in the app repository grants the additional permission that GPLv3 section 7 allows. These two libraries stay under their own licences.

## How the documents use ISO/IEC/IEEE 29148:2018

The documents are written following clauses 5.2.4 to 5.2.8 (construct, characteristics, language criteria and attributes), 6.5 (verification methods) and 9.6 (SRS content). This is guidance adopted for a small project, not a claim of formal conformance. It has been tailored as follows:

- **Attributes used:** identification, priority, rationale and type, plus status.
- **Attributes left out:** version comes from git history, and there is one owner, the maintainer. Risk and difficulty aren't tracked because they don't change any decision at this size.
- **Open values:** a requirement marked `TBD` has a value that is still undecided. Clause 5.2.6 allows this while a specification evolves, but the set isn't complete until each one is resolved. See `OPEN-QUESTIONS.md`.

## Definitions

| Term | Meaning |
|---|---|
| Active screen | The screen shown while a session exists |
| ANT+ device picker | The sensor selection screen shown by the ANT+ service app |
| ANT+ service app | The Android app that provides the ANT+ Plugins Service, through which the app reads ANT+ sensors |
| average pace | Elapsed time divided by session distance, in seconds per kilometre |
| BLE | Bluetooth Low Energy |
| Bluetooth permissions | The runtime permissions the app requests for BLE sensors: Bluetooth scan and Bluetooth connect on Android 12 and later, fine location on earlier versions |
| comparison period | The period of the same kind directly before the selected period: previous week, previous calendar month or previous calendar year |
| connection status | The state of the link to the paired sensor: searching, connected, signal lost, plugin missing, no ANT+ adapter, Bluetooth unavailable or sensor error |
| core metrics | The current pace, the distance and the elapsed time of a session |
| current heart rate | The heart rate of the latest reading from the paired sensor when that reading is a valid reading, absent otherwise |
| current pace | The pace over the most recent 10 s of location fixes, in seconds per kilometre |
| current week | The seven days from Monday 00:00 in the device time zone up to, but excluding, the following Monday 00:00, containing the present moment |
| distance unit | Kilometre or mile, as chosen in Settings |
| elapsed time | Time spent in the recording state, in whole seconds; saved as the duration of the workout |
| heart rate sensor | An external device that reports heart rate over BLE or ANT+ |
| History screen | The tab that lists saved workouts and the current week's totals |
| Home screen | The Track tab where the user chooses the activity type and selects Start |
| latest workout | The saved workout with the latest start time |
| location fix | One position reading from the device |
| map key | The access key for the map service, supplied when the app is built; the app has no map without it |
| map provider | The external service that supplies map styles and map tiles |
| map style cycle | The order Terrain, Minimal, Satellite, then back to Terrain |
| map view | The view of the Active screen that shows the map with the core metrics over it |
| metabolic equivalent | The energy cost of an activity relative to resting, used to estimate calories |
| paired sensor | The heart rate sensor the user last selected in Settings |
| pairing sheet | The bottom sheet in Settings where the user chooses a BLE sensor or starts ANT+ pairing |
| paused state | The session exists but is not counting time, distance and steps |
| peak pace | The shortest split pace among the splits covering a full distance unit, in seconds per kilometre |
| period selector | The week, month, year and whole-history control at the top of the Stats screen |
| placeholder graphic | A grid background with a fixed curve, identical for every session and not the recorded route, shown in place of the map |
| preference | A choice on the Settings screen that the app stores on the device: distance unit, map style, accent colour or Dark theme switch |
| recording state | The session is counting time and distance |
| reference altitude | The altitude that elevation gain is measured from; it moves as the altitude changes |
| reference device | The phone model used to measure performance targets, to be named by the owner |
| route | The ordered location fixes of a workout, each with latitude, longitude, timestamp and optional altitude, accuracy and heart rate values |
| selected period | The current week, current calendar month, current calendar year or whole saved history chosen with the period selector |
| session | A recording in progress, from Start until Finish |
| session distance | The distance in metres accumulated in a session from Start; the workout distance after Finish |
| Settings screen | The tab that holds preferences, data and sensor pairing |
| smoothed position | The position of a location fix averaged with earlier positions to reduce GPS jitter |
| split | The part of a workout covering one distance unit |
| split pace | The elapsed time of a split divided by the length of the split |
| stat view | The view of the Active screen that shows the session's live statistics |
| Stats screen | The tab that shows monthly totals and personal records |
| Summary screen | The screen shown after a session finishes, with the results of the session |
| the app | Pacemark on the user's device |
| valid reading | A heart rate above 0 bpm delivered by the paired sensor with sensor contact not reported as lost |
| workout | The saved record of a finished session |
| Workout Detail screen | The screen that shows one saved workout |
| workout figures | The distance, duration, average pace, peak pace, elevation gain, calories and step count of a workout |
