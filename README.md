# Polar Winds — Nexus Games Internship Showcase

Portfolio showcase of my gameplay-programming and development-tool contributions to **Polar Winds**, a real-time multiplayer game developed on the Atlas Arena platform during my 2026 internship with Nexus Games.

**Role:** Game Development Intern  
**Dates:** June 2026 – August 2026  
**Team Size:** 5

> Polar Winds was developed collaboratively by an internship team.  
> This repository focuses specifically on the systems and tools I personally contributed and does not represent the entire project as my individual work.

![Polar Winds gameplay board](Images/OrangePlayerBoardPOV.png)

---

## My Contribution

My primary responsibility was designing and implementing a mine-based gameplay system inside the existing Polar Winds multiplayer codebase.

The final version of the system included:

- Three mine types: **Square, Horizontal, and Vertical**
- Server-controlled detonation logic
- Multiplayer-synchronized mine state
- Player-specific mine visibility
- Stage-scaled mine spawning
- Persistent mine states
- Chain reactions
- Team scoring and penalty behavior
- 3D mine rendering and explosion feedback
- Audio feedback
- Internal developer tools for faster testing

The system required work across both client and server code rather than being implemented as an isolated visual feature.

---

## Final Mine Types

| Mine Type | Behavior |
| --- | --- |
| **Square** | Clears a 3×3 area around the mine |
| **Horizontal** | Clears the mine's visible row |
| **Vertical** | Clears the mine's visible column |

During early development, I experimented with a larger six-type system. Testing and feedback later narrowed the final design to these three mine types.

---

## Multiplayer Gameplay

Mine behavior is integrated into the game's synchronized multiplayer state.

Each player has a color, and inactive mines matching that player's color are hidden from their perspective.

This creates different hazard information depending on which player is viewing the board.

Once a mine is triggered, it becomes visible and remains on the board in an activated state.

Mine detonation, penalties, chain reactions, and resolution are processed by the server so connected clients remain synchronized.

---

## Developer Tools

I also created development utilities to speed up testing and debugging.

These included:

- **Server-backed stage skipping** for quickly reaching later stages
- **Board-fill controls** for rapidly generating player lines
- Faster testing of mine explosions, line destruction, scoring, and chain reactions

These tools reduced the need to repeatedly play through earlier portions of the game when testing later-stage systems.

---

## Technical Breakdown

For a more detailed explanation of the mine system, including architecture, gameplay logic, screenshots, and selected code samples:

**[View the Mine System Technical Breakdown](Technical/Mine-System.md)**

---

## Technologies

- TypeScript
- React
- React Three Fiber
- Three.js
- Colyseus
- Web Audio API
- Git / GitHub
- Atlas Arena

---

## Development Process

This internship involved working inside an unfamiliar existing codebase rather than starting a project from scratch.

My work included:

- Learning the existing project architecture
- Implementing features across client and server systems
- Working with synchronized multiplayer state
- Building and testing gameplay systems locally
- Debugging issues through repeated gameplay testing
- Iterating on feedback
- Working within a shared Git-based development workflow

---

## Contribution History

### Initial Mine-System Implementation

[984909f — Add mine system and gameplay improvements](https://github.com/sabidmahmud01/polar-winds-standalone/commit/984909f)

This commit introduced the original mine-system implementation across the client and server.

### Final Feedback Revision

[1738b68 — Refine mine feedback and randomize room seeds](https://github.com/sabidmahmud01/polar-winds-standalone/commit/1738b68)

This was the final commit in the internship repository and contains my final mine-system feedback revisions and room-randomization changes.

---

## Testing and Iteration

I participated in both:

- Self-testing of my own features
- Cross-team playtesting with another internship team

Testing feedback was used to refine gameplay behavior, visual feedback, mine visibility, and other aspects of the system before the final presentation.

---

## Project Outcome

At the end of the internship, our team presented the completed standalone prototype to Nexus Games internship leads.

The mine system was demonstrated as part of the final project, and the internship leads expressed interest in further development and possible integration into the main game.

---

## Original Team Repository

The complete standalone team project can be viewed here:

[Polar Winds Standalone Repository](https://github.com/sabidmahmud01/polar-winds-standalone)
