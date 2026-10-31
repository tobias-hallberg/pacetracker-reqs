# Pacemark requirements

Requirements and test specifications for [Pacemark](https://github.com/tobias-hallberg/pacetracker), an Android running tracker. The app repository includes this repository as a submodule at `reqs/`.

The documents are edited with [Mellon](https://github.com/tobias-hallberg/mellon-req-tool), which stores each requirement as a small XML file. You don't need Mellon to read them or review a pull request.

## Documents

| Document | Contents |
|---|---|
| `PACE-SRS` | Software Requirements Specification: what the app shall do |
| `PACE-STS` | Software Test Specification: how each requirement is checked |
| [`OVERVIEW.md`](OVERVIEW.md) | Purpose, scope, users, limitations, assumptions and definitions |
| [`OPEN-QUESTIONS.md`](OPEN-QUESTIONS.md) | Decisions still to make, and known defects in the app |

Each test in `PACE-STS` links up to the requirements it checks with a `verifies` link. Both documents use the same section headings, so a test sits in the same section as its requirement.

Pacemark is one app with no hardware or backend, so a system-level document above `PACE-SRS` would only restate the same text. The `PACE` prefix works with both the Pacemark and pacetracker names.

`PACE-SRS` contains only headings and requirements, because Mellon's coverage report counts every other kind of object as needing a test. Background goes in a requirement's `rationale` or in `OVERVIEW.md`. `PACE-STS` may also contain `Information` and `Note` objects, for shared items such as a field-test setup.

## Writing requirements

Requirements follow [ISO/IEC/IEEE 29148:2018](https://www.iso.org/standard/72089.html), clauses 5.2.4 to 5.2.8.

**Form:** `[Condition,] the app shall <action> <object> [constraint].` For example: *When the user selects Pause, the app shall change the session to the paused state.*

**Each requirement is:**

| Characteristic | Meaning here |
|---|---|
| Necessary | Removing it leaves a gap that no other requirement fills. |
| Appropriate | It is at product level. The code could be rewritten without changing it. |
| Unambiguous | It has one possible reading. |
| Complete | It is understandable without other information, apart from the terms in `OVERVIEW.md`. |
| Singular | It is one capability. If one part could pass while another fails, split it. |
| Feasible | It can be built with acceptable risk. |
| Verifiable | A test can tell pass from fail. Quantities are stated. |
| Correct | It accurately states what is needed. |
| Conforming | It follows this form and these rules. |

**Language:**

- The subject is always "the app", and "shall" appears exactly once. Don't use "must", "should", "may" or "will" in a requirement.
- State what, not how: no libraries, classes or internal states.
- Use positive statements in the active voice. Avoid "shall not", "shall be able to" and "it is required that".
- Avoid vague pronouns (it, this, they), superlatives (best, most), subjective words (easy, intuitive), vague qualifiers (fast, significant, appropriate), open-ended phrases (including, such as, etc.), comparatives (better than) and loopholes (if possible, as needed).
- Avoid totals such as all, always, never and every, which are hard to verify. Name the exact set instead.
- Reference external specifications with their version, and only the part that applies.
- Keep to 35 words. Longer usually means more than one requirement.
- Use the terms in `OVERVIEW.md`, with one term per concept.

Several named cases of one thing are still singular: *shall show the GPS signal quality as searching, weak, good or strong*. So is a defined group of values shown together on one screen, such as the workout figures. "and" and "or" joining two separate actions are not.

**Detail:** keep a requirement only if changing the behaviour would be a product decision: a user would notice, or data, privacy or a recorded value is at stake. Exact wording, layout, tuning constants and individual error branches don't belong here.

**IDs:** never reuse an ID. Mellon numbers a new object one above the highest ID in use, so deleting the newest requirement frees its number. To drop a committed requirement, set it to `Deferred` and keep it.

| Attribute | Values |
|---|---|
| `req-type` | `Functional`, `Performance`, `Interface`, `Usability`, `Quality`, `Constraint` |
| `status` | `Proposed`: not decided yet<br>`Accepted`: we intend to build it<br>`Implemented`: the app does it, reachable in a release build<br>`Deferred`: not planned for now |
| `priority` | `High`: recording correctly, not losing data, the privacy promise<br>`Medium`: normal features<br>`Low`: conveniences |
| `rationale` | Required: why the requirement exists, any assumption, and its basis (`as implemented`, `design handoff`, `both`, `inferred` or `owner decision`) |

A value nobody has decided yet is written `TBD`, in a `Proposed` requirement, with a question in `OPEN-QUESTIONS.md`.

## Writing tests

- Put the setup, steps and a measurable expected result in the description. For an automated test, a sentence saying what it proves is enough.
- A test may verify several requirements when one procedure truly checks them all.
- Set `status` to `Active` when every requirement it verifies is `Implemented`, and otherwise `Draft`. Use `Retired` for a test no longer in use.
- Don't record results here. This document says what to check, not how a given run went.
- A test whose requirements are all `Deferred` is `Retired`.

| Attribute | Values |
|---|---|
| `verification-method` | The four methods in clause 6.5.2.2:<br>`Test`: run it and measure against an expected result<br>`Demonstration`: show qualitatively that it works<br>`Inspection`: examine an artifact such as the manifest, source or an exported file<br>`Analysis`: reason from data or a model where a test isn't practical |
| `execution` | `Automated`: a JUnit test proves the expected result<br>`Manual`: everything else |
| `test-ref` | The JUnit class or method, required when `Automated` |

## Making changes

1. From the app repository, run `git submodule update --init`. Mellon needs a full clone, so don't use `--depth`.
2. Open `reqs/` in Mellon, not the app repository root. Mellon only accepts the root of a git work tree.
3. Edit and commit in `reqs/`, then push.
4. In the app repository, commit the updated `reqs` pointer.

**After changing a requirement,** its links from `PACE-STS` turn SUSPECT. Review each test, update it if needed, acknowledge the link and commit `PACE-STS`. This always takes a second commit.

**After adding a linked test:** a link to an uncommitted requirement reads BROKEN. Commit `PACE-SRS` first, then `PACE-STS`, then acknowledge the new links and commit `PACE-STS` again.

**Finding gaps:** the saved filter **Unverified** in `PACE-SRS` lists implemented requirements with no test. Use it rather than the coverage percentage, which also counts `Proposed` and `Deferred` requirements and won't reach 100% while any exist without tests. The filter only runs in the Mellon GUI or library. Over MCP, use `coverage_report` and check each uncovered requirement's status.

## Tracing to the code

A comment in the app's sources names each requirement it implements, directly above the code that does the work:

```kotlin
// PACE-SRS-46: fixes worse than 20 m add no distance.
```

Several requirements can share a comment: `// PACE-SRS-46, PACE-SRS-47: ...`. XML files use `<!-- PACE-SRS-152: ... -->`. Each test class opens with a comment naming the requirements it verifies, and the `PACE-STS` tests it implements where a test object names it.

To follow a requirement into the code, search the app repository for its ID, or list everything with `python tools/check_req_trace.py --list`. The same script runs in CI. It fails if an `Implemented` requirement has no reference, or if a comment names a missing, `Proposed` or `Deferred` requirement.

Add the comment when you build a requirement, and move or remove it when the code changes.

## Releases

Cut an issue tag (`PACE-SRS-issue-<n>`, `PACE-STS-issue-<n>`) only for an app release, not for ordinary commits.

## Working through the Mellon MCP server

- **Register with an explicit root.** Without `--repo-root`, the server uses the project directory, and it refuses to start at the app repository root:
  ```
  claude mcp add mellon -- "$env:LOCALAPPDATA\Programs\Mellon\mellon-mcp.exe" --repo-root <path-to>\reqs
  ```
- **Validate before committing.** MCP commits don't run validation, and rules are only reported by `validate_document` and `validate_repo`. `new_object` will create an object that breaks a rule.
- **New documents lose their labels.** `create_document` ignores attribute `label` values, so add them to the manifest by hand.
- **Filters and rules are files.** MCP can read saved filters but not create them, and it has no tool for rules files. Create filters in the GUI or write the files by hand, as with `PACE-SRS/filters/unverified.xml` and the `<doc-id>.rules.xml` files.
