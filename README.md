# Universe Sandbox

(no relation to [Universe Sandbox](https://universesandbox.com))


## Prerequisites

- Python 3.11 or lower (Python 3.12 won't work due to wap using deprecated methods)
- conda-forge
- an f77 executable (Fortan 77 compiler)
- git (obviously)
- Make sure your full path has no spaces 🙄

#### conda-forge

All you have to do is download the script and execute it.

```bash
wget -O Miniforge3.sh  https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3.sh
```

I'm unsure how/if it conflicts with an existing conda installation. After installing, you will need to add the executables to your path. Just do that with
```bash
conda init
```

#### f77

Making all the packages successfully requires you to have a f77 binary in your path. [This](https://stackoverflow.com/questions/1175897/how-to-compile-fortran-77-files-in-linux) answer suggests using gfortran instead. It should be available in your package manager.

You can make gfortran the f77 binary via an alias such as `alias f77=gfortran`. You can do this via
```bash
echo "alias f77=gfortran" >> ~/.bashrc
```

<!-- I'll stick with a custom install.

```bash
wget "ftp://ftp.icm.edu.pl/pub/Linux/sunsite/devel/lang/fortran/fort77-1.18.tar.gz"
tar -xvf fort77-1.18.tar.gz
rm fort77-1.18.tar.gz
cd fort77-1.18
```
Now that we have the binary, we need to rename it to `f77` and add the path to the `PATH` variable.
```bash
mv fort77 f77
echo "export PATH=$PATH:$PWD" >> ~/.bashrc
source ~/.bashrc
``` -->


## Installation

Note: This part takes major inspiration from the [official CosmoSIS installation guide](https://cosmosis.readthedocs.io/en/latest/intro/installation.html).

First we make a new environment and initialize it. Starting from the project root, run
```bash
conda create -p ./env -c conda-forge mamba python=3.11
conda activate ./env
```
Verify that it worked by running `which python`.

Then we get mamba (whatever that is) and install cosmosis.

```bash
mamba install -y cosmosis cosmosis-build-standard-library
```

Now, the magic command that makes all the upcoming builds work is
```bash
source cosmosis-configure
```
This sets up the environment variables correctly, and will be needed if you want to build any of the hundreds of the modules included in cosmosis. But it's not needed for just running and using the module.

Now we jump off the rails and run some custom commands because `cosmosis-build-standard-library main` is too rigid (it's 2024 and `git` still doesn't let you not care if the directory already exists).

```bash
git clone https://github.com/joezuntz/cosmosis-standard-library.git
```

### Trimming the excess

NOTE: Skip this section for now.

Now, open the Makefile in the cosmosis-standard-library directory and change the line
```bash
SUBDIRS = boltzmann shear supernovae structure mass_function likelihood
```
to
```bash
SUBDIRS = boltzmann shear supernovae structure likelihood
```
If you want access to the mass functions, you'll have to port the F77 code to F90 or higher. The errors you get can be fixed by following [this answer](https://stackoverflow.com/questions/5071455/scientific-fortran-compile-error).

Since you're interested in the planck likelihoods, you'll now have to do the same thing for the Makefile located in cosmosis-standard-library/likelihood/. Change then line
```bash
SUBDIRS = fgas  wmap_shift wmap9 planck2018
```
to
```bash
SUBDIRS = fgas  wmap_shift planck2018
```

Now, before making it all we need to actually get the planck data, which is a pain to get. From the root directory:

```bash
cd cosmosis-standard-library/likelihood/planck2018/plc-3.0
./waf configure
./waf install
```
This should install and compile all the necessary files. (except for the `io.h` error, it all works.)

Finally, from the same directory, we need to download the data.

```bash
cd ../ # So you should now be in cosmosis-standard-library/likelihood/planck2018/
make data
```

### Final command

Now, from the root directory, all you have to do is:

```bash
cd ./cosmosis-standard-library
make
```

That should build most of the modules. For now, you can ignore all the other errors as long as classy got pip installed.

## Running code

All code must be run from the `cosmosis-standard-library` directory. They can not be run from anywhere else because internally it uses relative paths to locate everything.

You can try running the files in `demos` or in `examples`.

The main combination of `classy` + `planck` + `polychord` is right here with the `planck_custom.ini` file. You can run it with

```bash
cosmosis ../planck_custom.ini
```

**NOTE**: Once the program starts running, it can't be quit using Ctrl+C. You have to kill the entire terminal to stop it.