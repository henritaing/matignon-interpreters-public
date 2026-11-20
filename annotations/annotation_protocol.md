# Annotation Protocol: Interpreter Labels for Matignon-LSF

This protocol describes how the interpreter labels released in this repository were produced. It accompanies the paper *Interpreter Annotations for Matignon-LSF* (Taing, 2026).

## 1. Data sources

- **Corpus:** Matignon-LSF (Halbout et al., 2024), 67 videos of the French Council of Ministers debriefings, interpreted live into French Sign Language (LSF).
- **Video list:** the list of video IDs used in Matignon-LSF, obtained from its authors (`Matignon-LSF_video_list.csv`).
- **Download:** the videos are downloaded from YouTube with `yt-dlp`.
- **Preprocessing:** before annotation, each video is cropped to the interpreter's area with the cropping script of the Matignon-LSF repository (https://github.com/JulieLascar/Matignon-LSF).

## 2. Annotators and training

- **Annotator A** (main annotator) annotates all 67 videos.
- **Annotator B** (second annotator) is a French PhD student. Both annotators are hearing and do not sign.
- **Training:** annotator B attends a one-hour session in which he reads these guidelines and watches a demonstration of the annotation process on 10 segments.

## 3. Interpreter gallery

A first fast pass over all 67 videos identifies every distinct interpreter and builds a **frozen gallery**: one fixed ID and one fixed reference image per interpreter.

- IDs are assigned in order of first appearance during this pass, going through the videos in video ID order.
- The number of interpreters found is compared with the count reported by Halbout et al. (2024) as a sanity check; any discrepancy is examined.
- The gallery stays **private** and is **not part of this repository**, to protect the interpreters' privacy. Only arbitrary IDs (`interpreter_1` … `interpreter_15`) are released.

## 4. Segments and transitions

- A **segment** is one interpreter's continuous turn in a video.
- A **transition** is the handover between two interpreters. It starts when the interpreter leaves the frame and ends when the next interpreter is settled in the middle of the frame with the face visible.
- Annotator A fixes all segment boundaries and transition windows for the 67 videos in a first pass, and labels them in a second pass. The boundaries are then frozen, so that both annotators label exactly the same segments.
- Boundaries are placed with a precision of about **±1 s per boundary**.
- **Consistency check:** `blank.py`, an automatic blank-frame detector (per-video adaptive thresholds with hysteresis; the first 90 s of each video are skipped), was run on the videos to check whether any transition had been missed. The detector has not been evaluated and is not used to produce the labels.

## 5. Annotation schema

Annotations are entered in one Google Sheet and exported as `Annotation_results.csv`, with these columns:

| Column | Content |
|---|---|
| `VideoID` | YouTube video ID, as listed in `Matignon-LSF_video_list.csv` (Halbout et al., 2024) |
| `SegmentIndex` | Position of the segment in the video |
| `Segment Start`, `Segment End` | `HH:MM:SS.mmm` |
| `Transition Start`, `Transition End` | `HH:MM:SS.mmm` |
| `Interpreter_#_A` | Interpreter ID assigned by annotator A |
| `Uncertainty_Flag_A` | `Y` / `N` |
| `Interpreter_#_B` | Interpreter ID assigned by annotator B (filled only for the agreement sample) |
| `Uncertainty_Flag_B` | `Y` / `N` (agreement sample only) |
| `Comments` | Free text |

Using the frozen gallery and the frozen segments, each annotator fills only their own columns.

**Edge cases:**
- **Unsure which interpreter it is:** the annotator writes `0` in their own column and raises their uncertainty flag.
- **Face not in the gallery:** the annotator writes `New_#` in their own column and raises their uncertainty flag. These cases are set aside during annotation and adjudicated after the full pass.

## 6. Quality control

- **Agreement sample:** 48 segments (about 20% of the segments), drawn at random with a fixed seed, are annotated by both annotators with the same frozen gallery. Because annotator B was trained by annotator A and uses the same gallery, the two annotations are not fully independent.
- **Inter-annotator agreement:** measured with Cohen's kappa on the segments where both annotators committed to an interpreter ID. Raw agreement is reported with an exact (Clopper–Pearson) 95% confidence interval, which is the more informative figure when agreement is perfect.
- **Conflict resolution:** disagreements and uncertain segments are resolved by adjudication. If no consensus is reached, the segment is excluded, and the number and identity of excluded segments are reported.

## 7. Outcome

- 67 videos, **242 segments**, **175 transitions**, **15 interpreters** (matching Halbout et al., 2024).
- Annotator A raised no uncertainty flag and used no `New_#` label.
- Agreement sample: 1 segment of 48 flagged as uncertain (2.08%), resolved by consensus. On the other 47: raw agreement 47/47 = 100% (95% CI [92.5%, 100%]), Cohen's κ = 1.00. No segment was excluded.

## 8. Known limitations

- A single main annotator for the 67 videos; both annotators are hearing and do not sign.
- The agreement sample does not cover 6 of the 11 minority interpreters, so the reported agreement does not apply to the rarest labels.
- Segment boundaries have not been independently validated.
- Segments are time intervals: anyone joining them with the audio-aligned subtitles may run into the handover problem.

## 9. Use and removal requests

The labels are intended for designing evaluations (e.g. signer-independent splits) and must not be used to assess or compare individual interpreters' performance. Any interpreter can ask to have their labels removed or merged by writing to henri.taing.pro@gmail.com.

## Reference

Halbout, J., Fabre, D., Ouakrim, Y., Lascar, J., Braffort, A., Gouiffès, M., and Beautemps, D. (2024). Matignon-LSF: a Large Corpus of Interpreted French Sign Language. In *Proceedings of the LREC-COLING 2024 11th Workshop on the Representation and Processing of Sign Languages: Evaluation of Sign Language Resources*, pages 95–101, Torino, Italy. ELRA and ICCL.