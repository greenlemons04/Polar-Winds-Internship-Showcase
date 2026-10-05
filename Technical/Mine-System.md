# Mine System — Technical Breakdown

During my 2026 internship with Nexus Games, I designed and implemented a mine-based gameplay system for **Polar Winds**, a real-time multiplayer game built on the Atlas Arena platform.

The feature was developed inside an existing TypeScript multiplayer codebase and required changes across server gameplay logic, synchronized game state, client rendering, visual/audio feedback, scoring, and developer testing tools.

I first implemented a larger prototype of the system and later revised it based on gameplay and presentation feedback. The final repository state is represented by commit `1738b68`.

## Final Mine System

The final version of the game contains three mine types:

| Mine Type | Blast Behavior |
| --- | --- |
| **Square** | Clears a 3×3 area centered on the mine |
| **Horizontal** | Clears the mine's entire visible row |
| **Vertical** | Clears the mine's entire visible column |

The directional mine designs remain fixed in orientation so players can visually understand the direction of their blast before interacting with them.

## Color-Based Gameplay

Each mine is associated with one of the game's three player colors:

- Orange / RED
- White / GREEN
- Blue / BLUE

The mine system uses player perspective as part of the gameplay mechanic.

A player cannot see mines matching their own color while those mines are inactive. Mines belonging to the other player colors remain visible.

This means each player can see hazards that may threaten their teammates while their own hazards remain hidden from them.

Once a mine has been triggered, it becomes visible regardless of player perspective.

## Mine Triggering

Only a player whose color matches a mine can activate that mine.

When a mine is triggered:

1. The mine enters a triggered state.
2. Its blast pattern is calculated on the server.
3. Player-created line cells inside the blast area are removed.
4. A 5-point team penalty is applied.
5. The server broadcasts the mine event to connected clients.
6. The client displays an explosion effect and plays explosion audio.

The mine itself is not immediately removed from the board.

Instead, it remains in a visibly triggered state.

## Team Resolution Mechanic

Triggered mines can be resolved by another player.

If a player of a different color moves onto a triggered mine, the mine is resolved and its 5-point team penalty is refunded.

This creates a cooperative mechanic in which one player's hidden hazard can become something another teammate must respond to.

The mine then returns to its normal state rather than being permanently removed.

## Chain Reactions

Mine explosions can also trigger another mine caught inside their blast area.

The final implementation intentionally limits the chain reaction:

- The original mine can trigger one randomly selected mine inside its blast area.
- The chained mine performs its own full explosion and penalty behavior.
- The chained mine cannot continue the reaction into additional mines.

This keeps chain reactions unpredictable without allowing an unlimited cascade across the board.

## Stage-Based Spawning

Mine spawning scales as the game progresses.

Stage 1 contains no mines.

Stages 2–3 add one mine for each player color per stage.

Later stages add two mines for each player color per stage.

Existing mines remain on the board as new stages add additional hazards.

Mine type selection is also weighted by stage. Square mines are the most common early type, while horizontal and vertical directional mines become more likely as the game progresses.

Mine placement avoids:

- Occupied board positions
- Existing mines and actors
- A line whose color matches the mine being placed

A mine may still overlap another player's colored trail.

## Multiplayer Architecture

Mine behavior is controlled through the server rather than being handled only on the client.

Each mine is stored in synchronized game state with information including:

- Board position
- Unique ID
- Player color
- Mine type
- Triggered state
- Whether its score penalty is currently active

The client receives the synchronized mine state and uses it to determine rendering and visibility.

Gameplay events such as mine triggering, chain reactions, scoring changes, and mine resolution are processed by the server.

This allowed the mine system to remain consistent across connected players.

## Rendering and Feedback

The client-side mine system uses **React Three Fiber** and **Three.js** to render the mines as 3D objects.

Each mine type has a different physical silhouette so its blast behavior can be recognized visually.

Triggered mines receive additional feedback, including:

- Increased glow
- Faster movement and pulsing
- A bright animated ring
- Temporary explosion effects
- Explosion audio

Mine visibility was also optimized during the final revision.

Instead of repeatedly removing and recreating hidden mines when player perspectives change, mines remain mounted in the scene and their visibility is toggled.

This reduced unnecessary WebGL work when switching between player perspectives.

## Explosion Audio

I implemented client-side explosion feedback using the Web Audio API.

The effect combines:

- A low-frequency impact
- A short filtered noise burst
- Rapid volume and frequency falloff

The audio system also safely handles situations where browser audio has not yet been activated by user interaction so an audio failure does not interrupt gameplay.

## Developer Testing Tools

I also implemented internal development tools to make the mine system easier to test.

### Stage Skip

A developer stage-skip command advances the actual server game state rather than creating a client-only visual change.

This allowed me to quickly test mine behavior at later stages without replaying the full game.

**F8** activates the stage skip while running locally.

### Board Fill Tool

A second tool fills available board cells with normal player line cells.

This made it possible to quickly create a dense board and visually test:

- Square explosions
- Horizontal explosions
- Vertical explosions
- Line destruction
- Chain reactions

**F7** activates the board-fill tool while running locally.

These tools reduced the time required to reproduce and test later-game mine behavior.

## Room Randomization

As part of the final feedback revision, I also changed room initialization so a new random seed is generated for each game room unless a specific seed is intentionally supplied.

This prevented new solo and multiplayer sessions from repeatedly using the same fallback seed while still allowing an explicit seed to be provided when reproducible behavior is needed for testing.

## Iteration

The first major implementation experimented with a larger system containing six mine types:

- Square
- Horizontal
- Vertical
- Cross
- Diagonal
- Cluster

During development, the system was revised and the final build narrowed the mine set to three types:

**Square, Horizontal, and Vertical.**

Other mechanics and feedback systems were also revised during testing.

This process involved implementing the initial feature, testing it inside the existing game, evaluating gameplay and presentation feedback, and modifying the system before the final internship build.

## Contribution History

### Initial Implementation

[984909f — Add mine system and gameplay improvements](https://github.com/sabidmahmud01/polar-winds-standalone/commit/984909f)

This commit introduced the original mine system across the client and server.

### Final Revision

[1738b68 — Refine mine feedback and randomize room seeds](https://github.com/sabidmahmud01/polar-winds-standalone/commit/1738b68)

This is the final commit in the internship repository and represents the final project state.

## Technologies Used

- TypeScript
- React
- React Three Fiber
- Three.js
- Colyseus multiplayer state synchronization
- Web Audio API
- Git / GitHub
- Atlas Arena

## Project Context

Polar Winds was developed collaboratively by a five-person internship team.

This page focuses specifically on the mine-system implementation, development tools, and related revisions that I contributed during my internship.

[View the original Polar Winds team repository](https://github.com/sabidmahmud01/polar-winds-standalone)
