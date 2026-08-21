# cav-platooning-delay-dataset

## Communication-Delay Experiments in Cooperative Vehicle Platoons

## Overview

This repository contains simulation-generated, run-level experimental
datasets produced as part of a bachelor thesis at Mälardalen University.

The experiments investigate the effects of communication delays on the
safety of cooperative vehicle platoons under different experimental
conditions.

Each row represents one simulation run and contains the simulation
configuration, attack parameters, aggregated vehicle-dynamics metrics,
and collision outcome.

The datasets are neither time-series datasets nor packet- or beacon-level
datasets. They can be used to calculate collision counts and collision
rates

## Data provenance and contributions

The dataset was generated using ComFASE (https://github.com/das-rise/ComFASE). [1] [2]

ComFASE is built on top of OMNeT++ (https://omnetpp.org/) and integrates SUMO (https://www.eclipse.org/sumo/) and Veins (https://veins.car2x.org/). It also uses platooning scenario from Plexe-Veins (https://plexe.car2x.org/tutorial/).

The source code, experimental configuration structure, and data-generation scripts originate from ComFASE and are not redistributed in this repository. The structure and column names of the resulting CSV files were determined by the ComFASE data-generation scripts. For the experiments conducted in this thesis, the author only selected and changed the investigated parameter values, executed the simulation campaigns, and collected the resulting CSV files. The numerical results contained in these files were generated during those simulation campaigns. This repository contains only the resulting CSV files and does not include the underlying ComFASE source code, Python scripts, or configuration files.

## Simulation environment

The datasets were generated using:

- OMNeT++ 5.6.3
- SUMO 1.18.0
- PLEXE 3.0-alpha2
- Veins 5.2
- ComFASE, Git commit `d450671`

## Repository contents

The `data/` directory contains the CSV files generated during the
simulation campaigns.

Scenario: Braking/Sinusoidal
Controller: CACC/PLOEG
CACC spacing: 5, 8, 10 m
Platoon size: 4, 5, 6, 7, 8 vehicles
Leader speed: 80, 100 km/h
Braking deceleration: 5, 6, 7, 8 m/s²
Braking start time: 17 s
Simulation time: 60, 120 s


first attack starts: 16 s
attack duration: 6 s
attack ends: 22 s
delay: 500 ms - 1100 ms
target: sender + receiver
attack type: Delay

## Software attribution

ComFASE and its underlying software components were not developed by the
dataset creator. Users should cite the relevant publications and software
repositories when referring to the simulation environment.

## Software licensing

No license has currently been assigned to the CSV datasets in this
repository. The licenses of ComFASE, PLEXE, Veins, OMNeT++, and SUMO apply
to their respective software components, which are not redistributed here.

ComFASE is distributed under the GNU General Public License v3.0
(GPL-3.0). The licenses of ComFASE and its underlying simulation
components apply to the respective software components.

This repository does not redistribute the ComFASE source code,
Python scripts, or configuration files. It contains only the resulting
CSV files generated during the simulation campaigns.

## References

[1] M. Malik, M. Maleki, P. Folkesson, B. Sangchoolie, and J. Karlsson,
“ComFASE: A tool for evaluating the effects of V2V communication faults
and attacks on automated vehicles,” in *2022 52nd Annual IEEE/IFIP
International Conference on Dependable Systems and Networks (DSN)*,
2022, pp. 185–192,
doi: [10.1109/DSN53405.2022.00029](https://doi.org/10.1109/DSN53405.2022.00029).

[2] DAS RISE, “ComFASE,” GitHub repository, Git commit `d450671`,
Mar. 12, 2024. [Online]. Available:
[https://github.com/das-rise/ComFASE](https://github.com/das-rise/ComFASE).
[Accessed: Aug. 21, 2026].
