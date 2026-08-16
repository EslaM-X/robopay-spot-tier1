# Contributing

Thanks for your interest. This project is designed so a first-time contributor
can land a small, reviewable change quickly.

## Ground rules

- **Evidence stays honest.** Every measurement in this repo (paw lift, sit
  depth, body height) must stay reproducible. Never commit a number you did
  not measure.
- **Offline tests.** New skills and gates ship with tests that run without
  network or API keys.
- **Small PRs.** One logical change per pull request.
- **Registry-driven.** A new skill touches the registry profile
  (`registry/vendors/boston_dynamics/spot/boston_dynamics.spot.mujoco-pybullet-sim.v1/`) —
  `skills.yaml`, `functions.yaml`, `payment-policy.yaml`,
  `execution-mapping.yaml`, `tests/`, and the skill table in `README.md`.

## Getting started

1. Fork and clone.
2. `cd simulation && bash setup.sh` — pinned official menagerie
   `boston_dynamics_spot` assets.
3. `pip install mujoco>=3.1.3 numpy pybullet cryptography eclipse-zenoh eth-account`.
4. `cd spot && python3 test_spot_control.py` — a single skill test must pass.

## First contribution in 6 steps

1. Pick an open issue (labels: `good first issue`, `good first contribution`,
   `help wanted`, `documentation`).
2. Read the [code of conduct](CODE_OF_CONDUCT.md) and this guide.
3. Run the test suite and keep it green (`python3 -m pytest simulation -x`
   or the suite listed in `simulation/README.md`).
4. Run `ruff check` and `ruff format --check` clean if you touch Python.
5. Open your pull request (use the [PR template](.github/PULL_REQUEST_TEMPLATE.md)).
6. Get reviewed — then your name goes on the contributor wall.

## Pull requests

- Add or update a test with every change.
- Keep the `Spot Simulation Tests` CI green.
- Update `CHANGELOG.md` with your change.
- Link the issue your PR closes.

## Labels you can grab

- `good first issue` / `good first contribution` — small, well-scoped.
- `help wanted` — maintainers would like contributions.
- `documentation` — docs-only, great starting point.
- `skill` / `payment-gate` / `sim2sim` — feature-area work.

## Code of conduct

Be respectful and constructive. See `CODE_OF_CONDUCT.md`.
