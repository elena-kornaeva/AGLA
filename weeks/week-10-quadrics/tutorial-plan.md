# Tutorial 10 implementation plan: Straight Structures, Focused Light

Planning date: 2026-11-15. Batch 6 local verification passed for both notebooks. All code cells ran in separate Python processes, including a second pass through the exercise cells with completed inputs. Ten static figures were inspected. Browser interaction, direct Colab execution, and publication remain pending.

This plan is in English. The student-facing and teacher notebooks will be in Russian. The target is a 90-minute first-year practical on quadric surfaces, canonical equations, surfaces of revolution, and plane sections. Week numbers follow the repository's editorial navigation, not an independently verified official calendar.

## 1. Decisions and boundaries

The chapter contains two connected investigations:

1. **A curved structure made from straight members:** design a hyperboloidal frame from specified dimensions, then explain why its members can be straight.
2. **A mirror that will not focus:** optimize a detector position, discover the limitation of a spherical reflector, and repair the geometry using a paraboloid.

The connecting question is: **How does a surface equation determine whether a design works?**

Students submit calculations, predictions, and explanations. The notebook supplies a small virtual laboratory, not a long programming assignment. All scenes and data are synthetic educational constructions. These are genuine geometric design questions, but neither a structural certification nor a full optical instrument design.

Do not add robots, pendulum dynamics, fluid mechanics, full telescope assemblies, neural-network training, or a general-purpose ray tracer. Do not make a new framework or website. Do not implement until the user starts an implementation batch.

Lecture alignment:

- Define a quadric as a zero set of a real polynomial of total degree two, including degenerate cases.
- Read canonical equations and infer sections before opening a plot.
- Investigate a one-sheet hyperboloid, sphere, and circular paraboloid deeply.
- Use concise comparison tasks for ellipsoids, two-sheet hyperboloids, cones, cylinders, and hyperbolic paraboloids.
- Explicitly distinguish a full mathematical surface from its bounded physical patch.
- Avoid eigenvalue classification and diagonalization: those belong to the subsequent quadratic-forms material in this repository.

## 2. Final deliverables and paths

Create during implementation, not as empty placeholders:

- `notebooks/straight-structures-focused-light.ipynb`: student interactive chapter.
- `notebooks/straight-structures-focused-light-teacher.ipynb`: same chapter with solutions, expected results, teaching notes, common errors, and mathematical verification.
- `notebooks/straight-structures-focused-light.version.json`: small release manifest for the optional notebook-update check, created when a release is prepared.
- Update this week's `README.md`, the root `README.md`, and the appropriate entry in `docs/course-map.md` when real notebooks exist.

Keep the notebooks self-contained: no imports from repository-local helper modules, no Drive mounts, no credentials, no external data downloads, and no GPU runtime. A downloaded single notebook must run.

Do not mark the week ready merely because a file exists. Report local execution, visual QA, and direct Colab verification separately.

## 3. Reading and interaction design

Working title in Russian: **«Кривое из прямого, свет из уравнений»**.

Opening question: **«Как выбрать поверхность, чтобы конструкция выполняла свою задачу?»**

Write a book chapter in connected prose. Each scene follows:

1. A concrete situation and one question.
2. A prediction or calculation space before the result.
3. A small experiment or short visible computation.
4. A hint, available separately from the answer.
5. An explicit self-check and a sentence explaining the result.

Prefer 100–220 words of prose per explanatory passage, separated by purposeful diagrams and questions. Do not force a rigid word quota. Avoid walls of equations or a succession of unexplained controls.

Visible student code should generally be 2–8 lines: parameter values, an expression, or a call to the laboratory. Two small computational exercises suffice: checking membership of points on a member and calculating the detector error from supplied hit coordinates. Mathematical answers also belong in editable Markdown cells.

Use `None` or equivalent explicit unfinished values in answer cells. An unanswered or malformed exercise must produce helpful feedback, not stop Run all. Never prefill correct answers in open exercises.

The student notebook may contain optional, explicitly labelled explanation/reveal blocks after an attempt. Do not treat collapsed content as secure answer storage. Keep the full answer key and numerical reference suite in the teacher notebook.

