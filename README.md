# Universe Sandbox

(no relation to [Universe Sandbox](https://universesandbox.com))


## Prerequisites

- Python
- conda
- git (obviously)
- Make sure your full path has no spaces 🙄

## Installation

Just follow the conda instructions in the [CosmoSIS installation guide](https://cosmosis.readthedocs.io/en/latest/intro/installation.html).

## Running code

All code must be run from the `cosmosis-standard-library` directory. They can not be run from anywhere else (AFAIK) because internally it uses relative paths to locate everything.

You can try running the files in `demos` or in `examples`.

### Downloading planck data

Some of the examples need Planck data. To download it, run
```bash
examples/get_planck_data.sh
```

The main combination of `classy` + `planck` + `polychord` is right here with the `planck_custom.ini` file. You can run it with

```bash
cosmosis ../planck_custom.ini
```

**NOTE**: Once this program starts running, it can't be quit using Ctrl+C. You have to kill the entire terminal to stop it. This is due to using polychord as the sampler.