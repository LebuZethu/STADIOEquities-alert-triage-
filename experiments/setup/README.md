# Experimental setup

Defines *how* each experiment is run.

This folder holds configuration files and pipeline definitions that specify
the experimental design — for example: train/test split parameters,
cross-validation settings, class-imbalance handling choices, the list of
candidate models, and the hyperparameter search space for each run.

Keeping setup separate from results means any experiment can be reproduced
from what is stored here.