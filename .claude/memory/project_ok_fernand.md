---
name: OK FERNAND - Gunslinger Game
description: Pixel-art western top-down shooter game in HTML5 Canvas
type: project
---
"OK FERNAND" is a pixel-art western gunslinger game built in a single HTML file using HTML5 Canvas.

**GitHub:** username `fernandez-studio`, repo `https://github.com/fernandez-studio/RetroGame`
Working directory `W:\Visual\Claude\Gunslinger` is initialized as a git repo, remote set to RetroGame, branch `main`.

**Current file:** `gunslinger_05.html` (committed to main)

**Tech:** Vanilla JS, HTML5 Canvas, no libraries. Arena is 240×160 game pixels, scaled 6x (S=6) to 1440×960 screen pixels.

**What's built:**
- requestAnimationFrame game loop with update()/render() split
- Movement: WASD + left stick (dead zone 0.15); all directions normalize to SPEED/√2 — constant speed
- Player sprite flips horizontally when moving left
- 3-frame leg animation (idle, walk_A, walk_B), ANIM_RATE=6 frames
- Shooting: arrow keys (8-directional) + XYAB buttons (4 cardinal) + right stick (45°-snapped, threshold 0.7)
- BULLET_SPEED=2.5, FIRE_COOLDOWN=30; player bullets white, AI bullets orange-red
- Sounds via Web Audio API: pew, hit, death, teleport in/out, ding, boom, playerDeath, laser, funeralMarch
- Click-to-start overlay unlocks AudioContext — required for Firefox
- Three cacti at 1.2x scale, cactus pixel damage + player sliding collision + ding sound on hit
- Two wooden barrels (dark brown) — randomly placed one per screen half, degrade when shot, block movement
- Teleport system — two portals (corners), fade in/out, TELEPORT_FRAMES=15, cooldown=90
- HP bar sidebar (red fill, div#health-fill); player takes 20 HP per AI bullet hit; 3 heart lives
- COWBOY_SIT / COWBOY_DEAD sprites
- Damsel sprite (9×16, facing left) at (223, 71)
- Power-ups + potions all randomly placed via skirt placement (outer 50% of arena only, no center):
  - Tommy gun (3): 3-shot bursts, 8 bursts
  - Wide-shot (3): fires 3 bullets at ±30°, 8 shots
  - Long-shot L (3): LONG_BULLET_RANGE=480, bounces off walls normally
  - Ricochet R (1): LONG_BULLET_RANGE=480, splits into 2 on wall hit
  - Green healing potion (1): 7×12 glass bottle sprite with flute, blue glass outline, dark cork
  - Red speed potion (1): same bottle, +30% speed for 30 seconds
  - Laser potion (1): same bottle with indigo liquid, grants 3 laser shots (purple ammo cells)
- Laser weapon: instant wall-to-wall beam via rayAABB, passes through cacti/barrels (damaging them), kills AIs; 2-hit instant kill; plays playLaser() sci-fi sweep sound
- Bombs (3): shoot to detonate → 4-directional orange/yellow beams; explosion beams = 2 hits instant kill; limited to 45% arena height; smoke trails perpendicular to beams
- Laser beam rendering: canvas ctx.save/translate/rotate/restore, purple outer + pink core, fades over LASER_FRAMES=20
- No power-ups/potions overlap each other or obstacles (rejection sampling, 600 attempts)
- Game-over on 3rd death: 50% dim overlay + "YOU FAILED YOUR DAUGHTER" text + Chopin Funeral March melody
- Turret (green octagonal body + cyan dome) at (207, 79), in front of damsel. Only activates when player.x+4 < 120 (near/left half). Single bullet at a time (TURRET_RANGE=108px = 45% of 240). On wall/range/player hit: 8-directional shrapnel, each 12px range (5%). Barrel rotates to aim. playTurretFire() mechanical thunk.
- Turret has hp=8; player bullets hit it (hitbox TURRET_CX±5, TURRET_CY±4); body color interpolates green→red per hit; smoke emits when hp≤2; dead=true stops all turret behavior.
- Countdown (3-2-1 + "Save your daughter!") with beeps after click/space; countdown=-1 before start, 3/2/1 counting, 0=play.
- Game-over drawn on canvas center (not HTML div). gameOver=true freezes loop.
- Win condition: turret.dead && killCount≥15 → gameWon=true, pink bobbing heart above damsel, playWinMelody() C-major arpeggio.
- Walk footstep sounds: 3 EQ variants — highpass (horizontal), lowpass (vertical), bandpass (diagonal). Triggered on leg frame change via playStep(type).
- AI leg animation: animFrame/animTick added to createAI(), COWBOY_FRAMES driven same as player when alive.

**AI system (ais[] array):**
- 2 AI cowboys spawn above (210,51) and below (210,90) the damsel at round start
- Frozen until player moves or fires (gameActivated flag)
- Behavior state machine: pursue (direct), flank (perpendicular approach), strafe (circle at range)
- Behaviors switch on random timers; stuck-escape: if blocked 15+ frames, flip flankDir + reset to pursue
- AI shoots every 70–110 frames with slight random spread; fires when alive or sitting
- Hit system: first hit → sitting in place; second hit → dying→dead; 2-second respawn
- Explosion/laser beams call aiHit(a) twice for instant kill

**What's NOT done yet:**
- Win condition (save the damsel / clear all waves)
- Round progression
- Potion pickup HP restore logic (green potion sprite exists but no heal-on-pickup yet)