Work in pairs. Alternate the proposer and checker roles between the two stories. Finish with an individual response. Do not add leaderboards: comparisons should reward justified decisions, not slider speed.

## 4. Ninety-minute route

| Time | Scene | Required student action | Evidence of completion |
| --- | --- | --- | --- |
| 0–5 | Prologue and launch | Predict whether straight members can form a curved surface; run setup | Prediction and successful environment check |
| 5–15 | T1: Select and size a surface | Use sections to choose a quadric and calculate its parameters | Equation and three section radii |
| 15–25 | T2: Construct the frame | Calculate ring offset and member length; verify membership | A calculated frame and a numerical/algebraic check |
| 25–30 | T3: Challenge the construction | Explain the zero-offset and half-turn limits | Cylinder/cone explanation |
| 30–40 | O1: Two similar mirrors | Compare sections, predict focusing, check one reflection | One ray calculation and a revised prediction |
| 40–55 | O2: Optimize the detector | Define the error, bracket/refine its minimum, validate it | Detector position, RMS spot size, and error plot |
| 55–70 | O3: Repair the surface | Calculate a paraboloid from aperture and focal distance | Equation, depth, section, and focusing check |
| 70–80 | O4: Test the explanation | Change aperture and compare the optimized outcomes | A justified quality-versus-collected-area tradeoff |
| 80–90 | Epilogue | Solve section/classification questions and write a conclusion | Individual exit ticket |

This is a teaching budget, not a claim that every class will finish at exactly 90 minutes. Supply staged hints and a printed/static route. Full symbolic proofs and extensions are not mandatory in addition to this schedule.

If time slips, reduce repeated parameter trials and use a supplied coarse error plot; preserve the section reasoning and the final explanation. Fast pairs work on the challenge boxes rather than waiting.

## 5. Opening and setup cells

Use stable cell IDs so the two versions can be kept aligned.

1. `front-title`: title, question, duration, outcomes, source/release identity, and eventually a verified Open in Colab link.
2. `front-predictions`: two prediction questions, with empty response space.
3. `front-howto`: run setup once, then read and work in order; distinguish experiment inputs from exercise answers.
4. `setup-environment`: collapsed imports, installed-version diagnostics, renderer choice, constants, optional seed.
5. `setup-geometry`: collapsed computational helpers for surfaces, sections, endpoints, reflection, and detector intersections.
6. `setup-display`: collapsed Plotly and Matplotlib scene functions and controls.
7. `setup-feedback`: collapsed exercise input validation and lightweight self-check helpers, without an exposed answer dump.
8. `setup-status`: short visible status and a static sanity figure; no long package/version log in the chapter flow.
9. `setup-update`: optional version-check control, disabled by default, with concise status.
10. `tools-card`: definition of a quadric, substitution for a section, line-segment parameterization, and units. Reflection gets its own short card at the start of Story 2.

Auxiliary code goes here, before the story. Keep the mathematics next to the question it explains. Use collapsed Colab form/code metadata where supported; the notebook must remain understandable in Jupyter or a static viewer if collapse metadata is ignored.

## 6. Story 1: A curved frame made from straight members

### T1. Choose the surface from requirements

Student brief: two horizontal rings of radius R = 2 m, separated by H = 4 m, with a waist radius a = 1 m halfway between them. Find a smooth connected rotational surface patch with these dimensions.

Place the waist at z = 0 and the rings at z = ±H/2. Offer canonical-equation cards, initially without revealing the full surfaces. Students eliminate candidates by section behavior rather than their names alone.

Provide the candidate form, but leave its parameters to students:

\[
\frac{x^2+y^2}{a^2}-\frac{z^2}{c^2}=1,
\qquad -H/2\le z\le H/2.
\]

Tasks:

- Derive the radius r(z) of a horizontal section.
- Calculate c from the ring and waist dimensions.
- Predict the sections z = 0, z = 1 m, and z = 2 m.
- Explain why an ellipsoid or a two-sheet hyperboloid does not satisfy these requirements in the proposed orientation.
- Identify the hyperbola in an axial plane and use its rotation to generate the surface.

