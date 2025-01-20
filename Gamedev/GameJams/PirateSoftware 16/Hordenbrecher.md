# Hordenbrecher (working title)
## Design Document
*by redplanetcat for the 2023 Pirate Software Game Jam*

### Introduction
#### One-sentence pitch
Hordenbrecher is a tower defense-style game about a living spaceship trudging through hordes of enemies.
#### Inspirations
_Factorio: Space Age_
The latest DLC for Factorio features a new type of gameplay segment where you build on a space station that travels from one planet to the next, trying to maximize its defense by positioning enough guns and grabby arms to survive travel through an asteroid field.

_Lexx_
TV series Lexx features a living spaceship with a very unique personality and outlook on life. The show features a lot of dark moody visuals and body horror, which isn't very common for space operas.
#### Player Experience
The game has three stages featuring large enemy hordes. Between stages there's a section with an asteroid field which you also have to defend against, but it can bring you enough resources to build up and survive the next wave.
#### Platform
The game is developed for desktop web browsers and Windows/Linux PC, with possible later port for mobile touchscreen devices.
#### Development tools
- SDL3 as an application framework
- EnTT - library for Entity Component System
- Aseprite for graphics
- Korg Minilogue hardware synth, Ableton and Audacity for music and sound FX
#### Genre
Tower Defense, Casual
#### Target Audience
The game is leaning towards ***casual players*** and fans of darker Sci-Fi.
### Concept
#### Gameplay overview
The player controls placement of structures on a grid representing the spaceship. Each structure has associated cost of production. You gain resources for construction by capturing and processing asteroids floating toward the ship. Processing of asteroid ores requires its own special building.
The goal is to survive through three stages featuring large enemy hordes.
#### Theme Interpretation (`You Are The Weapon`)
You are the Ship. You were born to destroy, and you're doing the only thing you know: fly forward and make sure nothing stands in your path.
You hone the Weapon, Yourself, by surviving through arduous battles and building up on what's lacking: more firepower.
#### Primary Mechanics
| Structure        | Description                                                                                                | Damage    | Production                    | Upkeep    | Cost   | HP     | Sprite |
| ---------------- | ---------------------------------------------------------------------------------------------------------- | --------- | ----------------------------- | --------- | ------ | ------ | ------ |
| Sentry Gun       | Narrow cone of fire, fires single projectile                                                               | 10 DMG    | -                             | 0.01 MJ   | 10 CU  | 200 HP |        |
| Autogun          | Wider cone of fire, continuous fire                                                                        | 10 DMG    | -                             | 0.01 MJ   | 100 CU | 200 HP |        |
| Missile Launcher | Fires single guided missile, uses up Missiles as ammo                                                      | 100 DMG   | -                             | 0.05 MJ   | 50 CU  | 300 HP |        |
| Volley Launcher  | Fires a spread of 6 guided missiles, uses up Missiles as ammo                                              | 100 DMG   | -                             | 0.05 MJ   | 400 CU | 300 HP |        |
| Intake           | Gathers pieces of ore that drift directly into it                                                          | -         | -                             | -         | 50 CU  | 400 HP |        |
| Tractor          | Slowly nudges pieces of asteroid closer to itself                                                          | -         | -                             | 0.01 MJ/s | 200 CU | 200 HP |        |
| Armor Plating    | Fills up a single building spot to obstruct structures behind it from enemy fire                           | -         | -                             | -         | 50 CU  | 400 HP |        |
| Perimeter Shield | When activated, drains Electricity with great speed, but deals damage to all enemies that touch the shield | 100 DMG/s | -                             | 1 MJ/s    | 400 CU | 100 HP |        |
| Ore Processor    | Transforms gathered ore into Construction Units (CU)                                                       | -         | 10 Ore > 1 CU                 | 0.01 MJ/s | 20 CU  | 200 HP |        |
| Ore Storage      | Stores up to maximum 1000 Ore                                                                              | -         | -                             | -         | 50 CU  | 400 HP |        |
| Methane Burner   | Transforms gathered Methane crystals into Electricity                                                      | -         | 10 Methane > 1 MJ             | -         | 40 CU  | 200 HP |        |
| Methane Storage  | Stores up to maximum 10000 Methane                                                                         | -         | -                             | -         | 100 CU | 400 HP |        |
| Missile Assembly | Transforms Ore + Methane into a Missile                                                                    | -         | 1 Ore + 1 Methane > 1 Missile | 0.05 MJ/s | 100 CU | 200 HP |        |
| Missile Storage  | Stores up to maximum 100 missiles                                                                          | -         | -                             | -         | 200 CU | 400 HP |        |

