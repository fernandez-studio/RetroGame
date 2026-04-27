---
name: Next steps — gunslinger_06
description: Pending fixes and features to implement in the next session
type: project
---

Fixes and features requested for the next working session (gunslinger_06.html):

1. **Turret not receiving damage / not being destroyed** — the bullet-vs-turret hitbox check exists in code but doesn't seem to register hits. Debug and fix. Suspect possible coordinate mismatch or ordering issue in the bullet loop.

2. **Turret sometimes stops working** — the turret occasionally goes dormant mid-game and never fires again. Likely related to the `playerInRange` gate (requires `gameActivated` + `player.x+4 < TURRET_ACTIVE_X`), or a cooldown that stops decrementing when a bullet is in-flight. Investigate and fix so the turret reliably fires throughout the round.

3. **Turret barrel not rotating correctly** — the half-screen activation trigger is correct and should stay. The bug is the barrel only visually turns at slight angles and doesn't aim up/down when the player is above or below the turret's y position. The `turret.angle` math is likely correct but the barrel pixel drawing doesn't reflect large vertical angles properly. Fix the barrel rendering.

**Why:** Player reported these as broken/unreliable during testing of gunslinger_05.

**How to apply:** When user types "next steps", list these items. When ready to implement, create gunslinger_06.html from gunslinger_05.