Visualization: linked 3D patch, axial section, and horizontal section. A rotation-angle control reveals the sweep of the generating hyperbola. Distinguish rotating a generating curve from rotating the camera. The section radius is annotated with units.

Hint ladder: substitute z = h; express x² + y² as r²; enforce the top-ring condition.

Teacher reference, not prefilled student content:

\[
r(z)=a\sqrt{1+z^2/c^2},\qquad
c=\frac{aH}{2\sqrt{R^2-a^2}}=\frac{2}{\sqrt3}\ \mathrm m.
\]

The section radii are 1, sqrt(7)/2, and 2 m. Require 0 < a < R and H > 0 for this finite-parameter hyperboloid design.

### T2. Build the same surface from straight segments

Use endpoints with angular offset δ:

\[
A(\theta)=\begin{pmatrix}R\cos\theta\\R\sin\theta\\-H/2\end{pmatrix},\qquad
B(\theta)=\begin{pmatrix}R\cos(\theta+\delta)\\R\sin(\theta+\delta)\\H/2\end{pmatrix}.
\]

The member is p(t) = (1 − t)A + tB, 0 ≤ t ≤ 1. Restrict the design offset to 0 ≤ δ ≤ π and explain the radian convention in computation.

Required tasks:

- Use the midpoint to determine the waist radius as a function of δ.
- Calculate δ for the specified design and calculate member length.
- Test several interior points against the implicit surface equation.
- Explain why checking only endpoints would not prove membership of the whole member.

For the main route, provide the intermediate identity below as a hint after the numerical investigation. Full derivation is a fast-pair challenge:

\[
r(t)^2=R^2\left[1-4t(1-t)\sin^2(\delta/2)\right].
\]

Teacher reference:

\[
a=R\cos(\delta/2),\qquad
\delta=2\arccos(a/R)=120^\circ,
\]
\[
\ell=\sqrt{H^2+4R^2\sin^2(\delta/2)}
=\sqrt{28}\ \mathrm m\approx5.292\ \mathrm m.
\]

Using t = z/H + 1/2 gives

\[
r(z)^2=R^2\cos^2(\delta/2)
+\frac{4R^2\sin^2(\delta/2)}{H^2}z^2,
\]

which identifies the same hyperboloid. A finite set of members samples its ruled structure; it is not a continuous wall.

Visualization: toggles for members, translucent surface, highlighted member, and sections. Highlight one straight member and show its endpoints and midpoint. Changing δ represents a different design: member lengths change. Do not animate it as a rigid mechanism with fixed-length bars.

### T3. Limits and a second family of lines

Required: explain δ = 0 (cylinder) and δ = π (a double-cone patch meeting at its vertex). Compute these from the endpoint/radius formula rather than dividing by a vanishing hyperboloid parameter.

Optional: show offsets +δ and −δ as two families of straight generators. Ask whether every curved surface can contain straight lines. Use the sphere as a counterexample; the saddle appears in the exit ticket.

## 7. Story 2: The mirror that will not focus

### Fixed geometric and optical assumptions

Use a right-handed orthonormal frame, vertical optical axis z, and metres throughout. Mirrors open upward and have vertex (0, 0, 0). Incoming unit direction is d = (0, 0, −1). The detector is the plane z = s above the mirror.

Model ideal specular reflection in geometric optics. Ignore diffraction, absorption, finite source size, mounting obstructions, detector obstruction of the incoming beam, surface roughness, and multiple reflections. The detector is a mathematical observation plane. State these assumptions in a short readable paragraph, not an intimidating checklist.

Baseline comparison:

- Sphere curvature radius R_s = 4 m.
- Circular aperture radius ρ = 1.5 m.
- Paraboloid focal distance f = 2 m = R_s/2.
- Sphere: x² + y² + (z − R_s)² = R_s², lower cap only.
- Paraboloid: x² + y² = 4fz.
- Both patches satisfy x² + y² ≤ ρ².

