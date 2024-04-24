# Guide for evaluating metrics for a Molecular dynamics (MD) simulation with AMBER Software on ASU Sol cluster

## Follow the MDGuide to run the MD simulation first

Follow the MDGuide guide here: [MDGuide](https://github.com/John-Kazan/MDGuide)

## Using VPN to connect to ASU network:

Follow the VPN guide here: [VPNGuide](https://github.com/John-Kazan/VPNGuide)

## After logging in to SoL cluster:

`pwd` shows me home directory `/home/ikazan`

change directory to scratch space

```
cd /scratch/ikazan
```

create a new directory here by using

```
mkdir -pv evaluationdir1
```

change directory to the new one

```
cd evaluationdir1/
```

```
ls
```

the directory is empty

`pwd` shows me the current working directory

copy the sol path: `/scratch/ikazan/evaluationdir1`

## Two options to download required files (I recommend option 2)

### Option 1

open a new ternminal tab (this will be connected to your own computer)

download the parameter file under `mdtrajectory` on github

and download the trajectory from the dropbox link:

[1btl.nc](https://www.dropbox.com/scl/fi/rcn2cecemv2erab298t4k/1btl.nc?rlkey=eqppb7rtuvxvmzghdp0ks4zsd&dl=0)

go to the directory where you have the files and copy the them to sol

```
scp ./*.parm7 ikazan@login.sol.rc.asu.edu:/scratch/ikazan/evaluationdir1/
```

```
scp ./*.nc ikazan@login.sol.rc.asu.edu:/scratch/ikazan/evaluationdir1/
```

we are going to switch the termnial window to the sol session one

### Option 2

copy the parameter and trajectory file directly on sol

```
cp -rv /scratch/ikazan/shared/test ./
```


## Start an interactive session

we are going to start an interactive session by running

```
interactive
```

## Reading the trajectory and evaluation

load the necessary modules on sol by running:

```
module load amber/22v3
```

then we are going to start the evaluation process by running:

```
cpptraj
```

this will start the `cpptraj` software

first we need to load the parameter (topology) file

```
parm 1btl.parm7
```

then we are going to load the trajectory

```
trajin 1btl.nc
```

After succesfully loading the parameter and trajectory files we need to do some cleaning first by removing water molecules and the ions

```
strip :WAT,Cl-,Na+
```

then superimpose (align) the trajectory

```
autoimage origin
```

```
rms first mass @CA,C,N
```

Then we will evaluate the Root Mean Square Fluctuation (RMSF)

RMSF is a measurement of flexibility. It measures how much amino acid residues of a protein move around over time during a molecular dynamics simulation. We will use the alpha carbon (CA) locations of amino acids to calculate the RMSF.

```
atomicfluct out rmsf.txt @CA
```

at this point all the commands should be entered. To run the commands simply type:

```
run
```

After processing completes type `quit` to quit the application:

copy `rmsf.txt` back to your computer by running the following command on the terminal connected to your local computer

```
scp ikazan@login.sol.rc.asu.edu:/scratch/ikazan/evaluationdir1/rmsf.txt ./
```

Compare the `rmsf.txt` file you generated to the one provide under `outputs` folder on github

Now use your favorite tool to generate a plot

