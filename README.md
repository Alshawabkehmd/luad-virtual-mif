# Virtual multiplex immunofluorescence from H&E in lung adenocarcinoma

Analysis code for:

> Alshawabkeh AB, Haddad M, *et al.* **Virtual Multiplex Immunofluorescence from H&E in Lung Adenocarcinoma: A Pre-registered Prognostic Study and a Prespecified Molecular Validation.** (submitted to *Translational Lung Cancer Research*)

We applied GigaTIME-Flash (virtual mIF, 23 channels) to TCGA-LUAD whole-slide H&E images and tested
(1) whether virtual CD8 features predict overall survival (pre-registered, [osf.io/kduvj](https://osf.io/kduvj)) and
(2) whether virtual markers agree with bulk RNA and protein measures (retrospectively registered, [osf.io/jdqas](https://osf.io/jdqas)).
Both studies were negative; see the manuscript for details.

## What is here

| Path | Contents |
|---|---|
| `notebooks/LUAD_vmIF_full_pipeline.ipynb` | End-to-end Colab notebook: data download, tiling, inference, feature extraction, Study 1 survival analysis, Study 2 molecular validation, all figures. Each cell has an explanation above it. |
| `requirements.txt` | Python packages used |

## What is not here

- **Model weights.** GigaTIME-Flash is distributed by its authors at [prov-gigatime/GigaTIME-flash](https://huggingface.co/prov-gigatime/GigaTIME-flash) under their own licence. Request access there and add your token to Colab Secrets as `HF_TOKEN`. Never paste a token into a cell.
- **Data.** All inputs are public: TCGA-LUAD slides and clinical/follow-up files (GDC), Saltz et al. 2018 TIL maps, UCSC Xena RNA-seq, Thorsson et al. 2018 immune scores, TCPA RPPA. The notebook downloads or points to each.

## Running it

1. Open the notebook in Google Colab with a GPU runtime.
2. Add `HF_TOKEN` in Colab Secrets.
3. The notebook uses `MyDrive/luad_project` on Colab (or the `LUAD_ROOT` environment variable elsewhere); change `ROOT` in Part 0 to use another folder.
4. Run top to bottom. Inference over all slides takes several GPU-hours; set `RUN_INFERENCE = False` in Part 0 to reuse saved tile features.

## Notes for reproducing

- Tiles are 128 µm (resized to 256 px) using each slide's own microns-per-pixel; ignoring mpp gives wrong tile sizes.
- Survival uses GDC `follow_up.tsv`; `clinical.tsv` alone drops about 310 living patients.
- The checkpoint stores the patch embedding as LoRA pieces; the loader merges them (scale 2.0) and loads with `strict=True`. Check the printed key report before trusting outputs.
- Random seed 42 throughout.

## Citation

Please cite the manuscript (citation will be added on publication) and the GigaTIME paper.

## Licence

Code: see `LICENSE`. Model weights and data remain under their original licences.
