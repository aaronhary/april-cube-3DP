# april-cube-3DP

Onshape custom feature that sketches an [AprilTag](https://april.eecs.umich.edu/software/apriltag) on any planar face or plane, ready to emboss/deboss or split for multi-colour 3D printing.

## Feature: `AprilTag`

| Input | Meaning |
|---|---|
| Face or plane | Planar face or construction plane. The tag reads correctly when viewed from outside the face. |
| Family | `tag36h11` (default), `tag25h9`, `tag16h5`, `tagStandard41h12` |
| Tag ID | 0 … (codes in family − 1) |
| Tag size | Overall edge length of the whole tag, including the white border: 4 cm gives a 4 × 4 cm tag. Note that detectors such as OpenCV and ROS expect the *black-border* size for pose estimation; see below. |
| Center point | Optional vertex / mate connector; default is the face centroid |
| Align to edge | Optional linear edge defining the tag's x-axis |
| Rotation | Extra rotation about the face normal |
| Draw outer boundary | Adds the full-tag square (incl. white quiet zone) so white areas become regions too |

Every sketch region is a single colour, but white islands enclosed by black also become regions. When extruding, select only the black regions; selecting the whole sketch fills the white holes. With the outer boundary turned on, the white quiet zone becomes a region as well, which helps for multi-colour printing.

### Detector tag size

Pose estimation needs the edge length of the black border square, not the overall size:

| Family | Grid (overall) | Black border | Detector tag size |
|---|---|---|---|
| tag36h11 | 10 × 10 | 8 | overall × 8/10 |
| tag25h9 | 9 × 9 | 7 | overall × 7/9 |
| tag16h5 | 8 × 8 | 6 | overall × 6/8 |
| tagStandard41h12 | 9 × 9 | 5 | overall × 5/9 |

Example: a 4 cm tag36h11 has a detector tag size of 3.2 cm.

## Files

- [featurescript/AprilTag.fs](featurescript/AprilTag.fs): the Feature Studio source
- [tools/generate_families.py](tools/generate_families.py): regenerates the code tables from the official `tagXXX.c` sources
- [tools/verify_tags.py](tools/verify_tags.py): checks the rendering and sketch logic against the official tag PNGs
- [tools/onshape_push.py](tools/onshape_push.py): uploads the feature to Onshape through the REST API

## Getting it into Onshape

**Manually:** create a Feature Studio in your document, keep the two header lines Onshape generates, and paste everything after them from `AprilTag.fs`.

**With the API:**
1. Create API keys at <https://dev-portal.onshape.com/keys>.
2. `cp .env.example .env` and fill in the keys plus the Feature Studio URL.
3. Run `python tools/onshape_push.py`.

## Regenerating / verifying the tag data

```sh
# sources: https://github.com/AprilRobotics/apriltag  (tag36h11.c, tag25h9.c, tag16h5.c, tagStandard41h12.c)
python tools/generate_families.py tag_src/
# images:  https://github.com/AprilRobotics/apriltag-imgs  saved as <family>__<id>.png
python tools/verify_tags.py tag_imgs/
```

Tag data © The Regents of the University of Michigan, BSD 2-Clause.
