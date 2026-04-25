---
name: Firefox AudioContext — use click-to-start overlay
description: Firefox requires a real click gesture to unlock AudioContext; gamepadconnected and keydown don't reliably work
type: feedback
---
Always use a click-to-start overlay to unlock the Web Audio API — do not rely on `keydown` or `gamepadconnected` to call `audioCtx.resume()`.

**Why:** Firefox (and some Chrome versions) require a genuine pointer/click user gesture to unlock AudioContext. `gamepadconnected` doesn't qualify in Firefox. Using `keydown` to resume audio caused an accidental gun sound because the key was also processed as a game input in the same frame.

**How to apply:** Add a fullscreen overlay with "CLICK TO START" at page load. On click: call `audioCtx.resume()`, hide overlay. Remove all other `audioCtx.resume()` calls from input handlers.
