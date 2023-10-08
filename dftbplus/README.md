# Installation
The general installation on Arch is handled by `makepkg`:

```shell
makepkg -si
```

## Slater-Koster files
To really use DFTB+, you also need the Slater-Koster files. A full archive of
publicly available SK-files can be found
[here](https://dftb.org/fileadmin/DFTB/public/slako-unpacked.tar.xz). Retrieve
and unpack them into the `/opt/dftb+` installation directory with the following
commands:

```shell
cd /tmp
curl -O 'https://dftb.org/fileadmin/DFTB/public/slako-unpacked.tar.xz'
xz -d slako-unpacked.tar.xz
tar xf slako-unpacked.tar
sudo cp -r slako /opt/dftb+/.
sudo chmod +r /opt/dftb+/slako -R
sudo find /opt/dftb+/slako -type d -exec chmod +x {} \;
```

These last two commands should allow all users to read files in this directory
and `cd` into the directories, respectively.

# Testing your installation

Save the input file below as `dftb_in.hsd`, which is adapted from
`recipes/basics/firstcalc/dftb_in.hsd` retrieved from [here][recipes].

```text
Geometry = GenFormat {
3 C
  O H

  1 1  0.00000000000E+00 -0.10000000000E+01  0.00000000000E+00
  2 2  0.00000000000E+00  0.00000000000E+00  0.78306400000E+00
  3 2  0.00000000000E+00  0.00000000000E+00 -0.78306400000E+00
}

Driver = GeometryOptimization {
  Optimizer = Rational {}
  MovedAtoms = 1:-1
  MaxSteps = 100
  OutputPrefix = "geom.out"
  Convergence {GradAMax = 1E-8}
}

Hamiltonian = DFTB {
  Scc = Yes
  SlaterKosterFiles {
    O-O = "/opt/dftb+/slako/mio/mio-1-1/O-O.skf"
    O-H = "/opt/dftb+/slako/mio/mio-1-1/O-H.skf"
    H-O = "/opt/dftb+/slako/mio/mio-1-1/H-O.skf"
    H-H = "/opt/dftb+/slako/mio/mio-1-1/H-H.skf"
  }
  MaxAngularMomentum {
    O = "p"
    H = "s"
  }
}

Options {}

Analysis {
  CalculateForces = Yes
}

ParserOptions {
  ParserVersion = 12
}
```

Run `dftb+` as `/opt/dftb+/dftb+ dftb_in.hsd` and confirm that it exits
successfully with `echo $?` printing `0`. The total energy printed to stdout on
my machine is `-4.0779379E+00 H`.

[recipes]: https://dftbplus-recipes.readthedocs.io/en/latest/_downloads/5919f4094cd60c5c70c13b47928442f5/recipes.tar.bz2
