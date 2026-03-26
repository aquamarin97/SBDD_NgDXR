# In Silico Identification of Novel Inhibitors Targeting NgDXR in *Neisseria gonorrhoeae*

<p align="center">
  <img src="https://tez.yok.gov.tr/UlusalTezMerkezi/tezSorguSonucYeni.jsp" alt="MSc Thesis"/>
</p>

---

## Abstract

Antibiotic-resistant *Neisseria gonorrhoeae* is classified as a **WHO high-priority pathogen**, with an estimated 82.4 million new infections worldwide in 2020. Current treatment options are rapidly diminishing due to emerging resistance.

This MSc thesis applied a complete **structure-based drug design (SBDD) pipeline** to identify novel inhibitors of **NgDXR** (1-deoxy-D-xylulose 5-phosphate reductoisomerase) — a key enzyme in the methylerythritol phosphate (MEP) pathway with **no human ortholog**, making it an ideal selective antibacterial target.

**17 novel compounds** were identified as potential NgDXR inhibitors — the first time computational chemistry methods have been used to discover DXR inhibitors targeting *N. gonorrhoeae*.

---

## Project Context

| Detail | Info |
|--------|------|
| **Thesis** | MSc Biotechnology, Yıldız Technical University (May 2023) |
| **Supervisor** | Prof. Dr. Dilek Balık |
| **Co-supervisor** | Assoc. Prof. Dr. Özal Mutlu |
| **Funding** | TÜBİTAK Project No. 121N818 |
| **Collaboration** | International research project (Germany × Thailand × Turkey) |
| **Author** | Mustafa Necati Haşimoğlu |

---

## Computational Pipeline

```
NgDXR Gene Sequence (WP_180374256.1)
        │
        ▼
┌─────────────────────────────────┐
│  1. Sequence Analysis & Alignment │ ──→ NCBI BLAST, MUSCLE, ClustalW
│     (vs. X-ray resolved DXRs)     │     100% conservation of catalytic residues
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  2. Homology Modeling             │ ──→ MODELLER 10.1 (500 models each)
│     • Closed model (from 2EGH)   │     Template: EcDXR + FSM + NADP + Mg²⁺
│     • Semi-open model (from 3ANM) │     Template: EcDXR + Compound 8 + NADP
│     + Energy minimization (YASARA)│     DOPE score selection
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  3. Model Validation              │ ──→ ERRAT: 98.42 / 97.65
│                                   │     VERIFY3D: 99.49% / 99.75%
│                                   │     QMEAN: 0.09 / 0.00
│                                   │     QMEANDisCo: 0.80 / 0.81
│                                   │     Ramachandran: 94.2% / 93.6% MFR
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  4. Cognate Docking Validation    │ ──→ Glide XP (Schrödinger)
│     FSM → 2EGH: Lig_RMSD 0.92 Å │     Confirmed algorithm reliability
│     Cmpd8 → 3ANM: Lig_RMSD 0.89Å│     for DXR proteins
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  5. Reference Docking             │ ──→ FSM → Closed-NgDXR
│     XP GScore: -8.748 kcal/mol   │     DockingScore: -8.72 kcal/mol
│     + 100 ns MD validation        │     Desmond (Schrödinger)
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  6. Library Preparation           │ ──→ ~4.5M compounds
│     Enamine Screening: 2.9M      │     LigPrep + OPLS4 force field
│     ChemDiv: 1.6M                 │     Epik ionization (pH 7.5 ± 0.5)
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  7. Hierarchical Virtual Screening│ ──→ Glide (Schrödinger Maestro)
│     HTVS → top 10%               │
│     SP   → top 10%               │
│     XP   → top 1000              │
│     + QikProp ADME/Lo5 filtering  │     Cutoff: XP score ≤ -9.0 kcal/mol
│     + QPLD (QM-polarized docking) │     B3LYP/6-31G* charge calculation
│     + IFD (Induced Fit Docking)   │     Prime refinement (5.0 Å shell)
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  8. MD Simulations (100 ns each)  │ ──→ AMBER18 + GAFF2 + ff14SB
│     31 top compounds + FSM ref    │     TIP3POX solvent, 150 mM NaCl
│     + 10 analogs of Cmpd 426646   │     Langevin thermostat (300 K)
│     = 41 total simulations        │     PME electrostatics
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│  9. Post-MD Analysis              │ ──→ RMSD (<3.5 Å cutoff)
│     RMSD / RMSF / No-fit RMSD    │     No-fit RMSD (<3.0 Å cutoff)
│     H-bond occupancy              │     Mg²⁺ coordination analysis
│     Interaction fingerprints      │
└─────────────┬───────────────────┘
              ▼
┌─────────────────────────────────┐
│ 10. Binding Free Energy           │ ──→ MM/GBSA (MMPBSA.py, AMBER18)
│     ΔG ranking of all candidates  │
│     Comparison vs. FSM reference  │
└─────────────┬───────────────────┘
              ▼
        17 Novel NgDXR
        Inhibitor Candidates
```

---

## Key Scientific Insights

### Why Two Conformational Models?

DXR undergoes a significant conformational change upon ligand binding — a **flexible loop** closes over the active site:

- **Closed conformation** (template: 2EGH) — Fosmidomycin (FSM) binds via Mg²⁺-dependent mechanism, causing full loop closure
- **Semi-open conformation** (template: 3ANM) — Lipophilic inhibitors (Compound 8) interact with Trp211 via π-π stacking, preventing full closure

Both models were screened to capture inhibitors operating through **different binding mechanisms**.

