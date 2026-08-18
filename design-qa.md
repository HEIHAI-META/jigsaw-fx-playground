# Design QA

- Source visual truth: `/Users/shangwenqi/Downloads/IMG_0540.JPG` plus `/Users/shangwenqi/Downloads/Jigsaw_H5_Demo_最终需求_v3.md`
- Implementation screenshot: `/Users/shangwenqi/Desktop/Jigsaw优化/jigsaw-fx-playground/implementation-e8.png`
- Combined comparison: `/Users/shangwenqi/Desktop/Jigsaw优化/jigsaw-fx-playground/qa-comparison.png`
- Browser viewport: 1280 × 936 CSS px, device scale factor 1
- Source pixels: 4032 × 3024; implementation pixels: 1280 × 936
- Comparison normalization: both artifacts scaled proportionally into 640 × 720 panes, combined at 1280 × 720
- State: E8 completion, 4 Layer Star FX, Overlay Highlight FX, 1× playback

**Full-view comparison evidence**

- The implementation preserves the source product's vivid butterfly/garden subject, centered square puzzle, dark surrounding stage, and completion emphasis. The wider desktop control surface is intentional because this is an evaluation playground rather than a mobile game screen.
- The generated puzzle image is sharp, evenly detailed across all quadrants, and remains continuous across the 5 × 5 piece grid.

**Focused region comparison evidence**

- Puzzle surface: piece seams remain readable without breaking image continuity; moving and fixed states are visually distinct.
- Completion state: the PRD-required final sweep and completion ceremony remain contained to the puzzle and do not obscure the controls.
- Control panel: Star, Highlight, distribution, timing, range, strength, and playback parameters remain visible in one desktop viewport.

**Required fidelity surfaces**

- Fonts and typography: compact system sans-serif hierarchy is clear at small tool-label sizes; Chinese case titles remain legible.
- Spacing and layout rhythm: eight cases fit in one row; stage and control panel align; persistent playback controls are not clipped.
- Colors and visual tokens: dark neutral shell keeps the gold star and mint highlight effects visually separable.
- Image quality and asset fidelity: the generated artwork is high resolution and correctly cropped; no placeholder imagery or stretched screenshot is used.
- Copy and content: all eight PRD event names, levels, modes, and parameter labels are represented.

**Interaction verification**

- Tested E1–E8 selection; each loaded the correct heading and moving-piece count.
- Tested Play, Replay, Pause/Resume, Reset, and 0.5×/1×/2× speed controls.
- Tested 4 Layer Star FX and Overlay Highlight FX together on E8.
- Verified 22 rendered star particles, overlay presence, and E8 completion label.
- Browser console: no warnings or errors.

**Comparison history**

