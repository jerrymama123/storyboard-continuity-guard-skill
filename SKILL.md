---
name: storyboard-continuity-guard
description: Audit and repair storyboard frame, scene reference, line-art, and video-reference continuity for AI short drama production. Use when working with ChatGPT Image, Seedance, Kling, Sora, storyboard guidance frames, scene images, reference-image packs, POV shots, or any task where generated images must preserve spatial direction, character placement, props, camera axis, or scene consistency across shots.
---

# Storyboard Continuity Guard

## Purpose

Use this skill before approving or regenerating storyboard line art, scene references, and image-to-video guidance frames. The goal is to prevent AI image tools from silently changing location geometry, left/right direction, character identity, POV rules, props, or UI/text treatment between adjacent shots.

## Core Workflow

1. Identify the continuity chain: previous approved frame, current target shot, next shot, scene location, character visibility, props, UI overlays, and camera axis.
2. Choose continuity anchors from approved images, not from vague text. Prefer the most recent approved frame from the same space.
3. Write a compact continuity lock before the creative prompt. Include exact spatial anchors, forbidden changes, and whether reference annotations are content or not.
4. Generate or review one shot at a time. Do not batch-approve if one image breaks the space.
5. Classify issues as P0/P1/P2 and decide: approve, approve with note, regenerate, or update the source prompt.

## Continuity Anchors To Lock

Always lock concrete visual facts, not generic scene labels.

- Direction: left turn, right turn, stairs up/down, door side, hallway bend, camera axis.
- Layout: foreground/midground/background, wall side, door position, window position, handrail, pipes, lamps, table edge.
- Props: phone side, contract/paper corner, key position, bag, door lock, screen glow.
- Characters: who may appear, where they stand, wardrobe continuity, expression continuity, distance to camera.
- POV owner: hands/sleeves/phone/props may appear; face, body, back, full reflection, or mirror-body must not appear unless explicitly allowed.
- UI/text: decide whether text belongs in the static reference image or must be a blank placeholder for post overlay.
- Reference marks: arrows, boxes, labels, watermarks, and guide borders in reference images must usually be excluded from generated output.

## Approved Versus Rough References

Do not treat a visually useful image as approved if arrows, corner crop marks, black/white frame borders, direction curves, composition labels, or teaching annotations are drawn into the image.

- Approved reference: clean frame, no guide marks, safe to feed into video generation.
- Rough reference: useful for explaining composition, but must stay in rejected/drafts and be regenerated or cleaned before video handoff.
- If a rough reference must be used to explain a follow-up prompt, explicitly say: "Use only the layout/pose; do not copy arrows, borders, labels, or guide marks."
- Character sheets or scene cards with text panels, labels, biographies, system UI, phone text, or A/B/C choice overlays are not video-safe approved references. Treat them as identity/story rough references until a clean no-text crop/version exists.

## Prompt Pattern

Use this structure for any fragile follow-up shot:

```text
Continuity reference images to upload/use:
- [approved-frame.png]: locks [specific visual facts].

Spatial continuity hard rules:
- Continue the exact same [location] from [shot id].
- Keep [anchor A] on the left/right foreground.
- Keep [anchor B] on the same wall/side.
- Keep [door/lamp/handrail/window] in the same relative position.
- Do not mirror-flip the room, hallway, stairs, door, props, or character placement.
- Do not change to a new building, room, corridor, staircase, or apartment unless the script explicitly cuts there.

Reference-image cleanup:
- Use arrows/labels/boxes only to understand composition; do not draw them in the new image.
```

For the stairwell case that caused this skill: do not write only "old rental stairwell." Write facts like "phone stays in left foreground, right-side wall pipes/handrail/doorframe stay on the right, upper door stays right of center, stairs continue upward then left-turn at the far landing, forbid mirrored right-turn."

## Review Rules

P0 regenerate immediately:

- The scene becomes a different location or a mirrored version of the location.
- A left turn becomes a right turn, or the camera axis reverses without script reason.
- POV owner appears as a face, body, back, half-body, or full reflection when the project is first-person POV.
- A different character is introduced or a required visible character disappears.
- Readable UI/text appears in a video-reference prompt where it should be post overlay only.

P1 fix before video handoff:

- Props move to the wrong side or lose story meaning.
- Character wardrobe/age/identity drifts.
- Reference arrows, borders, labels, or watermarks are copied into the new frame.
- The frame is otherwise useful but contains visible guide arrows, border/crop marks, or composition annotations.
- Character/scene/prop reference images contain large text panels or baked-in UI that could be learned by the video model.
- The scene is technically similar but loses important landmarks needed for video stability.

P2 note or polish:

- Minor lighting, texture, or line-weight drift.
- Slight framing difference that does not break geography, POV, or story action.

## Line Art Versus Video Prompts

Line-art/storyboard guidance frames may contain limited intentional labels or clean UI if they are static references and will be reviewed manually.

Video-generation prompts should avoid precise Chinese UI, numbers, app screens, A/B/C choices, countdown text, chat messages, or overlays unless the video tool reliably supports text. Use blank screens, abstract light blocks, clean buttons, or "post overlay only" instructions instead.

## Approval Checklist

Before approving a frame, answer:

- Does it continue the same physical space from the previous approved frame?
- Are left/right relationships unchanged?
- Are the POV owner constraints respected?
- Are required characters visible and only allowed characters visible?
- Are story-critical props in the correct place?
- Are annotations excluded from the generated image?
- Is any UI/text appropriate for this artifact type?
- Would this image help video generation stay stable, or teach the video model the wrong thing?