Thus the mirrors share vertex, axis, aperture, and vertex curvature; their depths are close but their exact profiles differ. Avoid claiming that they have equal depth or equal surface area.

### O1. Compare profiles and check one reflection

Show the axial profiles first, without rays. Ask students whether the mirrors will focus identically and whether the differences near the rim matter.

Provide the reflection card:

\[
v=d-2(d\cdot\hat n)\hat n,\qquad \|\hat n\|=1.
\]

Give the normals without requiring prior multivariable calculus:

\[
n_s=(x,y,z-R_s)^T,\qquad n_p=(x,y,-2f)^T.
\]

Normalize before use. Reversing the normal must leave the reflected direction unchanged. Students calculate one noncentral ray in an axial plane, check its norm and outgoing direction, and then reveal the whole bundle.

Plot 9–15 rays for legibility; these display rays are not the full numerical integration sample. Mark the incoming direction with arrows and distinguish incoming and outgoing segments by line style as well as color.

### O2. Optimize the detector position

For a reflection point p_i and outgoing direction v_i, supply the intersection relation:

\[
\tau_i=\frac{s-p_{iz}}{v_{iz}},\qquad
q_i(s)=p_i+\tau_i v_i.
\]

Require v_iz away from zero and τ_i ≥ 0 for a forward hit. Do not silently discard failed rays and report an artificially improved score. Either restrict controls to a valid domain or explicitly report an invalid experiment and its missed-ray count.

Define the optimization objective in the text:

\[
J(s)=\frac1N\sum_{i=1}^N\left(q_{ix}(s)^2+q_{iy}(s)^2\right),
\qquad \operatorname{RMS}(s)=\sqrt{J(s)}.
\]

J has units m² and RMS has units m. This is distance to the optical axis on the detector, not distance to a fitted centroid and not the full spot diameter.

Use deterministic equal-area aperture sampling: radial samples proportional to sqrt((k + 1/2)/n), with evenly spaced azimuths. This represents uniform incoming illumination over the disk. Do not weight equal-radius rings equally. Use the same points for every detector position and both mirror shapes.

Student tasks:

1. Record an initial detector position and its predicted quality.
2. Complete the short expression computing J from a supplied array of hit coordinates.
3. Use a coarse search over s ∈ [1.5, 2.5] m, then refine around the best position.
4. Validate on a denser, staggered equal-area sample, rather than the exact optimization sample.
5. Explain the remaining positive RMS: positioning alone cannot eliminate spherical aberration.

Teacher calibration: with 256 equal-area radial midpoint samples, the axisymmetric spherical model gives s* ≈ 1.89349 m and RMS ≈ 0.0229653 m. Treat these as sampling-dependent reference values with tolerances, not exact student answers. Preliminary scalar calculations verified these values during planning; full implementation tests are still required.

Independent teacher check: set b_i = v_i,xy / v_iz and a_i = p_i,xy − p_iz b_i. Then q_i,xy(s) = a_i + s b_i, so J is a quadratic in s. Its unconstrained minimizer is

\[
s_*=-\frac{\sum_i a_i\cdot b_i}{\sum_i\|b_i\|^2}.
\]

Compare this with the grid/refinement result. If constrained, clip to the allowed interval. Handle a zero denominator explicitly. Keep this analytic result as an optional challenge after the student search, not an answer revealed beforehand.

### O3. Repair the mirror geometry

New brief: collect the same on-axis parallel bundle at f = 2 m using the same aperture ρ = 1.5 m. Supply the parabola focus relation as a tool; students calculate its canonical equation and depth.

Teacher reference:

\[
z=\frac{x^2+y^2}{8},\qquad
D_p=\frac{\rho^2}{4f}=0.28125\ \mathrm m.
\]

The spherical cap depth is

\[
D_s=R_s-\sqrt{R_s^2-\rho^2}\approx0.2919008\ \mathrm m.
\]

The roughly 1.07 cm rim-depth difference is small relative to the instrument, yet the focusing behavior changes substantially.

Required actions:

