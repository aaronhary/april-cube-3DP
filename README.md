<div align="center">

<img src="docs/header.jpg" width="100%" alt="Printed April Cube 3DP: an assembled cube with AprilTag tiles, the cube with one tile showing its clips, and a loose tag tile">

# April Cube 3DP

**A 3D-printable AprilTag cube, built from flat-printed two-colour tag tiles.**

[![Tiles](https://img.shields.io/badge/tiles-652_ready_to_print-111111?style=flat-square)](https://github.com/aaronhary/april-cube-3DP/releases/latest)
[![Format](https://img.shields.io/badge/format-3MF_·_STEP-555555?style=flat-square)](#-downloads)
[![Families](https://img.shields.io/badge/AprilTag-36h11_·_25h9_·_16h5-555555?style=flat-square)](#-choosing-a-tag-family)
[![Onshape](https://img.shields.io/badge/Onshape-custom_feature-555555?style=flat-square)](#-onshape-custom-feature)

[**Download tiles**](https://github.com/aaronhary/april-cube-3DP/releases/latest) · [**Cube**](cube/) · [**Onshape feature**](#-onshape-custom-feature)

</div>

---

## 💡 Why this project exists

<table>
<tr>
<td width="50%" valign="top">

### 🖨️ Flat printing: only 2 colour changes

Every tag tile prints **flat**, tag face down on the bed. The tag is a single 0.2 mm layer, the very first one, so the colour changes only there: **two filament changes per tile**, plus a perfectly flat tag surface from the bed. A cube with tags on its side walls would need a colour change on almost every layer.

</td>
<td width="50%" valign="top">

### 🧩 Maximum flexibility

Tags and cube are **separate parts**:
- Put any family and any ID on any face.
- Swap a tile without reprinting the cube.
- Reuse the cube.

Need something custom? The **Onshape feature** puts any tag on any flat face, at any size.

</td>
</tr>
</table>

> [!NOTE]
> The cube and the tag tile are based on designs by **[MADOS3D](https://www.mados3d.com/)**, released under CC0. Thank you for sharing them!

---

## 📦 Downloads

The tag tiles are on the **[latest release](https://github.com/aaronhary/april-cube-3DP/releases/latest)**, one zip per family. Each zip holds ready-to-print **plates of 16 tiles**: IDs 0–15, 16–31, and so on.

| Family | IDs | Plates | Download |
|---|---|---|---|
| **tag36h11** ⭐ | 0 – 586 | 37 | [april-cube-3dp_tag36h11_tiles.zip](https://github.com/aaronhary/april-cube-3DP/releases/latest/download/april-cube-3dp_tag36h11_tiles.zip) |
| tag25h9 | 0 – 34 | 3 | [april-cube-3dp_tag25h9_tiles.zip](https://github.com/aaronhary/april-cube-3DP/releases/latest/download/april-cube-3dp_tag25h9_tiles.zip) |
| tag16h5 | 0 – 29 | 2 | [april-cube-3dp_tag16h5_tiles.zip](https://github.com/aaronhary/april-cube-3DP/releases/latest/download/april-cube-3dp_tag16h5_tiles.zip) |

Every plate is a **Bambu Studio project** (`.3mf`) set up for the **Bambu Lab A1**: 0.2 mm Standard, Bambu PLA Basic, filament 1 black and filament 2 white.
- **Layout:** the tiles sit in a 4 × 4 grid, row by row from the back left: ID 0 at the back left, ID 15 at the front right. The grid is 194 × 194 mm.
- **File names:** each plate is named after its first and last ID, e.g. `tag36h11_00016-00031.3mf`. The last plate of a family holds the remaining IDs.
- **Tiles:** each tile is one object with two parts:
  - `black`: the tag, a single 0.2 mm layer on the **underside**.
  - `white`: the tile, with the **ID number engraved** 0.2 mm into its upper side, so you can tell the tiles apart.

In this repo:

| Folder | Contents |
|---|---|
| [`cube/`](cube/) | The cube: `cube.3mf` for slicing, `cube.step` for CAD |
| [`tags/`](tags/) | The blank tile without a tag: `tile-blank.3mf` and `tile-blank.step` |

| | |
|---|---|
| Tile | 44 × 44 × 6.8 mm |
| Tag | 40 mm overall, inlaid 0.2 mm (one layer) on the underside |
| ID number | Engraved 0.2 mm into the upper side |
| Cube | 40 mm |

---

## 🖨️ Printing

1. Open a plate, e.g. `tag36h11_00000-00015.3mf`, in **Bambu Studio** (OrcaSlicer works too).
2. Check the filaments: **filament 1 black**, **filament 2 white**.
3. Print it as it is. The tags lie **face down** on the bed, so the whole plate changes colour only in its first layer.
4. Print `cube.3mf` in any colour and mount six tiles.

> [!TIP]
> - **Other printers:** switch the printer profile in Bambu Studio. The plate needs a 256 × 256 mm bed; for smaller beds, move tiles to a second plate.
> - Use a **different ID on every face**, and on every cube if you track several, so each face can be identified.
> - **Matte filament** reduces glare and improves detection.

---

## 🎯 Choosing a tag family

| Family | IDs | Cells | Tag size for your detector¹ | |
|---|---|---|---|---|
| **tag36h11** | 587 | 10 × 10 | **32.0 mm** | ⭐ Recommended: robust, supported everywhere (OpenCV, ROS, FRC) |
| tag25h9 | 35 | 9 × 9 | **31.1 mm** | Bigger cells, fewer IDs |
| tag16h5 | 30 | 8 × 8 | **30.0 mm** | Biggest cells, but many false detections |

¹ Pose estimation expects the edge length of the **black border square**, not the 40 mm overall size.

---

## 🛠️ Onshape custom feature

[`featurescript/AprilTag.fs`](featurescript/AprilTag.fs) adds an **AprilTag** feature to Onshape:
- Pick a flat face, a family and an ID.
- The feature cuts the tag into the part and fills the pocket with a separate black part.

It produces the same tag geometry as the tiles above (without the engraved number), on any part you like.

**Install:**
1. Create a Feature Studio in your Onshape document.
2. Keep the two header lines Onshape generates.
3. Paste everything after the header lines from `AprilTag.fs`.

<details>
<summary><b>Feature parameters</b></summary>

| Input | Meaning |
|---|---|
| Face or plane | Planar part face (or a plane, for a sketch only). The tag reads correctly when viewed from outside. |
| Family | `tag36h11` (default), `tag25h9`, `tag16h5`, `tagStandard41h12` |
| Tag ID | 0 … (number of tags in the family − 1) |
| Tag size | Overall edge length including the white border: 4 cm gives a 4 × 4 cm tag |
| Center point | Optional vertex or mate connector; default is the face centre |
| Align to edge / Rotation | Orientation of the tag on the face |
| Draw outer boundary | Also outlines the white border (sketch-only use) |
| Corner chamfer | 0.2 mm by default. Where black cells touch only diagonally, the white corners are cut back, so the black stays one piece and both colours are valid solids |
| Inlay / Inlay depth | On by default, 0.2 mm (one layer). Cuts the black area into the part and creates the black part. Off: sketch only |

</details>

---

## 📄 Licenses

- **Cube and tile design:** [MADOS3D](https://www.mados3d.com/), CC0.
- **AprilTag code tables:** [AprilRobotics/apriltag](https://github.com/AprilRobotics/apriltag), © The Regents of the University of Michigan, BSD 2-Clause.
