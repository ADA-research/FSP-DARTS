# FSP-PC-DARTS

This project is built upon [PC-DARTS](https://github.com/yuhuixu1993/PC-DARTS). Inspired by the study of the Flow of Solution Procedure (FSP) matrix in [RobNets](https://github.com/gmh14/RobNets), we incorporate FSP as a robustness-related metric into the PC-DARTS search process.


Experimental results are organised in the following directories:

- `preliminary_study`
- `ablation`
- `final_test`


### The main file directory of the project


This project is based on the [PC-DARTS](https://github.com/yuhuixu1993/PC-DARTS) source code, with added scripts for FSP evaluation and modifications to some scripts. The primary scripts used are listed below.
```

├── fsp_test.py         # FSP evaluation for a single discrete model
├── fsp_adv_test.py     # FSP evaluation for a single discrete model under after additional fine-tuning with adversarial examples
├── fsp_search.py       # FSP evaluation and architecture parameter update integrated into PC-DARTS search
├── model_search.py     # Add methods to construct the discrete candidate pool for fsp_search
├── train_search.py     # Add the FSP evaluation process to the search
├── train_adv.py        # Perform full adversarial training on the genotypes found by the search.
├── final_test.py       # Robustness testing of the final trained model
└── utils.py            # Add mean and standard deviation for input normalisation of the new dataset; update some incompatible legacy code.

```

- `fsp_test.py` : Perform FSP and robustness evaluations(default: PGD-1) on the genotypes found by PC-DARTS. Load the genotypes and network weights, then fine-tune for one epoch using clean inputs.<br><br>
- `fsp_adv_test.py` : Models fine-tuned using clean data alone failed to detect the correlation between FSP and adversarial accuracy; adversarial examples are used in addition for fine-tuning. Allow flexible configuration of training and evaluation parameters and datasets. Default: CIFAR-10, 1-epoch clean + 5-epoch PGD-7 adversarial fine-tuning, and PGD-7 evaluation.<br><br>
- `fsp_search.py` : Integrate FSP evaluation into the PC-DARTS search process. The configurable parameters `N`, `K`, `λα / λβ` control the evaluation interval, the number of candidate architectures, and the reward strengths for the updates, respectively. Default: FSP evaluation starts at epoch 16 and is skipped during the final search epoch.