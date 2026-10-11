# BLiMPPI

**BLiMPPI: Benchmark of Linguistic Minimal Pairs for Polarity Items**

Over 12,000 minimal pairs for negative polarity items (NPIs) and negative concord items in 33 languages. Each pair contrasts a grammatical sentence containing a licensed NPI with an ungrammatical counterpart in which the licenser has been removed or replaced.

This repository accompanies a paper under anonymous review.

## Data

Pairs are in `final_pairs/npi/`, one folder per language:

- `<language>_minimal_pairs.tsv`: the pairs. `sen` is the grammatical sentence and `wrong_sen` the ungrammatical one; further columns give the NPI lemma, the licensing environment (`env`) and the NPI–licenser distance.
- `<language>_stats.csv`: counts of licensing environments per NPI lemma in the source corpus.
- `<language>_output.json`: full generation output with per-pair metadata (all languages except Dutch).

## Code

```bash
pip install -r requirements.txt
pip install -e .
```

**Scoring a model.** A pair counts as correct when the model assigns the grammatical sentence a higher summed log-probability than the ungrammatical one.

```bash
python scripts/lm_eval/eval_model.py --model_name <huggingface-model-id> --data_dir final_pairs
```

Per-language scores and a `summary.json` are written to `results/model_results/<model>/main/`. The n-gram baseline is `scripts/n-gram_scorer.py` (KenLM); set the paths at the top of the script before running it.

**Regenerating the pairs.** This requires UD v2.18 in `resources/ud/ud-treebanks-v2.18/`. The 12 languages whose scripts use UniMorph inflection (Belarusian, Finnish, German, Hungarian, Lithuanian, Persian, Polish, Romanian, Russian, Slovenian, Turkish, Ukrainian) also need UniMorph in `resources/unimorph/` and the UD-derived inflection tables built by `scripts/ud2um.py`. From the repository root:

```bash
python -m src.blimppi.languages.<module>
```

There is one module per language; `bul_srp_hrv` covers Bulgarian, Serbian and Croatian, and `norwegian` covers both Norwegian varieties. The NPI Bank part of the English subset is added by `src/blimppi/npi-bank_convert.py`. The Dutch subset is built by `src.blimppi.languages.dutch_npu` from `resources/npu/npu.csv`.

**Analysis.** `src/analysis/` computes per-lemma accuracy (`compute_accuracy.py`) and the entropy of each NPI's licensing environments (`compute_entropy.py`).

The code is licensed under Apache-2.0 (see `LICENSE`).

## Sources and licenses

BLiMPPI is derived from existing resources, and each language subset keeps the license of the resource it was built from. Using a subset means accepting the terms of its source.

### Universal Dependencies (32 languages)

All languages except Dutch are built from Universal Dependencies v2.18 treebanks. English also includes pairs from NPI Bank (see below).

