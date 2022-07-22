## Fork
This is a fork of [zhangchonglab/sgRNA-cleavage-activity-prediction](https://github.com/zhangchonglab/sgRNA-cleavage-activity-prediction) updated to work with Python 3.8+ and Scikitlearn 1.1


# sgRNA-cleavage-activity-prediction

## What is this?
This python script collection is used to predict the activity scores of sgRNAs (CRISPR/Cas9). This program is especially useful for biologists and bioengineerers interested in using CRISPR/Cas9 genome editing technology in bacteria, because the sgRNA activity dataset used to train this model is experimentally determined in model bacteria *E. coli*, which is the first such dataset obtained in bacterial host to the best of our knowledge. Besides, we found that previous sgRNA activity models trained by mammalian cell line screening data have poor performance on our experimentally measured dataset in bacteria. We published a paper (Guo J, Wang T, Guan C, Liu B, Luo C, Xie Z, Zhang C, Xing X-H (2018) Improved sgRNA design in bacteria via genome-wide activity profiling. Nucleic Acids Res 46:7052–7069 . doi: 10.1093/nar/gky572) comprehensively describing this algorithm and relevant experiments to produce the dataset. Please cite the paper if this program is useful to your work.

## General description of the algorithm
The utility of this software is fairly simple. Given a fasta file containing the target DNA sequences (N<sub>4</sub>N<sub>20</sub>NGGN<sub>3</sub>), the program will predict activity scores for each one, with output in .csv format. The algorithm uses a gradient boosting regression tree model to predict sgRNA activity. The model is trained on the experimentally characterized sgRNA activity dataset obtained in bacteria.

## How to use it?
### Step 1：Installation
Install dependencies with `pip install -r requirements.txt`

### Step 2：Prepare the necessary files.
All these files (or subdirectories) should be organized under a common working directory together with the all .py scripts.
Please check the example files post at GitHub, which are described as below.

#### File 1: fasta file of DNA target sequences (see example_sgRNA.fasta)
The fasta file contains the target DNA sequences with N<sub>4</sub>N<sub>20</sub>NGGN<sub>3</sub> format (N = A, T, C, G).

#### File 2: configure file (see example_configure.txt)
The configure file is used to set all the necessary parameters and tell the program where to find necessary files. This file is in a two-column format using colon (:) as delimiter. Each line starts with one word (name of one parameter) separated with the following (setting of this parameter) by a colon delimiter. We describe each parameter as below.

**targetFasta**: The location of the abovementioned fasta file of DNA target sequences.

**model**: This software provides two options to choose a model to predict sgRNA activity, "Cas9" and "eSpCas9". For the details, please read our paper. Briefly, "Cas9" refers to the wild type Cas9 nuclease, which is currently most widely used in CRISPR/Cas genome editing. By contrast, "eSpCas9" is a off-target-reducing mutant of "Cas9" with three amino acid mutations.  

**normalization**: Because we use Z score of the original dataset to train the model, hence the output for each sgRNA is a real number typically located between (0, 50). To normalize the result, we provide here an option to linearly project the result onto (0, 1). "Yes" or "No" should be specified here. 

**prefix**: prefix used for naming of all output files, keep it simple. For example, "myprediction" is fine.

Below is **an example configure file with default parameters**.

parameter|value
---------|-----
[configdesign]
targetFasta:|example_target.fasta
model:|Cas9
normalization:|Yes
prefix:|example

### Step 4：Run the pipeline
Open the command line window (for example, terminal in Macbook), cd to the working directory and run the analysis pipeline.

cd path_to_your_working_directory

python sgRNA_activity_predict_main.py configure

We also post a toy example together with the scripts and the example_configure.txt has been edit to make it compatible. For this test, cd to the working directory, type in: 

python sgRNA_activity_predict_main.py example_configure.txt

Check [here](./image/successful_running.png) for the output during a successful running of the abovementioned test.

## Output description
**prefix_result.txt**: file in .csv format (tab delimiter) with one header line containing ID and the activity score for each sgRNA.

sgRNAID|score
-------|--------------------
pyrC_259|0.765076211102
pyrD_534|0.561630568706
...|...
