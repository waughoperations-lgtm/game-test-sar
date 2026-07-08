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
  It loads the Three.js 3D library (version r128) from a CDN, plus a few Three.js
  post-processing add-ons (EffectComposer/UnrealBloomPass/etc.) for the "High" graphics
  mode. An internet connection is needed the first time it loads. If the add-ons fail to
  load, the game still runs — it just falls back to plain rendering.
- `multiplayer.html` — an online multiplayer version (separate file, single-player
  is untouched). Friends join a room by code; one player is randomly the monster and
  hunts the others; hiders survive 2 minutes to win. Networking is peer-to-peer over
  WebRTC via PeerJS (loaded from a CDN) using its free public broker — no server of
  our own. One player is the "host" and acts as the referee/relay, so the host must
  keep their tab open. To test: open multiplayer.html in two browser tabs (both need
  internet), Create a room in one and Join with the code in the other. For friends on
  other devices, the file needs to be hosted at a web link (e.g. GitHub Pages).
- `dominoes.html` — a separate online "Domino Night" mode. Classic draw dominoes
  (double-six set) for 2–4 friends around a 3D table in the town, each seated with a
  floating name tag. Includes push-to-talk voice chat (hold SPACE / the on-screen
  button) over WebRTC. Same PeerJS host-authoritative model as multiplayer.html: the
  host deals/validates turns and relays state; voice is a peer-to-peer audio mesh
  (`setupVoice`/`callEveryone`, mic muted until push-to-talk). The 3D scene is the
  hangout (table, seats, name tags, voice); the tile hand + played board are a 2D
  overlay. Dominoes engine + host validation: `fullSet`/`startRound`/`hostTryPlay`/
  `hostTryDraw`/`hostTryPass`/`finishRound`. Mic needs the browser's permission prompt.
  A "Practice vs computer" button (`practiceBtn`) starts a solo game against simple
  bots (`botAct`/`scheduleBots`) — no second player, no mic, no internet needed. The
  host re-renders its own hand/board via `renderGame()` inside `sendGameState`.
  Played tiles also appear as real 3D dominoes on the felt (`makeTile3D`/`tileTexture`/
  `updateBoard3D`, `boardTiles3D` map) and animate dropping in with a wooden "clack"
  (`playClack`). The night scene is alive: drifting clouds, fireflies, flickering
  house/table lights (`flickerLights`/`flick`), a swaying hanging lamp, and seated
  players that idly breathe — all driven in the `animate()` loop. Tiles are sized to
  fit the felt (`updateBoard3D` slot/scale). A 🔍 Overhead-view toggle (`viewBtn`, or
  the `V` key, `topView`) switches between the seated first-person camera and a
  top-down view of the board; the hanging lamp is hidden while overhead.

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
   - Procedural sound via the Web Audio API (`initAudio`, `setGrowl`, `thump`,
     `footstep`, `sting`) — a growl + heartbeat that rise with danger, footsteps,
     and chase/caught/win stings. All generated in code, so NO sound files are needed.
   - Graphics: drifting ground mist (sprites), floating dust motes, and post-processing
     (`setupPost`) — bloom "glow", plus a film-grain + vignette shader pass. A Low/High
     quality toggle (`applyQuality`/`toggleQuality`, the ✨ Graphics button or the `g` key)
     controls bloom, mist amount, shadow resolution and pixel ratio; defaults to Low on
     phones. Bump maps on the ground and walls add surface relief.
   - Optional gun: a glowing pickup (`gunPickup`/`gunSpawn`) appears at a random spot
     each round. Walk over it to equip (`equipGun`, limited ammo); Space or the 🔫 button
     fires (`shoot`) — a raycast against the monster. Hits `hitMonster` stun + knock it
     back (`monster.state = 'stunned'`); `HITS_TO_WIN` hits drive it off for a win
     (`endGame(true, 'drove')`). Gunshot/hurt sounds included.
   - The monster's "brain" (wander vs. chase vs. stunned/dead) lives in the `animate()` loop
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
