# Ermentrout2009
An implementation of reaction-diffusion whisker barrel patterns after:

Subbarrel Patterns in Somatosensory Cortical Barrels Can Emerge from
Local Dynamic Instabilities, Ermentrout et al 2009 (PLOS Comp Biol).

## Build instructions

First, git clone, compile and install libmorphologica:

https://github.com/ABRG-Models/morphologica

This model is launched by a python script called sim.py. sim.py will
launch (using code in processes.py) 1 or more instances of a compiled
C++ program called processes. Before this will work, you need to
compile processes:

```
cd processes/sim
mkdir build
cd build
cmake ..
make
cd ../../../
```

Now you can run the model:

```
python sim.py
```

This should launch two separate simulations showing the solution to a
reaction-diffusion system as given in the paper (does this correspond
to any particular figure in the paper?).