- Predict a horizontal section and an axial section before revealing them.
- Rotate the axial parabola to see the paraboloid patch.
- Position the detector using the equation, not trial-and-error.
- Verify that the RMS is near floating-point precision for this ideal model.
- Explain why this does not mean real instruments have zero spot size.

Do not use the central ray alone as a focus test: it lies on the optical axis everywhere and cannot determine the focal distance.

### O4. Robustness and tradeoff

Required comparison: aperture radii 0.5, 1.0, and 1.5 m. Re-optimize the spherical detector separately for each aperture; compare both the optimized RMS and collected geometric area πρ². Otherwise the exercise confounds defocus with spherical aberration.

Students choose whether they would accept reduced light collection for a smaller spot and defend the choice using the table. There is no single best tradeoff without a specified requirement. For an optional decision task, supply a spot-size threshold and ask for the largest admissible aperture within the tested range, with refinement if necessary.

Keep all controls in a domain where the chosen detector interval intercepts forward-going rays. For these examples, ρ/R_s ≤ 0.375; do not expose arbitrarily deep spherical caps without explaining backward/horizontal outgoing rays.

Optional extension, outside the 90-minute route: a finite source at A and an ellipsoid of revolution reflecting toward its other focus B. Do not implement this extension before the core route is complete.

## 8. Epilogue and course coverage

Three individual prompts:

1. For z = x² − y², draw the sections x = 0, y = 0, and z = 0. Explain the two intersecting lines at z = 0 and why this is not a surface of revolution.
2. Compare x² + y² − z² = 1 and z² − x² − y² = 1 through horizontal sections. Explain the connected-waist versus two-separated-parts distinction.
3. Complete: “Changing the equation changed the behavior of the design because …” Include one geometric observation and one numerical check from the chapter.

Provide a compact reference atlas AFTER the investigations: ellipsoid, one- and two-sheet hyperboloids, elliptic and hyperbolic paraboloids, cone, and representative cylinders. For each: canonical equation, two diagnostic coordinate sections, and one boundary/degenerate section if applicable. Include a triaxial ellipsoid and a noncircular elliptic paraboloid to prevent equating all quadrics with rotational examples. Separate out-of-class exploration from the timed exit ticket.

Link the atlas to the week's README and notation conventions. Do not invent a missing lecture PDF or slide link.

## 9. Visualization specification

### General rules

- Use Plotly for interactive 3D and synchronized experiment panels; NumPy for calculations; Matplotlib for a complete static fallback.
- Do not require ipywidgets/FigureWidget or a server. Prefer built-in Plotly sliders/buttons with modest precomputed states. Experiment parameters can also be changed in ordinary cells and rerun.
- A 3D view answers shape questions; a 2D section answers measurement questions. Never ask students to estimate a precise radius from perspective alone.
- White/light background, dark readable text, labeled axes with units, equal physical scaling, and stable ranges for comparisons. Use Plotly scene aspectmode='data'; equal aspect for geometric 2D panels.
- Avoid decorative rainbow scales. Use a small blue/orange/purple palette with line patterns, labels, or symbols carrying the same distinctions.
- Translucency must not obscure the section curve. Allow the surface to be hidden. Highlight intersections with thick curves and provide a separate flat section view.
- Preserve the camera across parameter updates where supported, with an explicit reset-view control. Use orthographic default views for dimension comparison; free rotation is supplementary.
- No automatic continuous animation. A few meaningful states plus a slider are sufficient. Avoid hundreds of 3D frames or giant stored HTML outputs.
- Every figure has a caption explaining what to look for and a text/table alternative for key numerical information.

### Story 1 layout

At most three panels: 3D frame/patch; axial profile; horizontal section. Highlight the selected z level consistently. The member view and rotation view may be separate figures to avoid too many controls at once.

Suggested starting budgets: 24 members, 60–80 samples per profile direction, and about 21 offset states. Treat these as tuning starting points; measure usability and output size in the target environment.

### Story 2 layout

Main optimization scene: axial ray diagram, detector spot diagram, and J(s) or RMS(s) curve. Use one detector-position control to update all three with precomputed states. Mark the current position on the error curve. The final comparison keeps scales identical for both mirrors, with an explicitly labelled zoomed inset if the ideal spot is too small to see.

