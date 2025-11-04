Giraffe.js — OpenForum Canvas Graphics Library
==============================================

Use this context when building tooling, code, or documentation around the Giraffe graphics engine that powers OpenForum visualisations. The goal is to automate generation of Giraffe-based models and extend the available graphics primitives safely.

Key Resources
-------------
- Runtime bundle: `/web/content/default/OpenForum/Giraffe/giraffe.js` (generated, do **not** edit directly).
- Source modules: `/web/content/default/OpenForum/Giraffe/Core/` — one file per primitive or subsystem (canvas, objects, animation, interaction, utilities).
- Build recipe: `/web/content/default/OpenForum/Giraffe/Core/script.build.json` — consumed by the OpenForum JavaScript Builder to regenerate `giraffe.js` and bump version metadata.
- Documentation hub: `/web/content/default/AIOpenForumDocumentation/GiraffeDocumentation` — combined human + AI guidance for usage and extension.
- JSON resources: `/web/content/default/AIOpenForumDocumentation/GiraffeDocumentation#json-scenes` (workflow guide) and `/web/content/default/AIOpenForumDocumentation/GiraffeDocumentation/Schema` (schema file) for scene import/export tooling.
- Example implementation: `/web/content/default/HomeLab/ObjectModelView` shows draggable entity cards and relationship arrows built with composites and TextBlock (call `setWrapWidth()` + `setAlignment()` so centering is honoured).
- Quick reference page: `/web/content/default/OpenForum/Giraffe/QuickReference` — constructor signatures for primitives.

Core Concepts
-------------
- `Canvas` wraps an HTML `<canvas>` element, stores `graphicsObjects`, and exposes helpers such as `add`, `remove`, `repaint`, `setScale`, `stretchToFitWindow`, `serialize`, and `deserialize`.
- All drawables inherit from `Giraffe.GraphicsObject`, gaining translation, rotation, scaling, visibility, event stubs, and the `animate(frame)` hook. Draw implementations render relative to `(0,0)`; transforms are applied automatically.
- Built-in primitives include `Line`, `Rectangle`, `RoundedRectangle`, `Circle`, `Arc`, `Text`, `Picture`, `Polygon`, `PixelCanvas`, and `Composite`. Each sets `this.type` for serialization, provides an `isInside` hit-test, and leverages shared mutators (`setColor`, `setFillColor`, `setShadow`, `setLineStyle`, etc.).
- Interaction is handled through `Giraffe.Interactive.setInteractive(canvas)` and optional drag helpers (`canvas.dragAndDrop`, `canvas.makeDraggable(object)`, `object.onCatch`). Tablet orientation support is exposed via `Giraffe.Tablet.convertRotationToMouse(canvas)`.
- Animation is opt-in via `Giraffe.setAnimated(canvas)`; register frame listeners or implement `object.animate(frame)`. Prebuilt transitions such as `Giraffe.FlipInX`, `Giraffe.MoveSequence`, and `Giraffe.ExplodeSequence` extend `Giraffe.Transition`.

Workflow for Code Generation / Extension
----------------------------------------
1. **Create or modify source files** under `/OpenForum/Giraffe/Core/`. Use prototype-based constructors (`Giraffe.YourType = function(...) { ... }`) and set `Giraffe.YourType.prototype = new Giraffe.GraphicsObject();` to inherit behaviour.
2. **Register new modules** inside `script.build.json` (append step) so the builder includes them when producing `giraffe.js`.
3. **Implement serialization hooks** (`toJson`, `fromJson`) and `isInside(x,y)` for any new graphics object so drag/drop, persistence, and hit-testing remain functional.
4. **Run the JavaScript Builder** (`/OpenForum/Javascript/Builder`) targeting `script.build.json`. Confirm version/build metadata updates in the generated bundle.
5. **Update documentation assets**: extend `/OpenForum/Giraffe/QuickReference` with constructor signatures and mention the changes on `/AIOpenForumDocumentation/GiraffeDocumentation`.
6. **Smoke test** by loading `/OpenForum/Giraffe` (and relevant demos like `/OpenForum/Giraffe/SplineEditor`) to ensure animation, interaction, and serialization still work.

Automation Tips
---------------
- When generating scenes or models, prefer `Giraffe.quickStart(canvasId)` to bootstrap animation + interaction in one call. For custom setup, instantiate `new Canvas("canvasId")`, then call `Giraffe.Interactive.setInteractive(canvas)` and `Giraffe.setAnimated(canvas)` manually.
- Use composites to build hierarchical structures. Each child’s coordinates are relative to its parent, and serialization will recurse automatically if `parts` is populated.
- Reuse utility factories: `Giraffe.RadialColor`, `Giraffe.GradientColor`, `Giraffe.Shadow`, and `Giraffe.LineStyle` instead of hard-coding canvas API calls.
- Validate geometry before adding to the canvas. For polygons, call `addPoint(dx, dy)` and set flags such as `closed` or `smooth` to control rendering.
- When scripting AI-driven modifications, preserve backwards compatibility (avoid mutating shared prototypes directly; extend via new classes or helper functions).

Testing Checklist for AI-Generated Changes
------------------------------------------
- Animate: ensure `object.animate(frame)` runs without errors at configured FPS (`canvas.startAnimation(fps, frames, looped)`).
- Interaction: verify `isInside` returns true for expected hit areas and that drag events update coordinates correctly.
- Serialization: call `canvas.save(pageName, fileName)` / `canvas.load(pageName, fileName)` to confirm new properties survive round trips.
- Performance: large scenes should repaint smoothly; avoid per-frame allocations inside `draw`/`animate` functions.
- Documentation: regenerate or adjust quick reference and server docs to align with code changes.

Use this prompt as a launchpad for automation tasks that create new Giraffe sheets, extend primitives, or refactor existing graphics workflows inside OpenForum.
