# Working conventions

## Push to main

Commit and push straight to `main`. No feature branches or PRs.

To release: bump the version, move the changelog's Unreleased section under
it, tag, and let the release workflow publish. No approval step.

## The TV runs what main has

Install on the TV only from a commit that is on `main` and pushed to both
remotes (`origin` on GitHub, `forge` on git.nateshome.net). Name the
installed binary hashes in the commit or the handoff. Scratch worktrees are
for experiments; merge or remove them before the session ends, and never
leave the only copy of installed code under `/tmp`.

Build output (`target/`), packages and review screenshots stay out of git.

## Quality bar

- Before pushing: the touched crate's tests and `cargo clippy` pass.
- No WIP, placeholders, ignored tests or leftover debug logging on `main`.
- Frame loop, input, video and power changes need a measured result (frame
  gate, video check) before claiming they are faster or fixed.
- "Accepted" means checked on the actual TV, with the receipt path recorded.
  Untested cases are listed as untested, not implied.
- Tests cover the changed behavior and nothing more. A unit test runs well
  under a second: no sleeps or long loops where a fake clock or a smaller input
  proves the same. Delete tests that duplicate another test's coverage.
- Every surface, card and plugin takes after the active theme: colours,
  glass, radii and type come only from the theme's tokens and change live with
  it; plugins supply data, never styling.
- Integrations and extras (Scrypted, Home Assistant, Ring, Plex, Live TV
  tuner sharing and anything new) are plugins: off until enabled in Settings,
  each with its own settings and connection page. The core stays minimal.
  Privileged work stays in the trusted helper and runs only while its plugin
  is enabled. Reusable engines carry no LumaTV dependencies, so they can ship
  as standalone homebrew apps.

The open review of recent work, its method and findings: `docs/REVIEW.md` in
the lumaglass repo.

# Writing conventions

Applies to commit messages, PR descriptions, release notes, README and docs.

## Keep it short

State what changed and why. Stop. A PR description is usually a paragraph or
two, not a report.

Length should track the size of the change, not the effort behind it. A
one-line fix gets one line.

## Don't narrate the work

No process commentary: what was tried, what was ruled out, what turned out to
be a false alarm, how something was verified. If a check found nothing, that
is not a finding worth writing down.

Record decisions and their reasons, not the route taken to reach them.

## Third person, always

These are project artefacts, not messages to a reader. Never "you", "your TV",
"as you asked". Write "the TV", "the panel switch".

## No filler structure

Don't add headings, tables or bullet lists to a short change. Tables are for
genuine matrices - a compatibility list, a unit reference - not for restating
three sentences.

Don't list things that did not change.

## Comments in code

Explain why, where a reader would otherwise wonder or get it wrong: a platform
quirk, a non-obvious ordering constraint, a value that looks wrong but isn't.
Skip comments that restate the code.

Keep the measured facts that justify a decision (an observed value, a version,
a threshold). Drop the story around them.
