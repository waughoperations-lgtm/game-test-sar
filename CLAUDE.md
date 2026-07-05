# Hide from the Monster (night version)

## What this project is
A 3D hide-from-the-monster game that runs in a web browser. The player walks around
a small town at night with a flashlight while a monster prowls and hunts them. The
goal is to survive for 2 minutes without being caught. The monster wanders until it
spots the player, then chases; breaking its line of sight (behind houses or bushes)
makes it lose you. The flashlight helps you see but also gives you away, so you can
turn it off (F key, or the on-screen button) to sneak in the dark. A "danger meter"
fills up and the screen glows red as the monster closes in.

## Important context about the owner
The owner is NOT a programmer. They know how to use a computer but not how to code.
When working with them:
- Explain everything in simple, non-technical language
- Never ask them to edit code themselves — make the changes for them
- After changes, tell them to open index.html in their browser to see the result

## Project structure
- `index.html` — the ENTIRE single-player game in one file (HTML + CSS + JavaScript).
  It loads the Three.js 3D library (version r128) from a CDN, so an internet
  connection is needed the first time it loads.
- `multiplayer.html` — an online multiplayer version (separate file, single-player
  is untouched). Friends join a room by code; one player is randomly the monster and
  hunts the others; hiders survive 2 minutes to win. Networking is peer-to-peer over
  WebRTC via PeerJS (loaded from a CDN) using its free public broker — no server of
  our own. One player is the "host" and acts as the referee/relay, so the host must
  keep their tab open. To test: open multiplayer.html in two browser tabs (both need
  internet), Create a room in one and Join with the code in the other. For friends on
  other devices, the file needs to be hosted at a web link (e.g. GitHub Pages).

## How the code is organized (all inside index.html)
1. CSS styles for the on-screen displays (timer, found counter, giggle meter, menus)
2. HTML for those displays and the start/end screens
3. JavaScript sections, in order, marked with `// ----------` comments:
   - Renderer/scene/camera setup (with shadows + ACES tone mapping)
   - Night lighting: moonlight, plus a SpotLight "flashlight" that follows the camera
   - Procedural canvas textures (grass, plaster walls, dirt paths)
   - Night sky dome, moon, stars, drifting clouds
   - Ground and dirt paths
   - Houses (walls with door openings, pitched roofs, glowing windows, door lights)
   - Bushes, trees, glowing lamp posts, fireflies
   - The monster character, `hasLineOfSight()` (can it see you past buildings/bushes),
     and `monsterSpawn()` which places it on the far side of town
   - Player movement + controls (WASD/arrows + mouse drag; touch joystick on mobile;
     F key or 🔦 button toggles the flashlight)
   - Collision detection (`wallBoxes` rectangles and `circles`)
   - Game state: `startGame()`, `endGame()`, timer, danger meter
   - The monster's "brain" (wander vs. chase) lives inside the main `animate()` loop
   - Main `animate()` loop

## How to test
Open index.html in a browser (double-click it). No build step, no server needed.

## Ideas the owner has mentioned or may want next
- Online multiplayer so real friends can hide/seek from their own devices
  (would require a server — that's a big step up from this single file)
- Sounds: footsteps, a growl that gets louder when the monster is close, a scare sting
  when caught
- More than one monster, or a monster that gets faster over time
- Difficulty levels (more time, slower monster = easier)
- Hiding spots you can climb into (inside a house) where the monster can't follow
- A bigger town to run and hide in