Do not display dense 3D ray bundles. A 3D mirror view establishes geometry once; the axial diagram and detector view carry the actual investigation.

### Static and interaction failure route

Every experiment must have a normal parameter-cell invocation producing fixed-state Matplotlib views and numerical summaries. Renderer/import failure must not remove the exercise. If a browser silently fails to display an interactive figure, provide a visible “use static mode and rerun” instruction; do not claim Python can always detect browser display failures.

## 10. Colab, dependency compatibility, and updates

The user's request for updates is implemented as two separate features: environment compatibility diagnostics and an optional notebook-release check. Automatic package upgrades or remote-code execution are not part of the design.

### Colab link

Verified repository remote: `https://github.com/elena-kornaeva/AGLA.git`; local origin/HEAD points to main. Reconfirm at release time.

Intended URL, to activate ONLY after the target is publicly available and checked:

`https://colab.research.google.com/github/elena-kornaeva/AGLA/blob/main/weeks/week-10-quadrics/notebooks/straight-structures-focused-light.ipynb`

Until publication, use local notebook links and explain File → Upload notebook in Colab. Do not display a live badge pointing to an absent remote notebook. Implementation does not imply authorization to push or publish; use the user's then-current instructions.

When verified, put the Open in Colab badge/link in the notebook front matter and week's README. Check the student path and, if offered, the teacher path separately.

### Environment diagnostics

- Inspect Python, NumPy, Matplotlib, and optional Plotly availability and versions using installed metadata.
- Smoke-test the actual used capabilities rather than guessing compatibility from a broad minimum-version inequality.
- Record tested versions in teacher/release notes; avoid failing simply because an installed version is newer.
- If Plotly is unavailable, use Matplotlib; do not silently pip install or upgrade packages.
- Report succinctly: ready for interactive mode, ready for static mode, or actionable missing core dependency.
- No network request is required for the default execution route, apart from the hosting application's own delivery. Avoid figure assets that depend on a plotting CDN; use locally supplied Plotly JS where practical, or the static route.

### Optional notebook update check

- Embed NOTEBOOK_ID, NOTEBOOK_VERSION, and release date in notebook metadata/setup.
- A separate explicitly enabled control fetches only a small JSON manifest from this repository with a short timeout and bounded response size.
- Validate schema and notebook ID; compare parsed version tuples, not strings.
- Show current version, published version, and a link to open the published notebook. A different version is not automatically newer (e.g. a local development copy).
- Network failure, missing manifest, malformed data, rate limiting, or offline use means “could not check,” never “up to date.” Continue the chapter normally.
- Never overwrite the student's notebook, answers, or runtime code. Never execute anything obtained from the update request. Opening a new release is an explicit student action.
- The manifest contains only release metadata and the canonical notebook path; no external assets or student data are exchanged.

## 11. Feedback, execution, and mathematical QA

Exercise states: NOT ATTEMPTED, NEEDS REVISION, VERIFIED, rendered in Russian with words and symbols, not color alone. Feedback should identify the issue (units, radius versus diameter, angle convention, normal normalization, invalid detector position) without dumping the entire solution.

Recompute feedback from current inputs. If controls or answers change, do not retain a stale VERIFIED badge. Re-running setup must not accumulate callbacks or duplicate output widgets.

Core checks, using independent reasoning where possible:

1. Ring/waist dimensions match the design; interior member points satisfy the implicit equation over several angles and heights.
2. All sampled members have equal computed endpoint distance; compare with the independent chord-plus-height calculation.
3. δ = 0 and δ = π produce the cylinder and cone limits without divide-by-zero artifacts.
4. Surface meshes and flat sections agree numerically; physical truncation is respected.
5. Reflection preserves unit length, reverses the normal component, preserves the tangential component, and is invariant under normal reversal.
6. Detector hits satisfy z = s and forward travel; invalid rays are explicitly handled.
7. A paraboloid focuses multiple noncentral radii and azimuths at (0,0,f) within a scale-aware tolerance.
8. Grid-refined spherical optimum agrees with the independent quadratic-objective minimum and a denser sample.
9. Large/small aperture behavior is consistent after separate re-optimization. Collected area follows πρ².
10. Empty, incorrect, nonfinite, and wrongly shaped exercise inputs produce useful feedback without breaking Run all.
11. Core execution makes no update request unless explicitly enabled. Test update handling with deterministic local fixtures for newer/same/older/malformed/unreachable cases.

