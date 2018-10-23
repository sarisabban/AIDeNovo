# ProtAI
Preforms De Novo Design using Machine Learning and PyRosetta to generate a novel synthesic protein structure.

## Requirements:
1. Make sure you install [PyRosetta](http://www.pyrosetta.org) as the website describes.
2. Use the following command (in GNU/Linux) to install all necessary programs and python modules for this script to run successfully:

`sudo apt update && sudo apt full-upgrade && sudo apt install dssp gnuplot python3-biopython python3-pandas python3-numpy python3-matplotlib python3-bs4 python3-lxml python3-scipy python3-keras python3-tqdm`

## Description:
This is a script that uses Machine Learning and PyRosetta to preform *De Novo* Protein Design (from the beginning) i.e. develop and design a synthetic protein structure totally computationally. There is no input for this script, it autonomously generates a topology (random every time) then designs a sequence that fits this topology. It then submits the structure's FASTA sequence to the [Robetta](http://www.robetta.org/) server to generate and download the custom fragment files in preparation for an Abinitio fold simulation. The Abinitio script can be found [here](https://github.com/sarisabban/RosettaAbinitio). Finally it calculates the RMSD for each fragment position on the designed structure and plots an (RMSD vs Position) graph to indicate how good the Abinitio fold simulation might go (idealy you want an average RMSD < 2Å).

The script will generate 1 structure. It is advised to run this script in an array to generate multiple strctures and see which one has the lowest average RMSD fragments. Mind you, if you generate too many structures this might overwhelm the Robetta server by submitting and requesting too many fragment files, please be considirate and run this script once to generate 1 structure at a time only.

## How To Use:
1. You do not need to generate the Machine Learning Dataset, it is already provided (with 82,900 examples), but if you want to replicate the work use the following command to generate the Machine Learning Dataset from the Protein Databank Database (computation time ~168 hours and requires more than 128GB of free disk space):

`python3 Database.py`

This script is complicated, requiring you to carefulyl read the actual script itself and modify it (comment in/out functions) to your needs. One difficult point is the *Human Eye Filtering* step which requires a person to filter out all the unwanted structures before moving onto cleaning up each structure and augmenting the data which eventually results in the following file:

* dataPS.csv

Which stands for dataset of Phi pSi. The default parameters for the Database.py script is isolating proteins between 80 and 150 amino acids, that have more helices and strands than loops, and with an Rg value of less than 15. The script results in a dataset with the first column as the training example number, then the PDB ID of the file (with added strings to identify how it was made), then the angles *Phi* and *Psi*. *0.0* indicates a position with no amino acids, not all protein structures have the same length, but the entire dataset does have the same length and shape because the empty spaces are filled with zeros. If errors occure, that is fine, some protein files will cause errors (and they will be deleted/ignored), but the script should continue all the way to the end and result in a dataset file, given the required functions are commented in/out depending on needs. 

It is best to contact me if you want to generate your own database and I will walk you through it, it is not difficult, but works on individual basis. If you contact me I can also identify what people do not understand about this particular script and I will be able to modify this README file to make it easier to use.

You can then use the following command to train the General Adverserial Neural Netowork on the dataset.

`python3 Generate train.py`

But we provide a trained neural network within the DeNovo.py script, thus you can just go ahead and start your work from the next step.

2. Use the following command to generate a novel protein structure:

`python3 Generate.py`

This script (computation time ~24 hours) will result in 7 files:
* Topology file, which is basically just the structure of the backbone drawn using a sequence of Valine (**backbone.pdb**)
* Sequence designed file (**structure.pdb**)
* Abinitio input files (**structure.fasta**, **frags.200.3mers**, **frags.200.9mers**, **pre.psipred.ss2**)
* Fragment quality plot (**plot_frag.pdb**)

# This script is still under development. This statement will be removed when the script is completed and benchmarked.
# When/if this project is completed, I will make a video explaining it.