- Iteration 1 finding [P2]: the initial tooth mask created oversized dark holes that competed with the feedback animation.
- Fix: restored continuous square image coverage and replaced the mask with restrained curved tooth-seam outlines.
- Post-fix evidence: `implementation-e8.png` and `qa-comparison.png` show a continuous image surface with readable seams and no dark holes.
- Iteration 2 finding [P1]: feedback highlight was rendered as independent tile overlays, so the connected group appeared segmented during the brightest frame.
- Fix: replaced all per-tile overlays with one SVG light field clipped through the union of the active pieces; Horizontal, Cross, Distance, and Overlay now change only the propagation geometry.
- Iteration 2 finding [P1]: the simplified curved seam marks did not reproduce complete complementary jigsaw boundaries.
- Fix: introduced deterministic four-sided paths for every grid position. Shared edges are paired as convex/concave opposites, outer edges remain flat, and every piece carries the correctly aligned portion of the source image through its teeth.
- Post-fix evidence: `implementation-e5-continuous-highlight.png` shows the E5 moving group with complete teeth and one uninterrupted Horizontal highlight. Browser checks found one continuous highlight SVG, zero tile highlights, and no console warnings or errors.
- Iteration 3 clarification: the highlight target is the complete connected group formed after snapping, not a local subset selected by distance from the moving piece.
- Fix: each Case now provides a complete post-connection group mask. E5 covers all 15 pieces in the resulting group; E6 covers all 16 border pieces; E7 covers all 9 region pieces; E8 covers all 25 pieces. Direction modes animate across the full group and never reduce the final target coverage.
- Post-fix evidence: `implementation-e5-whole-group-highlight.png`; automated DOM verification matched mask-path and visible-piece counts (15/15), with E6/E7/E8 coverage verified as 16/9/25 and no console warnings or errors.
- Iteration 4: localized the complete evaluation interface to Chinese while retaining only review identifiers (E1–E8, L1–L4) and standard technical units. Browser verification confirmed Chinese playback controls and no console warnings or errors; evidence: `implementation-chinese-ui.png`.
- Iteration 5 principle correction: Star FX now uses only the dragged piece or dragged multi-piece combination as its feedback object. Particle positions are generated from every exposed edge of the actual union outline, including concave edges of irregular groups, rather than from the target seam, connected result, or rectangular center field. Layer delays and fixed positions create the radiating impression without particle travel.
- Added a persistent Chinese principle board above the preview: Star FX = dragged-object outline; Highlight FX = full connected result. E5 browser verification covered all 8 exposed edges of its L-shaped moving combination, rendered the complete connected-group highlight, and produced no console warnings or errors. Evidence: `implementation-principles-outline-stars.png`.
- Iteration 6 splatter clarification: two-layer Star FX now forms two particle bands offset outward from the dragged object's true union outline. Particles remain stationary; the inner band appears first at the Min Size setting and the outer band follows at the Max Size setting, with only restrained random size variation.
- E5 browser verification rendered 11 particles per band. Both bands independently covered all 8 exposed outline edges; the inner band averaged 5.05 px and the outer band averaged 15.90 px with the default 5/16 px controls. No console warnings or errors were produced. Evidence: `implementation-two-ring-splatter.png`.
- Iteration 7 control clarification: removed all visible new-seam markers and their legend; changing any Star, Highlight, distribution, timing, or playback-speed parameter now increments the animation run and immediately replays the sequence. Star and Highlight fade-in/fade-out values are wired to independent animation phases.
- Renamed technical controls around user-visible outcomes, added concise inline explanations, removed the conflicting Highlight Spread Range control, and separated Highlight Start Delay into a dedicated “星芒与高亮间隔” panel. Browser verification confirmed automatic replay, four rendered Star layers, active fade/delay CSS variables, zero seam nodes, and no console warnings or errors. Evidence: `implementation-controls-clarified.png`.
- Iteration 8: added an independent Highlight Propagation Speed control from 0.5× to 2×. It scales the complete Highlight phase—including propagation, fade-in, fade-out, and inter-wave timing—without changing Star FX timing or the configured Star-to-Highlight start delay. Browser verification confirmed the control, live Highlight rendering, and no new console warnings or errors.
- Iteration 9: reinforced every piece path in the shared SVG mask so the Highlight union closes anti-aliased internal seams while retaining the exact jigsaw-tooth outer contour. Added a fifth “直接覆盖” mode that lights the complete post-connection group simultaneously with no directional or radial propagation. E5 verification found one direct-cover surface clipped through 15 reinforced union-mask paths, zero per-piece highlight layers, and no console warnings or errors.
- Iteration 10: added a three-state effect switch for Star-only, Highlight-only, and combined playback. Inactive parameter panels are visibly disabled, and completion-only celebrations remain isolated to combined playback. Added a Highlight Duration control (400–2400 ms) that defines the full Highlight lifetime before the independent speed multiplier is applied. Browser verification produced 22/0 Star/Highlight nodes in Star-only, 0/1 in Highlight-only, and 22/1 in combined mode; the default 1300 ms lifetime was applied exactly with no page warnings or errors.
- Iteration 11: added a bottom parameter-save dashboard with one independent local preset slot for each of E1–E8. The active Case can save, overwrite, or clear its full configuration; saved inactive Cases can be loaded and replayed directly, and top navigation restores a Case's saved settings automatically. Browser verification confirmed all 8 slots, save counter updates, persistence across reload, clearing, and no console warnings or errors; QA data was cleared afterward.
- Iteration 12: changed Star FX geometry from outline-shaped bands to concentric circular bands centered on the dragged piece/group bounding-box center. Replaced the continuous Range slider with five explicit diameter presets: 1×, 1.5×, 2×, 2.5×, and 3× relative to the dragged group's longest bounding-box side. E5 at 2× verified an inner average radius of 88.0 px and outer average radius of 184.3 px around the expected (276, 276) center, with all five choices available and no console warnings or errors.
- Iteration 13: separated Distance and Overlay Highlight behavior after identifying that both previously shared the same radial scaling. Distance now expands through four visibly stepped distance levels, while Overlay remains a continuous radial wash with an 18 px soft blur. Added a live one-sentence explanation beneath the mode selector. Browser verification confirmed mutually exclusive distance/overlay surfaces, the correct soft filter only on Overlay, distinct mode descriptions, and no console warnings or errors.
- Iteration 14: unified the propagation origin for Horizontal, Cross, Distance, and Overlay Highlight modes to the center of the complete post-connection group's bounding box. Direct mode remains centerless. E2 verification calculated and rendered the two-piece group center at (276, 230) for both Distance and Horizontal modes, matching the expected union-bounds center with no console warnings or errors.
- Iteration 15: corrected E1 to represent a true single-piece placement. All 24 non-participating puzzle pieces are fully absent rather than faint fixed context; only the one dragged piece appears, snaps into place, receives circular Star FX, and forms the one-piece Highlight mask. Browser verification found 1 visible/moving piece, 24 absent pieces, a 1-path Highlight target, and no console warnings or errors.
- Iteration 16: rebuilt Horizontal, Cross, and Distance Highlight modes around visible wave fronts rather than filled-area scaling. Horizontal renders two delayed horizontal wave fronts, Cross sequences horizontal and vertical wave fronts, and Distance renders three delayed expanding rings; each settles into a continuous full-group Highlight. Overlay now exclusively owns the smooth filled radial expansion behavior. Renamed the timing control to “波浪延迟” and wired it between successive fronts/directions/rings. Browser verification found 2 Horizontal waves, 1+1 Cross waves, 3 Distance rings, an Overlay-only soft surface, one Wave Delay control, and no console warnings or errors.
- Iteration 17: restored Overlay (“柔光扩散”) as an exact behavioral copy of the former Distance implementation: one filled Highlight surface expands from the complete-group center through four stepped distance levels, leaves passed regions highlighted, and then fades as a whole. Removed the continuous blur filter from this mode. Browser verification confirmed one unfiltered Overlay cover, zero Distance rings in Overlay mode, the updated four-level description, and no console warnings or errors.
- Iteration 18: replaced simulated light bands/rings in Horizontal, Cross, and Distance with true piece-batch Highlight pulses. Horizontal groups exact jigsaw paths by column, Cross runs a column sequence followed by a row sequence, and Distance groups pieces by Manhattan adjacency distance from the complete-group center. Added two experiment controls: Wave Flow (sequential pulse, overlapping pulse, or accumulating light) and Horizontal Direction (center-out, left-to-right, or right-to-left). Every batch uses complete tooth paths with reinforced internal seams. E5 verification found 15 Horizontal paths, 30 Cross passes, 15 Distance paths, no residual ring geometry, a live Wave Delay control, all flow/direction options, and no page warnings or errors.
- Iteration 19: reorganized the right control rail into exclusive Star Settings and Highlight Settings tabs, with Highlight timing kept inside the Highlight view. Selecting Star-only or Highlight-only automatically opens the matching parameter tab; switching tabs itself does not replay. Made the left preview card sticky within the workspace so it remains visible while scrolling through parameters. Browser verification confirmed only one parameter family is mounted at a time, correct automatic tab selection, sticky positioning at 10 px, and no page warnings or errors.
- Iteration 20: rebuilt Edge + Particle as a true edge-band mode. The full dragged-piece/group union is rendered into one alpha surface, morphologically expanded, and subtracted from itself to produce a simultaneous outer contour that follows every jigsaw tooth while excluding internal joined seams. Circular Range controls are hidden in this mode and replaced with an applicability note.
- Iteration 21: changed the Edge + Particle treatment to a single outer-side dotted contour. Equal-size circular points follow the dragged piece/group's exact union perimeter immediately outside the fitted light band; internal seams are removed by the union mask. This mode exposes one “点状星芒尺寸” control and hides count, maximum-size, layer-delay, distribution, and randomness controls because they do not apply.
- Iteration 22: added three independently switchable geometry situations to E3, E4, and E5 only: the retained current arrangement, a long horizontal bar, and a long L. A compact switcher sits on the left side of the stage; changing it replays the selected case and recalculates the moving group outline, star center/range, highlight mask, and propagation batches from the new geometry.
- Iteration 23: separated ordinary star placement from the independent edge-band effect. Ordinary two-layer/four-layer stars now support circle rings, the dragged piece/group's true exposed perimeter, or only the union outline's geometric corners. Corner mode removes perimeter propagation and places one equal-size star at each real turn; its irrelevant count, outer-size, layer-delay, distribution, and randomness controls are hidden.
- Iteration 24: restored an explicit total-count control for corner placement. The chosen total is distributed across the union outline's true turns; overflow creates compact outward-facing corner clusters instead of extending along any edge. Corner stars remain equal-size and simultaneous.
- Iteration 25: rebuilt corner placement as layered quarter-circle arcs. Every convex and concave union turn becomes the center of an outward-facing 90° arc; two-layer/four-layer mode controls concentric radii, inner/outer size interpolation, and layer delay. Total count is allocated across corners and layers rather than along perimeter segments.
- Iteration 26: added an E8-only completion beam panel. The existing diagonal warm-white sweep remains single-pass and fixed-direction, while start delay, duration, width, brightness, and edge softness now drive dedicated beam variables and automatically replay the case after adjustment.
- Iteration 27: removed E6's automatic gold perimeter celebration entirely. E6 now uses only the selected ordinary star and highlight feedback; the separately selectable Edge + Particle star mode remains available and unchanged.
- Iteration 28: rebuilt Drag Edge placement as evenly sampled parallel contour layers. Each layer now covers the full exposed union perimeter at a constant outward offset, reproduces jigsaw tabs/notches with the same translated profile, and uses fixed near-to-far offsets for two or four visually parallel lines. Layer size and timing still interpolate from inner to outer controls.

**Findings**

- No actionable P0, P1, or P2 issues remain.

**Follow-up polish**

- P3: replace the generated demonstration artwork with production Jigsaw artwork if a later review needs exact asset fidelity.

final result: passed
