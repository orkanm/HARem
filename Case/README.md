# HARem Case V1.0

This directory contains the 3D printable files for the HARem case. The design is split into two parts: the main housing and the cover.

## Files

- **STLs (Standard Tessellation Language)**: Ready-to-slice files for 3D printing.
    - `HARem_Case_V1.0-Housing.stl`: The main body of the remote.
    - `HARem_Case_V1.0-Cover.stl`: The front cover.
- **STEP (Standard for the Exchange of Product model data)**: Editable CAD files for modification.
    - `HARem_Case_V1.0-Housing.step`
    - `HARem_Case_V1.0-Cover.step`
    - `HARem_Case_V1.0.step`: Full assembly.
- **FreeCAD Project**: Source project file for FreeCAD.
    - `HARem_Case_V1.0.FCStd`

## Printing Recommendations

These are general recommendations for printing the case on an FDM printer.

| Setting | Recommendation | Notes |
| :--- | :--- | :--- |
| **Material** | PLA / PLA+ / PETG | PLA is easier to print; PETG is more durable. |
| **Layer Height** | 0.2mm | Or smaller for finer detail. |
| **Infill** | 20% - 100% | 20% is usually sufficient for structural integrity. |
| **Supports** | Minimal / None | Check your slicer preview. The design aims to be printable without extensive supports, but some orientations might require them. |
| **Adhesion** | Skirt / Brim | Use a brim if you experience warping. |

### Assembly
The front cover is designed to snap fit or be secured with small screws depending on your specific print tolerances. You may need to lightly sand the edges for a perfect fit.
