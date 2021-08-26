# Phasmophobia Mechanics (as of August 26th, 2021 'exposition' update)

- [Phasmophobia Mechanics](#phasmophobia-mechanics)
  - [Compiled by Mythra](#)
  - [General Gameplay](#general-gameplay)
    - [Leveling](#leveling)
    - [Difficulty Levels](#difficulty-levels)
    - [Sanity](#sanity)

## General Gameplay

### Leveling

<!-- See-able in many places, but can see in contract selection for example: Contract_SetDifficulty  -->
- Your level is just your total experience points divided by 100.
  - This means you 'level-up' every 100 experience points.

### Difficulty Levels

<!-- LevelSelectionManager_u091Du0921u091Bu091Fu0921u091Bu091Du0928u091Du0927u0927 -->
<!-- Contract_SetDifficulty -->

- Amatuer is internally represented as '0' difficulty
  - This difficulty is always unlocked
- Intermediate is internally represented as '1' difficulty
  - This difficulty is unlocked at level 10
- Professional is represented as '2' difficulty
  - This difficulty is unlocked at level 15

### Sanity

Internally the game has switched to 'sanity' in some places, and 'insanity' in others. To keep this entire section clear we will refer to it only as 'sanity'.

- Sanity can affect the game in two ways:
  <!-- SanityEffectsController_Update -->
  - Players with low sanity in the house may see ghosts running towards the window
    - this will happen once every 10 to 20 seconds if your sanity is below 50, otherwise every 10 to 30 seconds.
  - The ghost will respond to the group of players average sanity
    - Only dead players do not count for average sanity, house status has nothing to do with this.

Effects on Sanity (excluding ghost specialities which are down below):

<!-- PlayerSanity__ctor -->
<!-- PlayerSanity_Start -->

- Difficulty Modifier
  - If this is the tutorial: 0.5
  - If this is Amatuer: 1.0
  - If this is Intermediate: 1.5
  - If this is Professional: 2.0

- Map Size Modifier
  - If this is a small map:
    - If the map is in 'setup phase': 0.09
    - If the map is out of 'setup phase': 0.12
  - If this is medium map:
    - If the map is in 'setup phase': 0.06
    - If the map is out of 'setup phase': 0.08
  - If this is a large sized map:
    - If the map is in 'setup phase': 0.03
    - If the map is out of 'setup phase': 0.05

- Ghosts Seen Modifier is a default of 0.2 for ghosts (specials listed below)

- Single Player Non-Tutorial Modifier:
  - If you are playing alone not in the tutorial, the "Map Size Modifier" is halfed.

<!-- PlayerSanity_Update -->

- Sanity is synchronized every 5 seconds to other players.
- Sanity will update the lights every two seconds.
- Sanity will not go below 50 while the setup phase is happening.
- Sanity will decrease if you can see the ghost, and they are within 10m of a player by: `(timeSinceLastFrame * Ghosts Seen Modifier)`
- Sanity will decrease every frame by: `((timeSinceLastFrame * Map Size Modifier) * Difficulty Modifier)`

<!-- HeartRateData_u0921u091Cu0923u091Fu0929u091Au0922u091Du0920u091Du091A -->
- Sanity is displayed in the truck, the game calls this "Heart Rate Data"
  - The true sanity for a player is not ever displayed, instead it is a range of sanity from: `(Sanity-2, Sanity+3)`
  - For players with zero/one percent sanity, the heart rate data represents treats the sanity as '2' so it can still display 0, but it doesn't have to do any fancy trickery dealing with negative numbers.