| Language | UD treebank | License |
|---|---|---|
| Armenian | ArmTDP, BSUT | CC BY-SA 4.0 |
| Belarusian | HSE | CC BY-SA 4.0 |
| Bulgarian | BTB | CC BY-NC-SA 3.0 |
| Catalan | AnCora | CC BY 4.0 |
| Chinese | CFL, GSDSimp | CC BY-SA 4.0 |
| Chinese | Beginner | CC BY-NC-SA 3.0 |
| Croatian | SET | CC BY-SA 4.0 |
| Czech | CAC, CLTT, PUD | CC BY-SA 4.0 |
| Czech | FicTree | CC BY-NC-SA 4.0 |
| Danish | DDT | CC BY-SA 4.0 |
| English | PUD | CC BY-SA 3.0 |
| English | EWT | CC BY-SA 4.0 |
| English | GENTLE, GUM, LinES, ParTUT | CC BY-NC-SA 4.0 |
| Finnish | FTB | CC BY 4.0 |
| Finnish | OOD, PUD, TDT | CC BY-SA 4.0 |
| German | GSD, HDT | CC BY-SA 4.0 |
| Greek | GUD | CC BY-SA 4.0 |
| Greek | GDT | CC BY-NC-SA 3.0 |
| Greek | GLCII | CC BY-NC-SA 4.0 |
| Hebrew | IAHLTknesset, IAHLTwiki | CC BY-SA 4.0 |
| Hebrew | HTB | CC BY-NC-SA 4.0 |
| Hindi | PUD | CC BY-SA 3.0 |
| Hindi | HDTB | CC BY-NC-SA 4.0 |
| Hungarian | Szeged | CC BY-NC-SA 3.0 |
| Icelandic | GC, Modern, PUD | CC BY-SA 4.0 |
| Indonesian | CSUI, GSD | CC BY-SA 4.0 |
| Lithuanian | ALKSNIS, HSE | CC BY-SA 4.0 |
| Norwegian Bokmål | Bokmaal | CC BY-SA 4.0 |
| Norwegian Nynorsk | Nynorsk | CC BY-SA 4.0 |
| Persian | PerDT, Seraji | CC BY-SA 4.0 |
| Polish | PUD | CC BY-SA 4.0 |
| Polish | PDB | CC BY-NC-SA 4.0 |
| Polish | LFG | GNU GPL 3.0 |
| Romanian | RRT, SiMoNERo | CC BY-SA 4.0 |
| Russian | PUD | CC BY-SA 3.0 |
| Russian | GSD, Taiga | CC BY-SA 4.0 |
| Russian | SynTagRus | CC BY-NC-SA 4.0 |
| Serbian | SET | CC BY-SA 4.0 |
| Slovak | SNK | CC BY-SA 4.0 |
| Slovenian | SSJ | CC BY-SA 4.0 |
| Spanish | AnCora | CC BY 4.0 |
| Spanish | GSD | CC BY-SA 4.0 |
| Swedish | SweLL, Talbanken | CC BY-SA 4.0 |
| Swedish | LinES | CC BY-NC-SA 4.0 |
| Turkish | PUD | CC BY-SA 3.0 |
| Turkish | BOUN, FrameNet, GB, Kenet, Penn | CC BY-SA 4.0 |
| Turkish | IMST | CC BY-NC-SA 4.0 |
| Ukrainian | ParlaMint | CC BY-SA 4.0 |
| Ukrainian | IU | CC BY-NC-SA 4.0 |
| Urdu | UDTB | CC BY-NC-SA 4.0 |

Several of these licenses are share-alike (SA) and some are non-commercial (NC); Polish LFG is under GNU GPL 3.0. An NC treebank's subset may not be used for commercial purposes.

### English: NPI Bank

Part of the English subset is built from NPI Bank (Buschbom et al., 2025), used with permission of its maintainers at SFB 1629 NegLaB, Goethe University Frankfurt. See https://npibank.neglab.de for the terms of use.

### Dutch: Dutch NPI Lexicon

The Dutch subset is built from Hoeksema (2024), *De negatief-polaire uitdrukkingen van het Nederlands: Inleiding en lexicon*, used with permission of the author.

## Citation

Citation details for BLiMPPI are withheld during anonymous review.

Please cite the resources the data is built on. Cite Universal Dependencies, and the papers of the individual treebanks you use (listed in each treebank's README):

```bibtex
@article{demarneffe2021ud,
  title   = {Universal {D}ependencies},
  author  = {de Marneffe, Marie-Catherine and Manning, Christopher D. and Nivre, Joakim and Zeman, Daniel},
  journal = {Computational Linguistics},
  volume  = {47},
  number  = {2},
  pages   = {255--308},
  year    = {2021},
  doi     = {10.1162/coli_a_00402},
}
```

If you use the English subset, cite NPI Bank:

```bibtex
@misc{buschbom2025npibank,
  title        = {{NPI} {B}ank},
  author       = {Buschbom, Dirk and Hohmann, Pascal and Lamoure, Nicolas and Richter, Frank and Sailer, Manfred},
  year         = {2025},
  howpublished = {Frankfurt: SFB 1629 Negation in Language and Beyond (NegLaB)},
  url          = {https://npibank.neglab.de},
  note         = {DB 20260603\_001 v1.0.1, GUI v1.1.0},
}
```

If you use the Dutch subset, cite the Dutch NPI Lexicon:

```bibtex
@book{hoeksema2024npi,
  title     = {De negatief-polaire uitdrukkingen van het {N}ederlands: Inleiding en lexicon},
  author    = {Hoeksema, Jack},
  year      = {2024},
  publisher = {University of Groningen Press},
  doi       = {10.21827/656dea670d2ba},
}
```
