# Magos Nodes — ComfyUI Skeleton Editor

A professional node pack by **Magos Digital Studio** for precise pose control in WanAnimate workflows.
Extract skeleton data from video, edit it frame-by-frame in a full-screen interactive editor, retarget proportions, and render clean pose images — all inside ComfyUI.

---

## What's Included

| Node | Purpose |
|---|---|
| **Magos DWP Extractor** | Detect body, hand, and face keypoints from a video batch |
| **Magos DWP Editor** | Full-screen interactive timeline editor (pop-up overlay) |
| **Magos DWP Renderer** | Render edited skeletons to image batch and ControlNet keypoints |
| **Magos Pose Retargeter** | Scale, offset, and rotate body clusters across all frames |
| **Wan Ratio & FPS** | Mod-16 compliant resolution + FPS picker for WanAnimate |
| **WanAnimate Sampler Presets** | One-click sampler configuration with user-saveable presets |

---

## Requirements

- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- [ComfyUI-WanAnimatePreprocess](https://github.com/kijai/ComfyUI-WanAnimatePreprocess) — provides the `POSEMODEL` and `POSEDATA` types used by this pack

---

## Installation

```bash
cd ComfyUI/custom_nodes
git clone https://github.com/MagosDigitalStudio/ComfyUI-Magos-Nodes
```

Restart ComfyUI. All nodes appear under **MAGOS Nodes** in the Add Node menu.

---

## Editor Highlights

- **Timeline editing** — drag joints, insert keyframes, smooth curves across any number of frames
- **Graph editor** — per-joint curve editing with Catmull-Rom / Linear / Step / Ease interpolation
- **Dope Sheet** — diamond view of all keyframes; move, copy, and trim in time
- **3D Orbit View** — drag joints horizontally to set Z-depth for 2.5D depth-sorted rendering
- **Add Hand** — synthesize a hand pose at any wrist, then move it with IK
- **Reference overlay** — load a still image, video file, or image sequence as a canvas backdrop
- **Save / Load project** — export editor state to JSON and restore it later
- **Auto Keyframe** — automatically write keyframes on every joint drag

---

## Typical Workflows

### Basic — Extract, Edit, Render

```
VHS_LoadVideo ──► Magos DWP Extractor ──► Magos DWP Editor ──► Magos DWP Renderer ──► WanAnimate
```

### With Retargeting

```
Magos DWP Editor ──► Magos Pose Retargeter ──► Magos DWP Renderer ──► WanAnimate
```

### ControlNet Pipeline

```
Magos DWP Renderer ──► pose_keypoints (POSE_KEYPOINT) ──► DWPose Preprocessor Visualizer ──► ControlNet
```

###  LTX-Video / UniAnimate

```
Magos DWP Renderer ──► pose_images (IMAGE) ──► LTX pose node
```

`pose_images` is a standard IMAGE tensor — no conversion needed for any of these pipelines.

---

## Experimental: NLF 3D Overlay

> **Status: Experimental — not ready for production use.**

The editor includes an optional NLF (Neural Localizer Fields) overlay that can display 3D SMPL joint positions as a ghosted purple skeleton alongside the standard DWPose view. This feature requires:

1. The [ComfyUI-SCAIL-Pose](https://github.com/kijai/ComfyUI-SCAIL-Pose) custom node (provides `MultipersonNLF`)
2. An NLF model file (`.safetensors`) placed in `ComfyUI/models/nlf/`
3. Connecting the `NLF Model Loader` node to the `Magos DWP Editor`

The NLF toggle appears in the editor sidebar under **⚗ Experimental**. When enabled, a blend slider lets you cross-fade between the standard DWPose skeleton and the NLF 3D overlay. Re-running the workflow caches the inference results.

This feature is included for research and experimentation. It is not required for any standard workflow.

---

## License

GNU GPL v3.0 — free to use, modify, and distribute.

---

*Magos Digital Studio*