| Enemy         | Description                                                    | Damage | HP      | Drops               | Sprite |
| ------------- | -------------------------------------------------------------- | ------ | ------- | ------------------- | ------ |
| Harpy         | Melee attacker that flies in a straight line                   | 10 DMG | 50 HP   | 1 Ore               |        |
| Drachen       | Melee attacker that flies in a straight line                   | 20 DMG | 100 HP  | 10 Ore, 1 Methane   |        |
| Drachenlord   | Melee attacker that flies in a straight line                   | 50 DMG | 1000 HP | 100 Ore, 10 Methane |        |
| Drachenritter | Ranged attacker that stays at a distance and fires at the ship | 20 DMG | 200 HP  | 50 Ore, 5 Methane   |        |
#### Secondary mechanics
_Intro cutscene_
"I am Hordenbrecher. I was raised on Planet Homunculus."
(slide with ship's front mouth mandibles grabbing human remains off a conveyor belt)
"The food was good." 
"Insurgents boarded me before I could mature."
"Now I am fleeting through the Galaxy of Peace, to the Galaxy of Terror, through the Unknowable Dark."
_Ending cutscene_
TBD
### Art
#### Theme Interpretation
TBD
#### Design
TBD, but probably lots of gore, reddish and brown hues.
### Audio
#### Music
TBD, but Zerg music from Starcraft and Lexx OST should probably be main inspirations.
#### Sound Effects
TBD
### Game Experience
#### UI
The interface would consist of a main viewport, with a control panel alongside its right edge, reminiscent of earlier C&C games.
The top right corner would hold information on selected structure. The right panel would let you select available structures to build.
To activate the Permieter shield, you either press the button on the construction itself, or press a brightly lit emphasized button on the right panel underneath the building selection. The button would remain inactive until the Perimeter structure is built.
#### Controls
All interactions are through mouse point'n'click interface.
### Development Timeline
#### Minimum Viable Product
| #   | Assignment          | Type   | Status      | Finish By | Notes                  |
| --- | ------------------- | ------ | ----------- | --------- | ---------------------- |
| 1   | Design Document     | Other  | In Progress | Jan 31    | It's a living document |
| 2   | Input system        | Coding | Completed   | Jan 20    |                        |
| 3   | Camera movement     | Coding | Completed   | Jan 20    |                        |
| 4   | Game UI             | Coding | Not Started | Jan 21    |                        |
| 5   | Structure placement | Coding | Not Started | Jan 22    |                        |
| 6   | Enemy AI            | Coding | Not Started | Jan 22    |                        |
| 7   | Structure damage    | Coding | Not Started | Jan 22    |                        |
| 8   | Structure functions | Coding | Not Started | Jan 23    |                        |
| 9   | Main menu           | Coding | Not Started | Jan 23    |                        |
| 10  | Ending              | Coding | Not Started | Jan 24    |                        |
| 11  | Audio system        | Coding | Not Started | Jan 24    |                        |
| 12  | Main theme          | Audio  | Not Started | Jan 25    |                        |
| 13  | Sound effects       | Audio  | Not Started | Jan 26    |                        |
| 14  | Structure sprites   | Art    | Not Started | Jan 27    |                        |
| 15  | Enemy sprites       | Art    | Not Started | Jan 27    |                        |
| 16  | Ship art            | Art    | Not Started | Jan 28    |                        |
| 17  | Background art      | Art    | Not Started | Jan 28    |                        |
| 18  | UI graphics         | Art    | Not Started | Jan 29    |                        |
| 19  | Special effects     | Art    | Not Started | Jan 29    |                        |
| 20  | Playtesting         | Other  | Not Started | Jan 30    |                        |
| 21  | SUBMIT              | Other  | Not Started | Jan 31    |                        |
#### Beyond
| #   | Assignment         | Type   | Status      | Notes |
| --- | ------------------ | ------ | ----------- | ----- |
| 1   | Starting slides    | Art    | Not Started |       |
| 2   | Ending slides      | Art    | Not Started |       |
| 3   | Camera shake       | Coding | Not Started |       |
| 4   | Settings menu      | Coding | Not Started |       |
| 5   | Saving and loading | Coding | Not Started |       |
