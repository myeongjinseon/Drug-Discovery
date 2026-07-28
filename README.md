# Drug Discovery Camp

Hands-on materials for a two-day introduction to Structure-Based Drug Discovery (SBDD). The notebooks cover preparation of 3D protein and ligand structures, binding-site prediction, molecular docking with AutoDock Vina, and cross-docking analysis.

## Learning objectives

- Generate docking-ready 3D ligand structures from SMILES strings.
- Download and prepare PDB structures, then convert receptors to PDBQT format.
- Predict binding sites using protein structure alone.
- Select co-crystal structures as templates and compare docking outcomes.
- Interpret cross-docking results using binding scores, pocket-center distances, and self-docking RMSD.

## Contents

| Day | Notebook | Topic |
| --- | --- | --- |
| Day 1 | `Day_1_Session_2.ipynb` | Ligand preparation, protein preparation, and introductory molecular docking |
| Day 2-1 | `Day_2_Session_1_1_Binding_Site_Prediction.ipynb` | LIGSITE-style binding-site prediction and comparison with co-crystal ligands |
| Day 2-2 | `Day_2_Session_1_2_Template_Selection.ipynb` | Abl kinase template selection (1IEP/2GQG), Imatinib and Dasatinib docking, decoy evaluation, and AlphaFold comparison |
| Day 2-3 | `Day_2_Session_2_2_Corss_Dock.ipynb` | Cross-docking with a team-selected template and calculation of evaluation metrics |

The repository also includes the accompanying lecture slides:

- `day1-session1.pdf`
- `day1-session2.pdf`
- `day2-session1.pdf`

## Quick start

Python 3.10+ and Jupyter Notebook or Google Colab are recommended. Run the first installation cell in each notebook, then execute its cells from top to bottom. An internet connection is required to download PDB structures and AlphaFold models.

```bash
git clone <repository-url>
cd Drug-Discovery
python -m pip install jupyter
jupyter notebook
```

To install the main libraries in advance in a local environment, run:

```bash
python -m pip install rdkit "meeko==0.7.1" vina gemmi py3Dmol numpy scipy
```

`Day_2_Session_2_2_Corss_Dock.ipynb` automatically downloads an AutoDock Vina 1.2.5 executable for Linux. When running locally on macOS or Windows, update that cell with an operating-system-appropriate Vina installation path, or use Google Colab instead.

## Recommended workflow

1. Start with `Day_1_Session_2.ipynb` to learn the SMILES → 3D conformer → PDBQT workflow.
2. In `Day_2_Session_1_1_Binding_Site_Prediction.ipynb`, predict binding pockets for 1IEP and 1ERE, then compare them with the centers of their co-crystal ligands.
3. In `Day_2_Session_1_2_Template_Selection.ipynb`, compare 1IEP (DFG-out) and 2GQG (DFG-in) to select a template appropriate for the ligand of interest.
4. In the configuration cell of `Day_2_Session_2_2_Corss_Dock.ipynb`, replace `TEAM_TEMPLATE`, `LIGAND_NAME`, and `LIGAND_SMILES` with your team's assignment values, then run the notebook.

## Interpreting cross-docking results

| Metric | Meaning | Interpretation |
| --- | --- | --- |
| Vina score (kcal/mol) | Predicted binding affinity | A lower (more negative) score is generally a more favorable prediction. |
| Pocket distance (Å) | Distance between the docked pose and co-crystal ligand centers | A smaller value indicates closer agreement on the pocket center. |
| RMSD (Å) | Difference from the co-crystal pose in self-docking | A smaller value indicates better reproduction of the co-crystal pose. |
| ROC-AUC / Top-3 | Template performance for separating actives and decoys | A higher value suggests a more useful template for virtual screening. |

Docking scores are model-based predictions, not experimental binding affinities. Interpret them alongside the protein conformational state, protonation, search-box settings, and ligand conformers.

## Main tools and data sources

- [RDKit](https://www.rdkit.org/): molecular parsing, 3D conformer generation, and structure processing
- [Meeko](https://forli-lab.github.io/Meeko/): ligand and receptor PDBQT preparation
- [AutoDock Vina](https://autodock-vina.readthedocs.io/): molecular docking and scoring
- [Gemmi](https://gemmi.readthedocs.io/): PDB structure processing
- [py3Dmol](https://3dmol.csb.pitt.edu/): in-notebook 3D molecular visualization
- [RCSB PDB](https://www.rcsb.org/) and [AlphaFold DB](https://alphafold.ebi.ac.uk/): structural data sources

## Generated files and notes

Running the notebooks creates intermediate files in the current directory, including `.pdb`, `.pdbqt`, `.sdf`, `docked.*`, `*_clean.pdb`, and `*_receptor.pdbqt`. For reproducible comparisons, retain the random seeds and search-box settings used in the notebooks.

These materials are intended for education. Assessing a candidate compound's real-world activity, safety, or developability requires experimental validation and additional computational studies.
