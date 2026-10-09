# Pencil Trace

Upload a photo of a person and watch their outline get sketched in pencil, stroke by stroke, then save the drawing as a video.

Everything runs in the browser. The photo is never uploaded.

## How it works

1. **Find the person.** MediaPipe Selfie Segmentation (WebAssembly, bundled in `mp/`) marks the person; a guided filter snaps that mask to the real edges of the photo, and the background is dropped.
2. **Make the sketch.** The photo is converted to greyscale and divided by a blurred copy of itself (the classic colour-dodge pencil effect), which leaves the lines and soft gradients. Optional shading puts the dark areas back, and paper grain is added.
3. **Plan the pencil's route.** The silhouette is traced first, then the inner contours (Canny edges traced into strokes), then hatching passes for anything left.
4. **Draw it.** Each frame shows the finished sketch through a mask of the strokes made so far, so the animation always ends on the exact sketch.
5. **Record it.** The canvas is captured with `MediaRecorder` to MP4 (or WebM where MP4 is not supported).

## Run locally

It is a static site with no build step. Serve the folder over HTTP (the model files cannot load from `file://`):

```
npx serve .
# or
python3 -m http.server 8000
```

## Files

- `index.html` - the whole app (markup, styles and script).
- `mp/` - the person-segmentation runtime and model weights from `@mediapipe/selfie_segmentation@0.1.1675465747` (Apache-2.0). The loader scripts come from jsDelivr at the same version.

## Controls

- **Draw:** person only, or the whole photo.
- **Shading:** outline only, light or deep.
- **Darkness:** pencil grade, H to 6B.
- **Fine detail:** how much small texture is kept.
- **Drawn with:** graphite, charcoal, sanguine or ballpoint.
- **Drawing time:** 4 to 60 seconds.
