# Phasmophobia Mechanics (as of August 26th, 2021 'exposition' update)

- [Phasmophobia Mechanics](#phasmophobia-mechanics)
  - [Compiled by Mythra](#)
  - [General Gameplay](#general-gameplay)
    - [Leveling](#leveling)
    - [Difficulty Levels](#difficulty-levels)
    - [Sanity](#sanity)
    - [Setup Phase](#setup-phase)
  - [Evidence](#evidence)
    - [DNA Evidence](#dna-evidence)
    - [Ghost Orbs](#ghost-orbs)
    - [Freezing Temps](#freezing-temps)
  - [Ghosts](#ghosts)
    - [Ghost Types](#ghost-types)
    - [Spawning](#spawning)
  - [Process Detection](#process-detection)

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
  - The true sanity for a player is not ever displayed, instead it is random number in the range of: `(Sanity-2, Sanity+3)`
  - For players with zero/one percent sanity, the heart rate data represents treats the sanity as '2' so it can still display 0, but it doesn't have to do any fancy trickery dealing with negative numbers.

### Setup Phase

There is a clock in the truck that has a timer, this timer displays what is known as the 'Setup Timer'. The Setup Timer is a special time where players have time to collect, and set the things up they want to set up. Depending on your difficulty you'll get a different setup time.

<!-- SetupPhaseController__ctor -->
<!-- SetupPhaseController_Start -->
<!-- SetupPhaseController_Update -->

- For Amatuer Players you will have a setup time of: 4 minutes, and 59 seconds
- For Intermediate Players you will have a setup time of: 1 minute, and 59 seconds
- For Professional Players you will have no setup time.

## Evidence

### DNA Evidence

<!-- EvidenceController_SpawnAllGhostTypeEvidence -->
<!-- EvidenceController_u0927u0928u0929u0926u0921u0924u0925u091Bu091Du091Fu0927 -->

- DNA Evidence is what players commonly know as the 'bone'.
- The bone will spawn no matter what, it's position is chosen randomly by the host.
- This evidence doesn't actually help you catch the ghost, but it is listed here, because
  the game manages it like it is managed by the games Evidence Controller.

### Ghost Orbs

<!-- EvidenceController_SpawnAllGhostTypeEvidence -->
<!-- EvidenceController_u091Du091Cu0921u0920u091Cu0925u0927u0923u091Fu0929u091D -->

- Ghost Orbs are a type of evidence, for identifying certain types of ghosts.
  - Ghost Orbs are spawned in at the beginning of the game, and can be seen with a video camera.
  - Ghost Orbs will always spawn in the chosen 'ghost room', or the ghosts favorite room.
  - The Following Ghost Types are capable of spawning ghost orbs:
    - Banshee
    - Mare
    - Revenant
    - Yurei
    - Yokai
    - Hantu

### Freezing Temps

<!-- EvidenceController_SpawnAllGhostTypeEvidence -->
<!-- Thermometer_u091Du0929u0929u0923u0921u091Eu0926u0925u0928u0920u0921 -->
<!-- LevelRoom_Update -->
<!-- Thermometer_Update -->

- Freezing Temperatures are a type of evidence, for identifying certain types of ghosts.
  - Freezing Temperatures can be detected by using a themometer, or by waiting to see their breath visualize.
  - Thermometer will update every 2 seconds, except for the first update which is still 5 seconds
  - Temperature will not drop while the door is closed.
  - During a hunt if the player is within 10 meters of a ghost a random temperature will be display between 5-30 degrees celsius (41 - 86 fareinheit)
  - The temperature display on the termometer is not the real temperature of the room. Instead it is a random number in the range of: `(Real Temperature - 2, Real Temperature + 4)`
  - If the ghost is not in the room, and the fuse box is on the room will not change temperature, except to raise itself out of the minimum temperature allowed for the room.
    - The specific formula used is: `currentTempForRoom + (Random.Range(0.02, 0.05) * TimeSinceLastFrame)`
    - This is about ~0.6 degrees per call
  - If the ghost is not in the room, and the fuse box is off, the temperature will lower.
    - The specific formula used is: `currentTempForRoom - (Random.Range(0.04, 0.08) * TimeSinceLastFrame)`
    - This is about ~1 degrees per call
  - If the ghost is in the room, regardless of the fuse box the temperature will lower.
    - The specific formula used is: `currentTempForRoom - (0.5 * TimeSinceLastFrame)`
  - If the ghost is not a freezing ghost the room cannot go below 5 degrees celsius.
    - ***This means if you see a number below 3 degrees celsius/37.4 degrees Farenheit on your thermometer you know it is a freezing ghost.***
    - We have to use 3 degrees, as the thermometer may randomly take up to two degrees away.
  - If the ghost is a freezing ghost it will not go below -10 degrees celsius.
  - The following Ghost Types are capable of spawning Freezing Temps
    - Jinn
    - Revenant
    - Shade
    - Demon
    - Yurei
    - Oni
    - Hantu

## Ghosts

### Ghost Types

<!-- u091Eu0925u091Fu0929u0927u0923u0925u0925u0921u0929u0920_u091Eu091Du0925u0920u0924u0929u091Bu091Fu0925u0927u0925__Enum -->

There are 16 types of ghosts in the game, but one is just an 'empty' state for like when your sitting in a lobby:

- 0: none
- 1: Spirit
- 2: Wraith
- 3: Phantom
- 4: Poltergeist
- 5: Banshee
- 6: Jinn
- 7: Mare
- 8: Revenant
- 9: Shade
- 10 (0xA): Demon
- 11 (0xB): Yurei
- 12 (0xC): Oni
- 13 (0xD): Yokai
- 14 (0xE): Hantu
- 15 (0xF): Goryo
- 16 (0x10): Myling

### Spawning

<!-- GhostController_CreateGhost -->
<!-- GhostController_Start -->

- A ghosts spawns when a level is actually created after a contract has been selected, and the game is loading in.
- During Ghost Spawn the following things are randomized:
  - The Ghost Type is selected from a random range between (1, 17), we do not use choice '0' as that is a 'none' ghost.
  - Whether or not the Ghost is a male or a female.
  - The ghosts name (is chosen from a list based on if the ghost is male or female).

## Process Detection

<!-- u091Au091Du0922u091Au0923u091Eu0929u0922u091Du091Bu091F_u0927u091Eu091Bu091Bu0920u0926u091Du0927u091Cu091Eu0922 -->

Phasmophobia at certain points will look for certain process names running, and if they are running throw an exception. This feels like a really really bad form of anti-cheat/anti-debugging but it's incredibly ineffective.

Phasmophobia looks for processes the contain the following strings (case insensitively):

  - `cheatengine`
  - `sharpmonoinjector`
  - `fiddler`
  - `dnspy`
  - `ollydbg`