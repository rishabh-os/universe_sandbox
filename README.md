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

The main combination of `classy` + `planck` + `polychord` is right here with the `planck_custom.ini` file. You can run it with

```bash
cosmosis ../planck_custom.ini
```

**NOTE**: Once this specific program starts running, it can't be quit using Ctrl+C. You have to kill the entire terminal to stop it. Other programs can be, I'm unsure why.