Use dimensionally meaningful tolerances, relative to the geometry scale where appropriate. Do not demand literal floating-point equality or claim an ideal computed RMS must be exactly zero.

Execute student and teacher notebooks in fresh kernels. Save clean outputs, avoiding giant embedded frames and any personal metadata. Inspect representative rendered figures in light mode, including extremes and static fallback. Test a direct Colab session when available; explicitly report if only local validation was possible. Do not install unrelated packages or broaden tests after requirements pass without a reason.

## 12. Implementation batches

Each batch ends with a reviewable artifact, focused checks, and a short report. Respect the user's instruction to implement batch by batch; do not treat this planning document as approval to implement all batches immediately.

| Batch | Scope | Acceptance criteria |
| --- | --- | --- |
| 1. Chapter skeleton | Student and teacher notebook structure, Russian prose, all exercise prompts, tool cards, stable IDs, and collapsed setup scaffold | Full 90-minute narrative is readable; no answer leakage; unfinished cells run safely; no dead Colab link |
| 2. Frame laboratory | T1–T3 computations, 3D/section/rotation views, static fallback, and frame feedback | Correct dimensions and straight-member membership; cylinder/cone limits; visual inspection of all modes |
| 3. Mirror geometry | Profiles, normals, reflection, ray hits, and static/interactive views for O1 | Independent reflection invariants; forward-hit checks; consistent coordinate conventions and scales |
| 4. Optimization and repair | O2–O4 objectives, linked panels, student exercises, refinement, paraboloid design, and aperture tradeoff | Spherical optimum independently verified; equal-area sampling; near-zero ideal paraboloid error; no missed-ray score bias |
| 5. Book integration and robustness | Self-check flow, reveal order, atlas, exit ticket, compatibility diagnostics, optional release check, and teacher answers | Clear reading path; current feedback; successful static route; update failures harmless; no external runtime dependency for core calculations |
| 6. Verification and delivery | Clean execution, visual QA, Colab check if available, README/course-map updates, and release-link preparation | All applicable checks pass; tested environments reported precisely; publication-dependent links remain inactive until verified |

Before every batch, read this plan and the applicable AGENTS.md, inspect the current repository state, and preserve user edits. Update the plan if an implementation finding changes a constraint. Do not spend the session rebuilding already verified pieces.

Suggested handoff request: “Implement batch 1 of weeks/week-10-quadrics/tutorial-plan.md. Keep the student materials in Russian and progress reports in English. Stop after batch acceptance checks and report the result.”

## 13. Sources and attribution

These sources motivate the applications and verify tool capabilities. They do not prescribe the complete chapter or its timings.

- University of Arizona, ruled-surface models: https://archive.math.arizona.edu/models/Ruled_Surfaces/index.html
- Smithsonian, hyperbolic-paraboloid string model: https://americanhistory.si.edu/collections/object/nmah_693898
- OpenStax, spherical mirrors and spherical aberration: https://openstax.org/books/university-physics-volume-3/pages/2-2-spherical-mirrors
- Plotly, renderers including Colab: https://plotly.com/python/renderers/
- Plotly, sliders: https://plotly.com/python/sliders/
- Plotly, 3D scene configuration: https://plotly.com/python/reference/layout/scene/
- Google Colab, forms: https://colab.research.google.com/notebooks/forms.ipynb
- Google Colab, GitHub link format: https://github.com/googlecolab/open_in_colab

Generate all chapter diagrams from original code and synthetic parameters. Do not copy external images or download source PDFs. If an external image is later introduced, record its specific source and license before including it. Add a short student-readable source note, not a large bibliography interrupting the narrative.
