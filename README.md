# Matignon Interpreters

Interpreter labels for Matignon-LSF, the largest corpus of interpreted French Sign Language (LSF).

Each of the 242 segments of the 67 videos has an interpreter ID, and the 175 transitions between interpreters are marked.

Paper: *Interpreter Annotations for Matignon-LSF* (arXiv link coming soon).

## Why this project exists
Side project to contribute to research on LSF.

Signer-independent evaluation needs to know who signs where. Matignon-LSF reports 15 interpreters, but not which one appears in each segment. This repository adds that layer.

## What's in it
- `annotations/annotation_protocol.md`: how the labels were produced.
- `annotations/Matignon-LSF_video_list.csv`: the 67 video IDs and metadata (from the Matignon-LSF team).
- `results/annotation_results.csv`: the labels (one row per segment, with transitions).
- `results/cooccurrence.csv`: how many videos each interpreter pair shares.
- `results/coocc_src.csv`: intermediate annotator-video mapping used to build `cooccurrence.csv`.
- `scripts/`: the scripts used for downloading, checking and analyzing the data.

## What's not in it
- **The videos.** They are not redistributed here. You can watch them in this YouTube playlist: https://www.youtube.com/playlist?list=PLgtU_g_Bn-2Y1tvjm97zGzEZDhnpiM6Au, or rebuild the corpus with the Matignon-LSF toolkit: https://github.com/JulieLascar/Matignon-LSF
- **The interpreter gallery.** It stays private to protect the interpreters' privacy. Only arbitrary IDs are released.

## Main findings
- 15 interpreters, but the corpus behaves like 5 of equal size.
- Every video has at least two interpreters, so a strict interpreter- and video-disjoint split discards 23–34% of the remaining data.
- The only splits that discard nothing are 3 small groups of 1–2 videos, each under 45 min.

## Setup
```
uv sync
.venv\Scripts\activate
```

## License
- Annotations: Etalab Licence Ouverte 2.0.
- Code: MIT (see `LICENSE`).

## Use and contact
The labels are meant for designing evaluations, not for assessing or comparing interpreters.

Questions, or an interpreter who wants their labels removed or merged: henri.taing.pro@gmail.com

## Reference
Halbout, J., Fabre, D., Ouakrim, Y., Lascar, J., Braffort, A., Gouiffès, M., and Beautemps, D. (2024). Matignon-LSF: a Large Corpus of Interpreted French Sign Language. In *Proceedings of the LREC-COLING 2024 11th Workshop on the Representation and Processing of Sign Languages*, pages 95–101.