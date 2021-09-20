# Feature Model Repository

This repository provides feature models for several open-source projects based on the Kconfig language for variability modeling.
The feature models are created with [kconfigreader](https://github.com/ckaestne/kconfigreader), a tool for reading Kconfig files and converting them into feature model formulas for further reasoning.
For improved reproducibility, kconfigreader is set up and run in a virtual machine running Ubuntu 14.04 (for newer versions there are incompabilities between Java and the required Scala version).

## Getting Started

Clone recursively:

```
git clone --recurse-submodules git@github.com:ekuiter/feature-model-repository.git
```

Install [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/), then run `vagrant up` inside this repository. This will also prompt you to install the base system for the VM.
After `vagrant up`, use `vagrant ssh` to log on to the VM.
In the case that `vagrant up` fails with `error retrieving required libraries` for Scala, this can be fixed by re-running the setup script with `vagrant ssh` and then `source /vagrant/setup.sh`.

With `chmod +x /vagrant/eval.sh && /vagrant/eval.sh`, you can read feature models for several versions of Linux and other Kconfig-based projects.
The results are stored into the `models/` directory, resulting in (description taken from https://github.com/PettTo/Feature-Model-History-of-Linux):

```
*.rsf       Intermediate xml file format, created by KConfigReader (dumpconf). Contains raw dump of the original KConfig model file
*.features  Simple text file containing all feature names contained in the original variability model
*.model     Text file that contains boolean constraints, which represent the original KConfig model
*.dimacs    Text file that contains CNF constraints, which represent the original KConfig model. Created by KConfigReader by using Tseitin transformation.
 ```

The resulting models are comparable to those found at https://github.com/PettTo/Feature-Model-History-of-Linux, only that we have a different selection of projects and commits.
We also made some changes to dumpconf (the tool used to produce the input RSF file for kconfigreader), to allow for reading feature models for other projects and versions.
Specifically, we added support for E_CHOICE (treated as E_LIST), P_IMPLY (treated as P_SELECT), E_NONE, E_LTH, E_LEQ, E_GTH, E_GEQ (ignored).
We also fixed some other bugs to allow reading feature models for other projects as well.
All compiled dumpconf binaries are stored in the `dumpconf/` directory for later reuse.