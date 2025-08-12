# Membrane Transport Simulation — OpenMM + VMD

![Status](https://img.shields.io/badge/Project-Public%20Writeup-blue)
![Data](https://img.shields.io/badge/Data-Redacted-red)
![OpenMM](https://img.shields.io/badge/OpenMM-7.7+-orange)
![VMD](https://img.shields.io/badge/VMD-1.9+-purple)
![Python](https://img.shields.io/badge/Python-3.8+-blue)

![VMD Simulation Screenshot](VMDSS.png)

## Overview

This repository documents a **custom-force transport prototype** in OpenMM (Python interface) with VMD for inspection.  
The goal was to test whether particles could traverse a membrane-like barrier under a **downward driving force** while:
- keeping the membrane compact via **harmonic restraints**,
- **confining particles** within the simulation box,
- **blocking non-pore penetration** with a short-range repulsive wall near the membrane surface.

> **Note:** This prototype **does not implement explicit nonbonded interactions** (e.g., Lennard-Jones/Coulomb) or a full biomolecular force field. Temperature control was used to sustain motion and stabilize dynamics, but selectivity was governed by the custom forces and geometry—not by a calibrated force field.

## 🎯 Project Objectives

- **Design and implement** a membrane transport prototype
- **Establish** robust setup/troubleshooting protocols
- **Characterize** particle transport selectivity across membrane pores
- **Document** engineering decisions and troubleshooting approaches

## 🔬 Technical Approach (Accurate Summary)

**System Sketch**
- Membrane: simplified slab built from scratch (no lipid FF); compactness maintained using **harmonic restraints**.
- Particles: simple point masses (no bonded terms); a **spectrum of radii** and masses was explored (values redacted).
- Box/Boundaries: periodic or confining boundaries to keep particles in a defined region.

**Forces Used**
- **Downward drive** (pressure surrogate): a custom force pushing particles toward/through pores.
- **Membrane restraint**: harmonic restraint field to keep the slab compact and positioned.
- **Repulsive barrier near membrane**: custom short-range potential preventing transit **except through pores**.
- **No explicit LJ/Coulomb**: nonbonded interactions were not included in this prototype.

**Dynamics & Control**
- Integrator + **thermostat** (values redacted) to maintain a target temperature.
- Protocol: minimize → equilibrate → drive; frame cadence redacted.

**Diagnostics**
- Monitored **temperature**, **potential energy**, and qualitative trajectories in VMD.
- Checked mass–acceleration proportionality and boundary behavior.

## 🛠️ Implementation Details

### Force Field Design
The simulation employs a custom force field:

1. **Custom Forces**
   - Membrane restraint potentials
   - Transport driving forces
   - Surface repulsion barriers

2. **Boundary Conditions**
   - Periodic boundary implementation
   - Particle confinement strategies

### Simulation Protocol
```
1. System Preparation
   ├── Topology generation
   ├── Parameter assignment
   └── Initial structure optimization

2. Energy Minimization
   ├── Steepest descent
   ├── Conjugate gradient
   └── Convergence validation

3. Equilibration
   ├── Temperature ramping
   ├── Pressure coupling
   └── Structural relaxation

4. Production Dynamics
   ├── Trajectory generation
   ├── Data collection
   └── Analysis preparation
```

## 📊 Qualitative Findings

- Particles traversed the membrane **when pores were present** and drive was applied.
- Without pores, the **repulsive wall** blocked penetration as intended.
- Size selectivity was **limited** in this prototype because no explicit nonbonded potentials were used; behavior was dominated by geometry and custom forces.
- Membrane remained compact under harmonic restraint.

> Outcome: Did not reach the quantitative selectivity targets; prepared **next-step plan** for a successor to complete a physics-faithful model.

## 🔄 Next Steps (to make it physically meaningful)

1) **Introduce physical nonbonded interactions**
   - Add **WCA/Lennard-Jones** for sterics and optional **Coulomb** if charges matter.
   - Calibrate ε/σ (or WCA) to create radius-dependent exclusion realistically.

2) **Define the membrane potential surface**
   - Use an explicit atomistic slab or a **continuous potential field** for the wall/pore.
   - Parameterize pore radius and wall stiffness; sweep these systematically.

3) **Model the driving mechanism more faithfully**
   - Replace the global downward force with: concentration gradient, body-force density, or a **pressure differential** (barostat-compatible setup if applicable).

4) **Automate analysis**
   - Add a crossing detector (plane or cylindrical surface test) and log per-particle events.
   - Produce rate vs. radius curves; compute uncertainties via block averaging.

5) **Validation & stability**
   - Temperature/PE drift checks; timestep stability scans.
   - Compare toy results to known limits (e.g., hard-sphere sieving).

## 🚀 Technical Challenges & Solutions

### Challenge 1: Force Field Balance
**Problem**: Balancing membrane stability with transport dynamics
**Solution**: Iterative refinement of restraint strengths and force scaling

### Challenge 2: Numerical Stability
**Problem**: Maintaining energy conservation and temperature control
**Solution**: Careful integrator selection and parameter tuning

### Challenge 3: Selective Transport Implementation
**Problem**: Particles showed uniform transport behavior regardless of size, lacking expected selectivity
**Solution**: Identified need for stronger size-dependent barriers and refined pore geometry design

### Challenge 4: System Size Optimization
**Problem**: Balancing computational cost with statistical significance
**Solution**: Systematic convergence studies and efficient sampling strategies

## 🛠️ Technology Stack

- **OpenMM (Python interface)** — custom forces, integration, thermostating
- **VMD** — trajectory inspection
- Python scientific stack (NumPy, Matplotlib) for local plotting (plots not included)

## 📚 References & Resources

### OpenMM Documentation
- [OpenMM User Guide](http://docs.openmm.org/)
- [Python API Reference](http://docs.openmm.org/latest/api-python/)
- [Force Field Documentation](http://docs.openmm.org/latest/userguide/application.html#force-fields)

### VMD Resources
- [VMD User Guide](https://www.ks.uiuc.edu/Research/vmd/current/ug/)
- [Analysis Tools](https://www.ks.uiuc.edu/Research/vmd/current/ug/node127.html)

## 🤝 Contributing

Portfolio demonstration. Questions? Open an issue.

## Limitations

- No explicit LJ/Coulomb or bonded force field terms; selectivity limited by geometry + custom forces.
- No quantitative transport rates reported here; no trajectories or plots published.
- Prototype intended as a **troubleshooting and methods-finding step** toward a physics-faithful model.

---

**Note**: This repository serves as a technical portfolio piece demonstrating advanced molecular simulation capabilities. All proprietary information, parameter values, and quantitative results have been intentionally omitted to maintain confidentiality while preserving the technical insights and engineering approach.