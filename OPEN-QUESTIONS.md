# Open questions and known defects

Decisions and defects found while writing `PACE-SRS` and `PACE-STS` in October 2026. When a question is settled, update the requirement and remove the question from this file.

## Questions for the owner

None at the moment.

## Targets still to measure

The code for these is in place, and they stay `Accepted` until someone measures them on the reference device:

| Requirement | Target | Test |
|---|---|---|
| PACE-SRS-48 | Session distance within 3 % over a known 5 km route | PACE-STS-38 |
| PACE-SRS-182 | At most 5 % of battery capacity per hour with the screen off | PACE-STS-162 |
| PACE-SRS-183 | Recording continues for at least 4 h with the screen off | PACE-STS-163 |

The wake lock added for reliable recording with the screen off counts against PACE-SRS-182, so that measurement is the one to watch.

## Known defects in the app

| Defect | Where | Requirement |
|---|---|---|
| The release build is signed with the debug key, so it can't be published as is. | `app/app/build.gradle.kts` | n/a |