### Why NgDXR?

- The MEP pathway is **absent in humans** (humans use the mevalonate pathway)
- DXR catalytic residues are **100% conserved** across all examined species
- Known DXR inhibitors (FSM, FR900098) have **poor oral bioavailability** due to high hydrophilicity
- This work aimed to find **more lipophilic** alternatives

---

## Results Summary

| Metric | Value |
|--------|-------|
| Total compounds screened | ~4,500,000 |
| Compounds after HTVS | ~450,000 |
| Compounds after SP | ~45,000 |
| XP final candidates | Top 1,000 |
| Post-ADME/QPLD/IFD selection | 90 compounds |
| MD simulations completed | 31 compounds + FSM + 10 analogs |
| **Novel inhibitor candidates** | **17 compounds** |
| Key lead compound | **Compound 426646** |
| Lipophilic leads (improved vs FSM) | **Compounds 649428 & 426646** |

### Validation Scores (Post-Minimization)

| Parameter | Closed NgDXR | Semi-open NgDXR |
|-----------|:------------:|:---------------:|
| ERRAT | 98.42 | 97.65 |
| VERIFY3D | 99.49% | 99.75% |
| QMEAN | 0.09 | 0.00 |
| QMEANDisCo | 0.80 | 0.81 |
| Ramachandran (MFR) | 94.2% | 93.6% |

---

## Tools & Technologies

| Category | Tools |
|----------|-------|
| **Homology Modeling** | MODELLER 10.1 |
| **Energy Minimization** | YASARA Minimization Server |
| **Model Validation** | ERRAT, VERIFY3D, QMEAN, QMEANDisCo, Ramachandran (SAVES Server) |
| **Molecular Docking** | Schrödinger Maestro 13.0 — Glide (HTVS/SP/XP), QPLD, IFD |
| **Protein Preparation** | Protein Preparation Wizard, PROPKA (pH 7.5), OPLS3 |
| **Ligand Preparation** | LigPrep, Epik, OPLS4 |
| **ADME Profiling** | QikProp (Lipinski's Rule of Five) |
| **MD Simulation** | AMBER18 (ff14SB + GAFF2), Desmond (Schrödinger) |
| **Free Energy** | MM/GBSA (MMPBSA.py) |
| **Visualization** | PyMOL |
| **Sequence Analysis** | NCBI BLAST, MUSCLE, ClustalW |
| **Databases** | PDB, NCBI, PubChem, Enamine REAL, ChemDiv |
| **Hardware** | Intel i7-11700F, NVIDIA GPU, 32 GB RAM |

---

## Repository Structure

```
ngdxr-drug-design/
├── README.md
├── docs/
│   ├── figures/
│   │   ├── workflow_pipeline.png
│   │   ├── sequence_alignment.png        
│   │   ├── closed_conformation_with_Mg.png
│   │   ├── semi-open_conformation_without_Mg.png
│   │   ├── cognate_docking_poses.png  
│   │   ├── fsm_ngdxr_interaction.png 
│   │   ├── interaction_during_100ns_in_md_simulation.png    
│   │   ├── md_rmsd_complexes.png         
│   │   ├── md_rmsf_complexes.png 
│   │   ├── nofit_rmsd.png           
│   │   ├── mmgbsa_results.png
│   │   └── analog_results.png
│   └── thesis_abstract.pdf
├── models/
│   ├── NgDXR_semi-open_models/
│   └── ngdxr_closed_models/
├── docking/
│   └── reference_docking_results_example.maegz
└── requirements.txt
```

---

## Future Directions (from Thesis Recommendations)

1. Determine NgDXR 3D structure via **X-ray crystallography**
2. Extend MD simulations to **1 microsecond** for confirmed stable binders
3. Determine **IC50 values** through kinetic assays (*in vitro*)
4. For compounds with IC50 ≤ 1 μM: **medicinal chemistry optimization** to enhance lipophilicity
5. Experimental validation by collaborating wet-lab teams

---

## Citation

```bibtex
@mastersthesis{hasimoglu2024ngdxr,
  title     = {In Silico Identification of Novel Inhibitors Targeting 
               1-deoxy-D-xylulose 5-phosphate Reductoisomerase 
               in Neisseria gonorrhoeae},
  author    = {Haşimoğlu, Mustafa Necati},
  school    = {Yıldız Technical University, Graduate School of Science and Engineering},
  year      = {2024},
  type      = {MSc Thesis},
  department= {Biotechnology},
  note      = {Supervised by Prof. Dr. Dilek Balık, 
               Co-supervised by Assoc. Prof. Dr. Özal Mutlu. 
               Funded by TÜBİTAK Project No. 121N818}
}
```

---

## Author

**Mustafa Necati Haşimoğlu** — MSc Biotechnology | Bioengineer

Specializing in computational drug design, molecular modeling, and scientific software development.

📫 [your.email@example.com](mailto:your.email@example.com)  
🔗 [LinkedIn](https://linkedin.com/in/USERNAME) | [Upwork](https://upwork.com/freelancers/USERNAME)

---

## Acknowledgments

- Prof. Dr. Dilek Balık (Supervisor) — Yıldız Technical University
- Assoc. Prof. Dr. Özal Mutlu (Co-supervisor)
- Dr. Osman Mutluhan Uğurel, Erennur Uğurel, Tuğba Gül İnci
- Recombinant DNA Technologies Laboratory, YTU
- TÜBİTAK (Project No. 121N818)

---

## License

Academic research project. Shared for portfolio and educational purposes.  
Compound structures and detailed binding data are subject to publication restrictions.