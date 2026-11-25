# April Cube

A 3D-printable [AprilTag](https://april.eecs.umich.edu/software/apriltag) cube for camera-based pose tracking. A 40 mm cube skeleton carries six flat tag tiles, one per face. Each tile is a 42.4 × 42.4 × 6.8 mm plate with the tag inlaid 0.4 mm deep in two colours.

This repo contains every tag of the three classic AprilTag families as ready-to-print two-colour tiles (652 files), the cube skeleton, and an Onshape custom feature to make your own.

## Why this project exists

> **1. Flat printing: only 2 colour changes per tile.**
> Every tag tile prints flat, with the tag in the print plane, so the colour changes only in the thin inlay layers. With 0.2 mm layers the 0.4 mm inlay is two layers: about **two filament changes per tile**. A cube with tags on its side walls would need a colour change on almost every layer.
>
> **2. Maximum flexibility.**
> Tags and cube are separate parts. Pick any family and any ID for each face, swap a tile without reprinting the cube, and reuse the skeleton. If the ready-made tiles don't fit your needs, the Onshape feature puts any tag on any flat face, at any size.

## What's in the repo

```
tags/
  tag36h11/tag36h11_00000.3mf … tag36h11_00586.3mf   587 tiles
  tag25h9/ tag25h9_00000.3mf  … tag25h9_00034.3mf     35 tiles
  tag16h5/ tag16h5_00000.3mf  … tag16h5_00029.3mf     30 tiles
skeleton/
  april-cube-skeleton.3mf      cube skeleton, ready to slice
  april-cube-skeleton.step     cube skeleton for CAD
featurescript/AprilTag.fs      Onshape custom feature (source)
```

Every tag file is **one object made of two parts**, `white` (the tile with the pocket) and `black` (the inlay). Your slicer loads it as one object; assign a filament to each part. The file names use the tag family and the ID, zero-padded to five digits like the official [apriltag-imgs](https://github.com/AprilRobotics/apriltag-imgs).

## Printing

1. Open a tag file, e.g. `tags/tag36h11/tag36h11_00000.3mf`, in Bambu Studio, OrcaSlicer, PrusaSlicer or Cura.
2. Assign white (or any light colour) to the `white` part and black to `black`.
3. Print it as it lies, tag facing up, at 0.2 mm layer height.
4. Print `skeleton/april-cube-skeleton.3mf` in any colour and mount six tiles.

Tips:
- **Unique IDs:** use a different ID on every face, and on every cube if you track several, so the detector can tell faces apart.
- **Matte filament** reduces glare and improves detection.

## Choosing a tag family

| Family | IDs | Cells (overall) | Notes |
|---|---|---|---|
| **tag36h11** | 0–586 | 10 × 10 | Recommended: robust, widely supported (OpenCV, ROS, FRC) |
| tag25h9 | 0–34 | 9 × 9 | Bigger cells, fewer IDs |
| tag16h5 | 0–29 | 8 × 8 | Biggest cells, but many false detections |

### Tag size for your detector

The tags are 40 mm overall, including the white border. Pose estimation (OpenCV, ROS apriltag, …) expects the edge length of the black border square:

| Family | Detector tag size |
|---|---|
| tag36h11 | **32.0 mm** |
| tag25h9 | **31.1 mm** |
| tag16h5 | **30.0 mm** |

## Onshape custom feature

[`featurescript/AprilTag.fs`](featurescript/AprilTag.fs) adds an **AprilTag** feature to Onshape. Pick a flat face, a family and an ID, and the feature does the rest:
- It cuts the tag 0.4 mm into the part.
- It fills the pocket with a separate black part.

This is the same geometry as the tiles in this repo.

| Input | Meaning |
|---|---|
| Face or plane | Planar part face (or a plane, for a sketch only). The tag reads correctly when viewed from outside. |
| Family | `tag36h11` (default), `tag25h9`, `tag16h5`, `tagStandard41h12` |
| Tag ID | 0 … (number of tags in the family − 1) |
| Tag size | Overall edge length including the white border: 4 cm gives a 4 × 4 cm tag |
| Center point | Optional vertex or mate connector; default is the face centre |
| Align to edge / Rotation | Orientation of the tag on the face |
| Draw outer boundary | Also outlines the white border, for sketch-only use |
| Corner chamfer | 0.2 mm by default. Where black cells touch only diagonally, the white corners are cut back, so the black stays one solid piece and both colours are valid solids |
| Inlay / Inlay depth | On by default, 0.4 mm. Cuts the black area into the part and creates the black part. Off: sketch only |

**Installing it:**
1. Create a Feature Studio in your Onshape document.
2. Keep the two header lines Onshape generates.
3. Paste everything after the header lines from `AprilTag.fs`.

The feature then appears in the toolbar of that document.

## Credits and licenses

- **Cube skeleton and tile:** based on the designs by **[MADOS3D](https://www.mados3d.com/)**, released under CC0. Thank you!
- **AprilTag code tables:** from [AprilRobotics/apriltag](https://github.com/AprilRobotics/apriltag), © The Regents of the University of Michigan, BSD 2-Clause.

<details>
<summary>Maintainer notes</summary>

- [`tools/generate_families.py`](tools/generate_families.py) regenerates the tag tables in `AprilTag.fs` from the official `tagXXX.c` sources.
- [`tools/verify_tags.py`](tools/verify_tags.py) checks the tag rendering against the official tag images.
- [`tools/onshape_push.py`](tools/onshape_push.py) uploads the feature and its [icon](icon/AprilTag.svg) to a Feature Studio via the Onshape API, with keys in `.env` (see `.env.example`).
- [`tools/make_icon.py`](tools/make_icon.py) builds the feature icon.

</details>
