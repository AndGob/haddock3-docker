CNS and haddock software should be requested and downloaded from their sites, then file should be put in the docker repository folder CNS and haddock3 to work properly.

To build the docker image:
```
cd <haddock3 folder from this repository>
docker build -t haddock3 -f .\Docker\Dockerfile .
```

To run a interactive session use:
```
docker run -it haddock3 /bin/bash
```

To export as singularity image ensure to have quay.io image on your system and procede as follow. Please note that target version of singularity depends on your system:
```
docker run -v /var/run/docker.sock:/var/run/docker.sock -v C:/Users/gobbini/Documents/Project/docker/converted2singularity:/output --privileged -t --rm quay.io/singularity/docker2singularity:v2.6 haddock3
```

Test performed on the cluster as follow (note activtion of the 'myenv' enviroment needed):
```
tmux
srun --nodelist=nightbird --cpus-per-task=32 --mem=64G --pty bash 2> /dev/null
cp -r examples/docking-antibody-antigen/ ~/scratch/haddock
```

NOTA: Ho creato uno script bash per caricare in automatico l'ambiente conda 'myenv' nel singularity ma non funziona

The image contains also two other useful tools for manage structures for docker:
- pdb-tools (installed through pip as a specific version -actually 2.5.0-)
- haddock-restraints (rust program imported as binary in the image)

