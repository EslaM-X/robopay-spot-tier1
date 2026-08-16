# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project
adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [v1.0.1] - 2026-08-11

### Added

- Contributor funnel: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`,
  issue/PR templates, CODEOWNERS, dependabot, discussion template.

## [v1.0.0] - 2026-08-11

### Added

- Authorship archive of the Boston Dynamics Spot Tier-1 RoboPay submission
  ([PR #86](https://github.com/fabricfoundation/RoboPay/pull/86)).
- Registry profile `boston_dynamics.spot.mujoco-pybullet-sim.v1` — 8 priced
  skills (`wave`, `sit`, `stand`, `stop`, `bow`, `nod`, `turn_to_face`,
  `hold`), x402 payment policy, execution mapping, skill-contract tests.
- `simulation/` — joint-space Spot controller on the official
  `mujoco_menagerie` model, payment gate, Zenoh link, and 5 test suites.
- PyBullet kinematic sim-to-sim agreement report.
- `Spot Simulation Tests` CI workflow; evidence media (`spot.gif`, sim-to-sim
  report).
