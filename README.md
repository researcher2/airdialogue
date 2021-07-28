# AirDialogue
AirDialogue is a benchmark dataset for goal-oriented dialogue generation
research. This python library contains a collection of tookits that come with the dataset.
- [AirDialogue paper][paper]
- [AirDialogue dataset][data]
- Reference implementation: [AirDialogue Model][airdialogue_model]

## What's New

- Jul 13,2020: Fixed a bug in BLEU evaluation. The current version gives higher BLEU scores. Support evaluation for different roles and add KL-divergence metric (see `--infer_metrics`).
- Jul 12,2020: We update the [AirDialogue dataset][data] to version v1.1. We fixed typos, misalignment between KB file and dialogue file. Please download and use the new data.


## Prerequisites
#### General
- python (verified on 3.7)
- wget

#### Python Packages
- tensorflow (tested on 1.15.0)
- tqdm
- nltk
- flask (for visualization)

## Install
<!-- To install the pre-build version from pip, use
```
pip install airdialogue
``` -->

To install the bleeding edge from github, use
```
python setup.py install
```

## Quick Start
#### Scoring
The official scoring function evaluates the predictive results for a trained model and compare it to the AirDialogue dataset.

```
airdialogue score --true_data PATH_TO_DATA_FILE --true_kb PATH_TO_KB_FILE \
    --infer_metrics bleu
```

`--infer_metrics` can be one of (bleu:all|rouge:all|kl:all|bleu:brief|kl:brief).
`brief` mode gives a single number metric. (bleu|kl) is equivalent to (belu:brief|kl:brief)

#### Context Generation
Context generator generates a valid context-action pair without conversatoin history.
```
airdialogue contextgen \
    --output_data PATH_TO_OUTPUT_DATA_FILE \
    --output_kb PATH_TO_OUTPUT_KB_FILE \
    --num_samples 100
```

#### Preprocessing
AirDialogue proprocess tookie tokenizes dialogue. Preprocess on AirDialogue data requires 50GB of ram to work.
Parameter job_type is a set of 5 bits separted by `|`, which reqpresents `train|eval|infer|sp-train|sp-eval`.
Parameter input_type can be either `context` for context only data or `dialogue` for dialogue data with full history.
```
airdialogue prepro \
  --data_file PATH_TO_DATA_FILE \
  --kb_file PATH_TO_KB_FILE \
  --output_dir "./data/airdialogue/" \
  --output_prefix 'train' --job_type '0|0|0|1|0' --input_type context
```

#### Simulator
Simulator is built on top of context generator that provides not only a context-action pair but also a full conversation history generated by two templated chatbot agents.
```
airdialogue sim \
    --output_data PATH_TO_OUTPUT_DATA_FILE \
    --output_kb PATH_TO_OUTPUT_KB_FILE \
    --num_samples 100
```

#### Visualization
Visualization tool displays the content of the raw json file.
```
airdialogue vis --data_path ./data/airdialogue/json/
```

#### Codalab simulator
To simulate running the Codalab selfplay workflow, you can run the following script that replicates the bundle workflow
for the competition. This requires a `model/scripts/codalab_selfplay_step.sh` that can be run as

```bash
sh scripts/codalab_selfplay_step.sh out.txt data.json [kb.json]
```

More details can be found on the [Airdialogue competition tutorial worksheet][airdialogue_tutorial] on Codalab.

```bash
bash airdialogue/codalab/simulate_codalab.sh <path_to_data>/json/dev_data.json <path_to_data>/json/dev_kb.json <model_folder>
```

[data]: https://storage.googleapis.com/airdialogue/airdialogue_data.tar.gz
[paper]: https://www.aclweb.org/anthology/D18-1419/
[airdialogue_model]: https://github.com/google/airdialogue_model
[airdialogue_tutorial]: https://worksheets.codalab.org/worksheets/0xa79833f4b3c24f4188cee7131b120a59
