# 🦾 RoboPay Spot Tier-1 — Boston Dynamics Spot, paid embodied skills in simulation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE) [![CI](https://github.com/EslaM-X/robopay-spot-tier1/actions/workflows/spot-simulation-tests.yml/badge.svg)](https://github.com/EslaM-X/robopay-spot-tier1/actions)

> **Profile:** `boston_dynamics.spot.mujoco-pybullet-sim.v1`
> **Scope:** simulator-only · 8 priced skills · MuJoCo + PyBullet · x402 payment gate

A paid RoboPay action arriving on the Zenoh topic `robot/tunnel/action` drives
the **official** MuJoCo Spot model (`google-deepmind/mujoco_menagerie`
`boston_dynamics_spot`) through real, policy-triggered skill episodes —
joint-space trajectory control with body-weight compensation. **Never** a
replayed animation, **never** a built-in demo motion.

This repository is the **original authorship archive** of the Spot Tier-1
submission to the RoboPay bounty. It lives independently of any fork so the
work is provably authored by EslaM-X, timestamped by git history, and tagged
for release.

## The 8 skills

| skill | what happens | measured |
|---|---|---|
| `wave` | front-right paw lifts in an arc, body-weight compensated | pawLift 0.212 m, body stable at 0.432 m |
| `sit` | crouch into a sit posture, then return | sitDepth 0.133 m |
| `stand` | return to the home standing stance | standHeight 0.435 m |
| `stop` | safe stop: halt motion, return to the stable home stance | halted at 0.434 m |
| `bow` | play bow (front dips, hind stays up) | bowPitchDeg 16.9 deg |
| `nod` | full-body greeting bob | nodDepth 0.055 m |
| `turn_to_face` | yaw toward `headingDeg`; reports achieved yaw + residual honestly | 10.7 deg toward heading 30 |
| `hold` | hold the stance for `seconds` | stable at 0.434 m |

Every successful skill returns to the home stance height afterwards
(`|bodyZ − 0.434| < 0.02`), so paid actions run back to back.

## End-to-end flow

```
paid action (x402) → tunnel → Zenoh robot/tunnel/action
    → robopay_link.py → validate envelope + payment gate
    → joint-space Spot controller on mujoco_menagerie
    → metrics → result on robot/tunnel/result (correlated by actionId)
```

## Why this clears the RoboPay success criteria

- **Real action, not a demo** — each skill is a joint-space trajectory driven
  by the controller; physics metrics are measured (paw lift, sit depth, torso
  pitch/yaw, body height, achieved heading), not scripted.
- **Payment safety** — settle **only** on `status: success`; unpaid ⇒ 402 +
  `PAYMENT-REQUIRED`, forged/expired receipts ⇒ 402, replay ⇒ 409, tampered
  `paramsHash` ⇒ `INVALID_PARAMS`. Every failure path returns an error result
  and never settles.
- **Sim-to-sim** — the same joint configurations are recomputed in PyBullet
  (from the rai-opensource `spot_simple` URDF); foot-tip positions agree to
  **0.06 cm** across all salient poses (`simulation/pybullet/sim2sim_report.json`).
- **Reproducible** — clean checkout + 4 commands, under 30 minutes.

## Reproduce

```sh
pip install mujoco>=3.1.3 numpy pybullet eclipse-zenoh
cd simulation
./setup.sh                      # pinned official Spot model assets
cd spot
python3 test_spot_control.py    # every skill's physics actually happen
python3 test_payment_gate.py    # 402/409, no-settle-on-failure
python3 test_result_semantics.py# success/error semantics, replay, tampering
python3 test_link.py            # paid action → Zenoh → episode → result
cd ../pybullet
python3 test_sim2sim.py         # MuJoCo ⇄ PyBullet kinematics agreement
```

Each test prints its checks as JSON with PASS/FAIL and exits nonzero on
failure. CI (`spot-simulation-tests`) runs the full suite on `ubuntu-latest`.

## Layout

```
registry/vendors/boston_dynamics/spot/boston_dynamics.spot.mujoco-pybullet-sim.v1/
    robot.profile.yaml      robot identity + Zenoh runtime
    skills.yaml             the 8 skills, params, limits
    functions.yaml          agent REST contract (/action, 402)
    payment-policy.yaml     x402 pricing + settle-on-success rule
    execution-mapping.yaml  skill → simulator runtime + metrics
    examples/               paid action envelope
    tests/                  skill-contract cases
    docs/                   README + validation report
simulation/
    setup.sh                pinned fetch of official model assets
    spot/                   controller, payment gate, Zenoh link, tests
    pybullet/               sim-to-sim URDF + comparison report
docs/images/flow.png        architecture diagram
```

## Evidence media

- Screen recording of the paid skills: `simulation/docs/spot.gif`
- Sim-to-sim comparison: `simulation/pybullet/sim2sim_report.json`
- Architecture diagram: `docs/images/flow.png`

## Contributing

Open source, licensed MIT. New skills, payment-gate hardening, and sim-to-sim
improvements are welcome. Start with
[CONTRIBUTING.md](CONTRIBUTING.md) and the
[`good first issue`](https://github.com/EslaM-X/robopay-spot-tier1/labels/good%20first%20issue)
label. Please read the [code of conduct](CODE_OF_CONDUCT.md) and report
vulnerabilities privately per [SECURITY.md](SECURITY.md).

## License

MIT — © 2026 EslaM-X 🇪🇬
