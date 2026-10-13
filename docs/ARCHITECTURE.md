# DentCAD AI — Architecture & MVP acceptance criteria

## Product target
Reduce dental technician time for routine single-unit posterior crown design. Early software is for technical evaluation only and is not clinically validated.

## Current runnable path
- React + Three.js browser workspace.
- FastAPI mesh ingestion for STL/PLY/OBJ.
- Trimesh unit conversion and structural diagnostics.
- Explicitly-labelled synthetic demo case.
- Baseline parametric shell proposal endpoint and STL download.
- No model inference is claimed to be implemented yet.

## Architecture boundaries
1. **Web CAD UI:** file selection, 3D inspection, layer visibility, margin interaction, preview/export.
2. **API/orchestrator:** validate inputs, manage processing jobs, record explicit generation mode and warnings.
3. **Mesh core:** parse, validate, normalize units, compute topology/measurements.
4. **Segmentation adapter:** model interface, isolated from product logic. Evaluate model weights, licences, input contracts, test data and inference reproducibility before enabling.
5. **Margin component:** manual editor first; automatic margin proposal is separate and confidence-visible.
6. **Crown proposal provider:** template retrieval / deterministic adaptation first, optional AI generator behind interface.
7. **Quality diagnostics:** watertightness, winding, self-intersection, thickness and contact analysis. These are engineering checks, not clinical approval.
8. **Artifacts:** durable object storage and provenance/manifest are required before public hosting.

## MVP acceptance criteria (technical)
- A real STL/PLY/OBJ scan can be uploaded with explicit units.
- The exact uploaded geometry renders interactively without silently losing scale.
- Inputs are validated and malformed/oversized meshes fail cleanly.
- A technician can select/define a preparation region and manually correct margin.
- Crown proposal provider is explicitly labelled by generation mode.
- STL output can be downloaded and reopened in an independent mesh viewer.
- API and UI tests cover normal flow and invalid inputs.
- Inference reports model name/version/checkpoint hash and does not silently fall back to demo geometry.
- Tests include representative real scans obtained with permission and correct data rights.
- Dental professional assesses generated results before any clinical or manufacturing claim.

## Near-term technical backlog
1. Replace screen-plane margin points with raycast-to-mesh picking; store XYZ in mesh coordinates.
2. Improve UI scan orientation and add camera presets / fit-to-scene control.
3. Ensure uploads for different roles can be independently selected and all case objects persist.
4. Add structured mesh preview transport for large meshes (decimation for viewer only; never modify the output-of-record geometry).
5. Evaluate MeshSegNet on representative scan dataset; audit weights/dataset/licensing.
6. Implement a true boundary-conforming intaglio and anatomy-aware crown provider; current ellipsoidal shell is only an integration fixture.
7. Add asynchronous worker, per-case persistent metadata and object storage.
8. Add auth, access control, encryption, retention controls and audit trail before accepting any identifiable patient data.

## Language strategy
- Python + PyTorch for research model inference and initial integration.
- Python/Trimesh for first mesh pipeline.
- Rust only for measured bottlenecks or safety-critical low-level processing after profiling; keep a stable geometry-service contract.
- TypeScript/React/Three.js for browser CAD UI.

## License and third-party policy
Initial app source under Apache-2.0. Do not bundle third-party code, model weights, datasets or tooth libraries until each has an explicit commercially suitable licence and provenance record. Keep GPL-derived code outside proprietary distribution until counsel reviews implications.
