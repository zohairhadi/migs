# MIGS

This is the code for the paper "MIGS: Meta Image Generation from Scene Graphs"

This code is build upon SG2Im work, the code for which might be found here: https://github.com/google/sg2im

## Setup
You can setup a virtual environment to run the code like this:

```bash
python3 -m venv env               # Create a virtual environment
source env/bin/activate           # Activate virtual environment
pip install -r requirements.txt   # Install dependencies
echo $PWD > env/lib/python3.5/site-packages/sg2im.pth  # Add current directory to python path
# Work for a while ...
deactivate  # Exit virtual environment
```
Please note that some packages may be missing from requirments.txt as this code is work in progress. You might have to install them manally via pip.

## Meta-learning labels and datasets
You would need to manually download BDD100k and Action Genome datasets.
For Action Genome you would also need to dump the labeled frames from the charades dataset.
We provide the labels that we used for all of our models, including the tasks splits for meta-learning in the anonymized google drive:

BDD100k: https://drive.google.com/file/d/1xb0JCtaYDO0VTrflUqnVqzbu_xZh1xTs/view?usp=sharing

Action Genome: https://drive.google.com/file/d/1EI92U15Kzb7vdG3Rxgk8AoiH2RNlRxsb/view?usp=sharing

## Pretrained Models
You can download pretrained models of MIGS with SPADE generator for both BDD and AG dataset from anonymized google drive via this link:
https://drive.google.com/file/d/1fzn85eURnsHaO2iCjKIa_bLgWnYDUKMP/view?usp=sharing
 
Please note that these are the weights for meta models, so they have to be fine tuned (trained for several iterations) to a specific task using the meta learning labels. 

## Training Models
For training a model, you need to first specify its cofig file or choose one of the configs we provide in configs folder (includes all of the models from the paper).
Then the model can be trained using the following command:

```bash
python scripts/run_train.py configs/migs_spade_ag.yaml
```

NOTE: make sure to correct all the paths in the confis with the locations to the downloaded datasets. 

## Fine-tuning and evaluating the models
To fine tune the meta learning weights on a specific tasks and evaluate the fid and kid, you can use the following scripts: 
`scripts/eval_ag_meta.py`
`scripts/eval_bdd_meta.py`
Both of these scripts take the config file of the model to be tested (all of the configs from our paper can be found in configs folder). The weights for the model are loaded from the output_dir folder, specified in the config file.     
## Running Models
You can use the script `scripts/run_model.py` to easily run any of the pretrained models on new scene graphs using a simple human-readable JSON format. For example:

```bash
python scripts/run_model.py \
  --checkpoint meta_spade_bdd_daytime_clear_highway.pt \
  --scene_graphs scene_graphs/bdd.json \
  --output_dir outputs
```

The generated images will be saved to the directory specified by the `--output_dir` flag. 

