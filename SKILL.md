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
- Props: exact prop type, side, scale, orientation, contact point, occlusion state, screen glow, and whether the prop is held, hidden, dropped, offered, inserted, or only implied.
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

For any fragile location, do not rely on a generic label such as "old stairwell", "office corridor", "hospital room", "apartment", "alley", or "cat cafe." Write concrete facts like "phone stays in left foreground, right-side wall pipes/handrail/doorframe stay on the right, upper door stays right of center, stairs continue upward then left-turn at the far landing, forbid mirrored right-turn." The examples can change; the rule is to lock physical anchors, not mood labels.

## Axis Turns And Newly Featured Objects

When a shot suddenly features a door, vehicle, table, elevator, window, weapon, screen, animal, or other major object that was not dominant in the previous frame, the prompt must make the transition legible.

- State whether it is the same physical space with a camera/POV turn, a motivated cut, or a new location.
- Keep at least one previous spatial anchor visible when possible: same wall texture, floor, railing, lamp, window, doorframe, furniture edge, character in background, or prop in foreground.
- Use language like "POV turns 35 degrees right from the previous frame toward the nearby door lock" or "camera pans from the character to the table in the same room" instead of simply naming the new object.
- If the object was only in the background before, say so: "the background door from the prior frame now becomes the right-foreground door lock."
- Flag as P1 if the object is script-correct but visually feels like it appears from nowhere. Regenerate or add a bridge prompt before video handoff.

## Room And Furniture Map Continuity

Interior scenes need a room map, not just a room label. Once a space is established, furniture sides and large landmarks must not drift between adjacent shots.

- Write a canonical room map before generating close-ups: "bed/sofa on left rear wall, old TV/cabinet on right rear wall, main door rear center, table foreground", or the equivalent for that scene.
- For every later shot in that room, restate the landmarks that should remain visible or implied. A close-up may crop furniture, but it must not mirror the map.
- If one frame flips bed/sofa, cabinet, window, door, desk, lamp, or shelf positions while the surrounding frames agree, reject the outlier rather than regenerating the whole sequence.
- Treat a mirrored furniture map as P0 when it changes navigation, blocking, or video continuity; treat it as P1 when the story still works but the video handoff would look unstable.
- Do not use a reference image with a different room map as the main prompt reference. If it is only a mood or texture reference, explicitly say "use texture only, do not copy layout."

## POV Micro-Action And Small Prop Repair

When an image model repeatedly misdraws hands, object ownership, or left/right anatomy, stop asking for a specific hand unless that hand orientation is story-critical. Reduce the action to visible evidence at the frame edge.

This applies to any short-drama prop or micro-action: paper, card, phone, key, ring, medicine, lipstick, badge, weapon, ticket, receipt, USB drive, pet collar, food, cup, bag strap, door handle, sleeve, pocket, or any small object that can drift in generated images.

- Prefer sleeve edge, wrist edge, cloth fold, pocket gap, tabletop contact, shadow, reflection, partial knuckles, fingertip, or object movement over a full palm or full arm.
- For hidden or partially revealed props, say exactly where and how much is visible: peeking from a pocket, half under a phone, pressed under a cup, reflected on glass, caught in a door gap, tucked under fabric, or barely visible at frame edge.
- Forbid the common wrong expansions: full sheet, folder, bag, oversized object, table placement when it should be hidden, hand holding when it should only peek out, or readable text when the prop should be blank.
- If the hand is not the story point, describe the result instead of the anatomy: "fabric edge is pressed inward", "object corner is tucked deeper", "screen glow touches the knuckles", "key ring swings slightly", "cup shadow covers the receipt".
- Do not let a prop-placement problem become a POV-owner body problem. The POV owner remains off-screen except for the minimum visible hand/sleeve/prop evidence required by the shot.
- When a generated frame keeps creating bad hands, the retry prompt should explicitly say: "do not draw a complete hand; do not emphasize left hand or right hand; show only the needed local evidence such as sleeve edge, fingertip, partial knuckles, object edge, shadow, or contact point."
- If a model keeps ignoring a subtle action, make the action easier to see without changing the story: increase prop scale slightly, move it closer to a light source, use a stronger shadow/contact point, or crop closer, but do not invent a new location or new visible character.

## Review Rules

P0 regenerate immediately:

- The scene becomes a different location or a mirrored version of the location.
- A room's furniture map flips or relocates major landmarks without a script reason.
- A left turn becomes a right turn, or the camera axis reverses without script reason.
- POV owner appears as a face, body, back, half-body, or full reflection when the project is first-person POV.
- A different character is introduced or a required visible character disappears.
- Readable UI/text appears in a video-reference prompt where it should be post overlay only.
- A POV small prop or hidden object expands into a different object, a full-size object, an unplanned bag/folder/container, or a visible body/arm action that changes the story logic.

P1 fix before video handoff:

- Props move to the wrong side or lose story meaning.
- Character wardrobe/age/identity drifts.
- Reference arrows, borders, labels, or watermarks are copied into the new frame.
- The frame is otherwise useful but contains visible guide arrows, border/crop marks, or composition annotations.
- Character/scene/prop reference images contain large text panels or baked-in UI that could be learned by the video model.
- The scene is technically similar but loses important landmarks needed for video stability.
- A full hand or full arm is drawn where local evidence such as sleeve edge, fingertip, partial knuckles, shadow, reflection, contact point, or implied object movement would be safer and clearer.
- A script-correct door/object/screen/vehicle enters as the new focus, but the frame fails to show how the camera got there from the previous shot.

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
- For interiors, does the furniture map match the canonical room map?
- Are the POV owner constraints respected?
- Are required characters visible and only allowed characters visible?
- Are story-critical props in the correct place?
- If a new object becomes dominant, is the camera turn/cut motivated and anchored to the previous frame?
- If a full hand is not required, did the prompt reduce it to local evidence such as sleeve edge, fingertip, partial knuckles, shadow, reflection, contact point, or implied prop movement?
- Are annotations excluded from the generated image?
- Is any UI/text appropriate for this artifact type?
- Would this image help video generation stay stable, or teach the video model the wrong thing?
