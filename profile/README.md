# DAO — Durham Adaptive Optics

**DAO is a real-time control (RTC) framework for adaptive optics and control.** It runs any loop
that reads sensors, computes, and drives actuators with low latency: an adaptive optics system
reading a wavefront sensor and shaping deformable mirrors, but also instrument control, active
optics, metrology or any other system where data must flow from sensor to actuator on time,
from a lab bench at a few hundred hertz to multi-node RTCs at several kilohertz.

The idea is simple: **a real-time system is a set of small processes, one per stage, talking
through shared memory.** In adaptive optics, a camera driver publishes frames; a calibration
process turns them into clean pixels; another computes slopes; another reconstructs; another
integrates and commands the mirror. Each process does one thing, can be written in C, C++, Python, Rust, Julia or MATLAB, and can be
watched, replaced or restarted on its own while the rest of the loop keeps running.

## Open source

| repository | what it is | docs |
|---|---|---|
| **[daoBase](https://github.com/Durham-Adaptive-Optics/daoBase)** | the core: low-latency POSIX shared memory (`dao.shm`) between real-time processes, ZMQ-based commands, events and logging, and a C++ component and state-machine framework for long-running RTC processes. C core, bindings for Python, Rust, Julia and MATLAB | [daobase.readthedocs.io](https://daobase.readthedocs.io/en/latest/) |
| **[daoTools](https://github.com/Durham-Adaptive-Optics/daoTools)** | the application layer: a C library for the per-pixel and per-mode math, 70+ real-time command-line processes (pixel calibration, centroiding, reconstruction, integrators, mirror commands, telemetry) and PyQt5 GUIs for live monitoring | [daotools.readthedocs.io](https://daotools.readthedocs.io/en/latest/) |
| **[daolite](https://github.com/davetbarr/daolite)** | Durham Adaptive Optics Latency Inspection and Timing Estimator: models the latency of an RTC (camera readout, calibration, centroiding, reconstruction, control) to size a system before it is built | [daolite.readthedocs.io](https://daolite.readthedocs.io) |

With these, you can build a complete single-conjugate AO loop on a Linux machine: the same tools
that run our benches.

## More under the hood

What is public is the foundation. **DAO is much larger than these repositories.** Built on the
same shared-memory core and the same process model, and used every day on real systems, are:

- **Hardware interfaces**: drivers for scientific cameras and frame grabbers, deformable
  mirrors, tip-tilt stages, spatial light modulators and power units, each a small process that
  publishes to shared memory, so pipelines and GUIs never talk to the hardware directly.
- **Telescope interfaces**: telemetry, guiding, pointing offsets and offloading between the AO
  loop and the telescope control system.
- **Instrument RTCs**: complete real-time computers for observatory instruments and laboratory
  benches: single-conjugate, laser-tomography and multi-loop systems, pyramid and Shack-Hartmann
  sensors, CPU and GPU pipelines.
- **High-performance components**: matrix-vector multiplication spread across NUMA nodes,
  distributed pipelines over the network, and tools to measure and monitor RTC latency end to end.
- **Simulation and twins**: camera simulators and numerical twins that sit behind a real RTC, so
  a pipeline is developed and tested before the hardware arrives, with the same code.
- **Operations tooling**: start-up and supervision scripts, calibration procedures, telemetry
  recording and control GUIs, shaped by years of running AO systems on the sky.

These parts are developed with, and for, the observatories and partners who use them. If you are
building an AO system and want to know whether DAO fits, **get in touch**.

## Who we are

DAO was created by [Sylvain Cetre](https://github.com/scetre) and 
[David Barr](https://github.com/davetbarr), and is developed at the [Centre for Advanced
Instrumentation](https://www.durham.ac.uk/research/institutes-and-centres/advanced-instrumentation/)
(CfAI), Durham University, at [Wakea Consulting](https://wakeaconsulting.com) and at
[Dave T Barr Consulting](https://davetbarr.com), with contributors from the AO community.
Between them, the team has decades of experience building and operating RTCs for some of the
world's largest telescopes.

## Paid support

DAO is open source and free to use. When you need more than the community can offer, paid
support is available from [Wakea Consulting](https://wakeaconsulting.com),
[Dave T Barr Consulting](https://davetbarr.com) and, as required, Durham University: help to
install and configure DAO, integration of your hardware, development of new components or of a
complete RTC, on-site commissioning, training and long-term maintenance.

## Citing DAO

If DAO helps your research, please cite daoBase:
[10.5281/zenodo.17264152](https://doi.org/10.5281/zenodo.17264152)

```bibtex
@software{barr_2025_17264152,
  author    = {Barr, David and Cetre, Sylvain and Connolly, John and Thomas Davies},
  title     = {Durham-Adaptive-Optics/daoBase: Initial Release},
  year      = 2025,
  publisher = {Zenodo},
  version   = {v0.0.1},
  doi       = {10.5281/zenodo.17264152},
  url       = {https://doi.org/10.5281/zenodo.17264152},
}
```
