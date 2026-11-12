# Memory Matter

A mobile-first creative technology experiment: touch a Gaussian-Splat memory, dissolve spatial fragments, leave colored light trails, and restore the scene.

**[Open the demo](https://dom2500.github.io/memory-matter/)**

## Try it

1. Open the demo in a current Safari or Chrome with WebGL 2 enabled. The example scan downloads automatically.
2. In **Auflösen**, touch and drag across the scan. Up to 24 spherical regions are removed nondestructively; the oldest region is restored when the limit is reached.
3. Use **Erkunden** to orbit; pinch to zoom and use two fingers to pan.
4. Adjust **Radius**, tap the color wheel for three palettes, or **Reset** to restore the scene.
5. **Deine Erinnerung → Scan importieren** accepts Gaussian-Splat `.spz` and `.ply` files, up to 100 MB. Regular polygon PLY files are not supported. Files are read locally; the app does not upload them.
6. **Bild ↓** exports the rendered canvas as PNG. The interface is excluded.

## Run locally

No build step:

```sh
python3 -m http.server 8000
```

Open `http://localhost:8000`. Internet is required for the pinned rendering libraries and the example scan. Serve over HTTPS for remote mobile access; the app does not request camera or microphone permissions.

## Implementation

- Three.js **0.180.0**, Spark **2.4.0**, loaded from jsDelivr through an import map.
- Genuine Gaussian Splatting through Spark's WebGL 2 renderer.
- Raycasting locates each brush center. Scene-level spherical `SplatEditSdf` masks remove splats on the GPU. Reset shrinks the masks; the source scan remains unchanged.
- Colored additive rings and particles visualize the edits. These light trails are generated graphics, not reconstructed interior geometry.
- Pixel ratio capped at 1.5; 24 edits maximum; brush sampling throttled. Imports limited by file size, not decoded GPU memory. Large or dense scans can still exceed mobile memory.
- Imported scans are centered and scaled automatically. **Scan umdrehen** helps with differing capture orientations.
- Reduced motion disables decorative particle motion and makes restoration immediate.

## Sources and assets

- [Spark](https://github.com/sparkjsdev/spark), MIT; [Splat editing documentation](https://sparkjs.dev/docs/splat-editing/).
- [Three.js](https://github.com/mrdoob/three.js), MIT.
- [Painted Bedroom sample](https://storage.googleapis.com/forge-dev-public/painted_bedroom.spz), remotely loaded from the asset URL used by [Spark's interactive holes example](https://github.com/sparkjsdev/spark/tree/main/examples/interactive-holes). The sample is not bundled; third-party asset rights remain with their respective owners. Substitute a scan you own for commercial deployments.

## Scope

This is an interactive prototype, not a scan-capture app. It does not generate new 3D geometry behind removed surfaces, save edited scans, or perform generative AI reconstruction. Smartphone performance depends on the GPU, browser, and scan size. Real iPhone and Android hardware validation remains necessary.
