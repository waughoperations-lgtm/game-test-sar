# Hide and Seek Town (night version)

## What this project is
A 3D hide-and-seek game that runs in a web browser. The player walks around a small
town at night with a flashlight, looking for 3 hidden characters (Pip, Momo, Zuzu)
who hide inside houses or behind bushes. There is a 2-minute timer and a "giggle
meter" that fills up as the player gets close to a hider.

## Important context about the owner
The owner is NOT a programmer. They know how to use a computer but not how to code.
When working with them:
- Explain everything in simple, non-technical language
- Never ask them to edit code themselves — make the changes for them
- After changes, tell them to open index.html in their browser to see the result

## Project structure
- `index.html` — the ENTIRE game in one file (HTML + CSS + JavaScript).
  It loads the Three.js 3D library (version r128) from a CDN, so an internet
  connection is needed the first time it loads.

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
   - The 3 hider characters and `pickSpots()` which chooses random hiding places
   - Player movement + controls (WASD/arrows + mouse drag; touch joystick on mobile)
   - Collision detection (`wallBoxes` rectangles and `circles`)
   - Game state: `startGame()`, `endGame()`, timer, giggle meter
   - Main `animate()` loop

## How to test
Open index.html in a browser (double-click it). No build step, no server needed.

## Ideas the owner has mentioned or may want next
- Online multiplayer so real friends can hide/seek from their own devices
  (would require a server — that's a big step up from this single file)
- Sounds: footsteps, giggles that get louder when close, catch fanfare
- Hiders that sneak to new hiding spots while the player isn't looking
- A mode where the player hides and a character seeks
- Bigger town, more hiders, difficulty levels
