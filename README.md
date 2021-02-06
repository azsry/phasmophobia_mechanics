# Phasmophobia Mechanics (Beta)

***This Guide is up to date as of Phasmaphobia build 6197011 released on Febuary 5th, 2021***

Thanks to the following people for spotting errors in this guide:
- AeonLucid
- DeceptivePastry
- Kyle2142
- u/Sowelu
- zendabbq

- [Phasmophobia Mechanics](#phasmophobia-mechanics)
      - [Compiled by Azsry](#compiled-by-azsry)
  - [General Gameplay](#general-gameplay)
    - [Difficulty levels](#difficulty-levels)
    - [Sanity](#sanity)
      - [Sanity effects](#sanity-effects)
  - [Evidence](#evidence)
    - [Dirty Water](#dirty-water)
    - [Freezing Temperatures](#freezing-temperatures)
    - [Ghost Orbs](#ghost-orbs)
    - [Ghost Writing](#ghost-writing)
    - [Spirit Box](#spirit-box)
  - [Ghost behaviour](#ghost-behaviour)
    - [Car](#car)
    - [Doors](#doors)
    - [Favourite Rooms](#favourite-rooms)
    - [Footsteps](#footsteps)
    - [Fusebox](#fusebox)
    - [Generic VOIP recognition](#generic-voip-recognition)
    - [Ghost activity](#ghost-activity)
      - [Ghost Powers](#ghost-powers)
        - [Banshee](#banshee)
        - [Jinn](#jinn)
        - [Phantom](#phantom)
        - [Poltergeist](#poltergeist)
        - [Wraith](#wraith)
    - [Ghost interactions](#ghost-interactions)
      - [Ghost Events](#ghost-events)
    - [Ghost phases](#ghost-phases)
      - [In truck](#in-truck)
      - [Setup phase](#setup-phase)
      - [Idle](#idle)
      - [Wander](#wander)
      - [Ghost appearing](#ghost-appearing)
      - [Hunting](#hunting)
    - [Window knocking](#window-knocking)
  - [Items](#items)
    - [Candles](#candles)
    - [Cameras](#cameras)
    - [EMF Reader](#emf-reader)
      - [Truck EMF Data](#truck-emf-data)
    - [Ouija Board](#ouija-board)
    - [Photo camera](#photo-camera)
    - [Salt](#salt)
    - [Smudge Sticks](#smudge-sticks)
    - [Sound Sensors / Parabolic Microphone](#sound-sensors--parabolic-microphone)
    - [Thermometer](#thermometer)
  - [Misc](#misc)
    - [Anti-Cheat](#anti-cheat)
    - [Insurance](#insurance)

## General Gameplay

### Difficulty levels

- Amateur is 0 difficulty
- Intermediate is 1 difficulty
- Professional is 2, or greater difficulty

### Sanity

- Game internally uses insanity, which is then displayed as sanity in the truck as `(100 - insanity) + Random.Range(-2, 3)`, but for the sake of clarity, we will refer to sanity.

- Small map
  - Normal drop rate: 0.12
  - Setup drop rate: 0.09
- Medium map
  - Normal drop rate: 0.08
  - Setup drop rate: 0.05
- Large map
  - Normal drop rate: 0.05
  - Setup drop rate: 0.03

- Solo games (that are not a tutorial)
  - Drop rates from above / 2

- Mechanics
  - Cannot go below 50 during setup phase
  - Decreases every frame by ((deltaTime * setup/normal drop rate) * difficulty rate (below))
  - Sanity does not decrease when you are in light or are outside the house
  - Light status updates every 2 seconds
  - Sanity level is synced to other players every 5 seconds
  - Being within 3m of a Jinn instantly decreases your sanity by 25%
  - Entering a recognised phrase into a Ouija Board has a 1-in-3 chance of decreasing your sanity by 40%
  - Sanity pills increase your sanity by 40%
  - Poltergeists decrease your sanity by double the number of props they are allowed to move
  - If the ghost is visible, or is in hunting phase, and is within 10m of the player, the player's sanity decreses by deltaTime (time since last frame) * sanity drain strength
  - Ghosts have a default sanity drain factor of 0.2
  - Phantom have a sanity draining strength of 0.4
- Average team sanity used for hunting phase and activity calculations consider everyone in the game, not just those in the house

#### Sanity effects
- The game will attempt to spawn ghosts at a window when the player is inside the house at a random interval between 10s and 30s if the player's sanity is below 50, otherwise at a random interval between 10s and 20s.
- This ghost will start far away from the window and dash towards the window until it is 0.2m away, then disappear

## Evidence
### Dirty Water
- Ghosts in range of a sink will have the same chance of interacting with the sink as it does with throwing or interacting with any other object.
- Ghosts leave a Ghost Interaction EMF (Level 2) on the sink

### Freezing Temperatures
- Ghosts that have freezing temperatures as an attribute will make the room they are in go down to -10C (14F), while all other ghosts make the room drop to 5C (41F). However, your thermometer has a +-2C randomness, so as soon as you see your thermometer go below 3C or 37.4F, or if you see cold puffs when you are in a room, you can tick off Freezing Temperatures.

### Ghost Orbs
- Ghost orbs only spawn in the ghost's favourite room, and can be seen in the doorway to that room from a hallway. This favourite room is designated on ghost spawn
- Ghost orbs can be seen before entering the house if a fixed camera is pointing at the door/hallway to their favourite room.

### Ghost Writing
- Ghosts have a 5-in-12 chance of doing random interactions such as opening doors, throwing props, and writing in ghost books.
- The ghost book does not appear to need to be in the ghost's room. Yet to be confirmed.

### Spirit Box
- Ghosts can only respond once every 10 seconds
- "Nothing detected" means the game recognised your phrase, but it triggered a fail check. However, fail checks and answers do not seem to be mutually exclusive.
- Ghosts that do not use spirit box will never respond
- Ghost responses only work if you are using Local Push to Talk (if you have push to talk enabled) or have a VR headset on. If you use Voice Activation, the ghost will always hear you.
- Ghosts will not respond if you are more than 3m away from them, or are on a different floor
- Ghosts that only respond to one person are shy
- Ghosts will not respond if the fusebox is on and a light switch in your current room is on
- Ghosts are more likely to respond to the Spirit Box if you have said their name in isolation (i.e. anger them with a phrase) recently
- Ghosts can respond with either location answers, difficulty answers, or age answers
    - Different location sounds are played depending on whether you are at least 4m away from the ghost
        - Difficulty questions:
            - What do you want?
            - Why are you here?
            - Do you want to hurt us?
            - Are you angry?
            - Do you want us here?
            - Shall we leave?
            - Should we leave?
            - Do you want us to leave?
            - What should we do?
            - Can we help?
            - Are you friendly?
            - What are you?
        - Location questions:
            - Where are you?
            - Are you close?
            - Can you show yourself?
            - Give us a sign
            - Let us know you are here
            - Show yourself
            - Can you talk?
            - Speak to us
            - Are you here?
            - Are you with us?
            - Anybody with us?
            - Is anyone here?
            - Anyone in the room?
            - Anyone here?
            - Is there a spirit here?
            - Is there a Ghost here?
            - What is your location?
        - Gender questions:
            - Are you a girl?
            - Are you a boy?
            - Are you male
            - Are you female
            - Who are you
            - What are you
            - Who is this
            - Who are we talking to
            - Who am I talking to
            - Hello
            - What is your name
            - Can you give me your name
            - What is your gender
            - What gender
            - Are you male or female
            - Are you a man
            - Are you a woman
        - Age questions:
            - How old are you,
            - How young are you,
            - What is your age,
            - When were you born,
            - Are you a child,
            - Are you old,
            - Are you young

## Ghost behaviour
### Car
- The car alarm can be turned on when the ghost is within 2m of the car. When the alarm is on, the car's headlights will also be on, and it will play some sounds. 
- To turn it off, you need the car keys and hit Use on the car. As soon as the car alarm turns on, a Ghost Interaction EMF (Level 2) spot will be created at the car's position.

### Doors
- As soon as a ghost with fingerprints touches a door, it will spawn the fingerprint evidence, but only on one side of the door. Fingerprint evidence does not spawn Ghost Interaction EMF
- Doors always lock during hunting phase, and will remain locked for a random amount of time between 10 and 20 seconds
- Doors all start with a locked timer between 5-20 seconds
- Ghosts make door knocking sounds when they are within 3m of the door, and create a Ghost Interaction EMF (Level 2) at the door's audio source location.
- Ghosts can close and lock doors randomly, which do NOT trigger Ghost Interaction EMFs.

### Favourite Rooms
- Ghosts stay in their favourite room before the front door is unlocked
- Upon entering Favourite Room state, the ghost will check to see if it is within 0-0.5m of its favourite room, and move towards a random location if it is
- The ghost will enter idle phase immediately after doing this distance check, regardless of its success.
- If ghosts are stuck in favourite room state for 30 seconds, either due to getting stuck or reaching its destination, it will return to idle phase.
### Footsteps
- Wraiths do not make black light footsteps after stepping on a salt pile
- Wraiths have an additional Wraith multiplier set to 50 after stepping in salt, making it less likely to start a hunting phase
- Ghost footsteps play at 5% volume during non-hunting and non-appearing phases
- When the ghost has appeared, footsteps are immediately played at full volume when in line of sight, then at 30% volume while you can still see the ghost
- During hunting phase, random hunting noises are played at full volume, then at 30% while you can still see the ghost
### Fusebox
- Small houses can have a max of 10 lights on
- Medium houses can have a max of 9 lights on
- Large houses can have a max of 8 lights on
- The ghost spawns a Ghost Interaction EMF (Level 2) when they use the fusebox
- Ghosts have a 1-in-5 chance of turning the fusebox back on

### Generic VOIP recognition
- Ghosts can react with your voice all throughout the game, even when you are not holding down a VOIP key
- You must be in the ghost room to trigger a response
- You must be in a room for more than 2 seconds for phrases to be recognised
- Shy ghosts will not respond to you if there are multiple people in the room
- Ghosts can react to their name at any time, even when you are not holding down a VOIP key, with a 20 second delay in between reactions
- Saying the ghost's name makes the ghost more likely to respond to the Spirit Box
- Saying just the ghost's first name is sufficient
- A generic ghost reaction will add a random value between 10 and 25 to their activity multiplier
- Generic reactions have a 1-in-200 chance of the ghost turning the fusebox off
- Generic reactions have a 1-in-8 chance of interacting with a random prop
- Generic reactions have a 1-in-8 chance of interacting with a random door

- Generic reaction phrases
  - "Fuck",
  - "Bitch",
  - "Shit",
  - "Cunt",
  - "Ass",
  - "Bastard",
  - "Motherfucker",
  - "Motherfucker", (yes, this is repeated. Probably indicattes a higher chance of them reacting to this)
  - "Arsehole",
  - "Crap",
  - "Pussy",
  - "Dickhead",
  - "Bloody Mary"
  - Plus 57 more that are language-specific. The English versions are provided below. For all langauges, refer to localisation.csv found in this repository, and search for Phrase_. All these are taken directly from the .csv, lack of punctuation, spelling errors, and all.
    - Can you speak
    - Show us
    - Let us know you are here
    - Do something
    - Is there anyone with me
    - Scream
    - Are you alone
    - Can we speak
    - would like to speak to you
    - Is there anyone here
    - May I ask you
    - Can you speak to us
    - Would you like to talk
    - Are you the only one here
    - Are you waiting
    - Is there anything that I can do
    - Do you know who we are
    - Are you happy
    - Are you here all the time
    - Are you male or female
    - Are there children here
    - Do you want us to leave
    - Make a noise
    - Can I ask you 
    - Can you make a sound
    - Show us your presence
    - Knock something
    - Make a sound
    - Open the door
    - Throw something
    - Talk to me
    - Talk to us
    - We mean you no harm
    - Open a door
    - We are friends
    - Is this you're home
    - Can you speak to us
    - I'm scared
    - I am scared
    - scared
    - scary
    - spooky
    - Horror
    - Scare
    - Open this door
    - Frighten
    - panic
    - Fright
    - Hide
    - Run
    - Show your presence
    - Show us
    - Show me
    - Turn on the light
    - Turn off the light
    - Are there any ghosts
    - Give us a sign
### Ghost activity
- Ghosts have random ability values set at the start of an idle phase, based on difficulty.
  - Amateur: 100
  - Intermediate: 115
  - Professional: 130
- Certain ghosts have their own multiplier that change depending on in-game events:
  - Oni multiplier: 30 if the ghost is an Oni and there are people in the room
  - Wraith multiplier: 50 if ghost is a Wraith and they have walked in salt
- Hunting multiplier
    - Starts at 0
    - Does not increase steadily over time
    - Decreases by 10 if ghost type is a Mare and a light switch in their current room is on
    - Increases by 10 if ghost type is a Mare and no lights in their current room are on, or there are no light switches in their current room
    - Increases by 15 if the ghost type is a Demon
- Ghost activity level
  - Starts at 0
  - Increases by 10 to 25 on phrase recognition
  - Increases by 20-30 if a smudge stick is used within 6m of it
  - Decays by deltaTime (time since last frame) / 2 every frame, capped at 0
- If a random number from 0 to \<random ability value\> (based on difficulty) is less than or equal to their current activity level, including all ghost-specific multipliers, and an additional 15% if playing solo, they have a chance of interacting with objects, or going to their favourite room, appear, use the fusebox, etc.
  - This means that a higher Ghost activity level, Oni/Wraith multiplier, and playing solo increases the chance of the ghost interacting with the house

- Ghosts can throw any prop around the house. A prop moving does not indicate the ghost being nearby.
Poltergeists throw objects with a random force between ((-5, 5), (-2.5, 2.5), (-3, 3)), while other ghosts throw them with a random force between ((-2.5, 2.5), (-2, 2), (-2.5, 2.5))

#### Ghost Powers
- All ghosts have a 4-in-12 chance of entering a ghost ability state.
- Ghosts that have specific behaviours (listed below) have a 2-in-12 chance of using their specific power on Amateur, and a 2-in-15 chance on other difficulties, once this state has been entered.
##### Banshee
- If the Banshee's current target is outside the house, it will not use its ability
- The Banshee will navigate to a spot close to the target player's position
- The Banshee will wait 20s. If the target player enters line of sight during this navigation time, the Banshee will go into hunting phase. If the current game is a tutorial, it will go back to idle.
- If the Banshee reaches its destination without spotting the target player, it will return to idle phase.
##### Jinn
- If the fusebox is off, Jinns will not enter their ability state, and will return to idle
- Once the Jinn enters their ability state, it will wait 5 seconds before using the ability.
- All players will have their sanity decrease by 25%
- A Ghost Interaction EMF (Level 2) will be created at the ghost's raycast point.
##### Phantom
- The Phantom will navigate to a random player's location
- The Phantom will create a Ghost Interaction (Level 2) EMF at its raycast point
- Once it reaches the random player's position, it will enter idle phase
##### Poltergeist
- If the Poltergeist has no props to interact with, it will return to idle phase
- If there are props to interact with, the Poltergeist will throw *all* of them with a random force of ((-4,4), (-2,2), (-4,4)), and create a Ghost Throwing (Level 3) EMF at the thrown prop's location.
- If the player was in line-of-sight of the EMF spot when it spawned, the player's sanity will decrease by 2x the number of props thrown
##### Wraith
- The Wraith will choose a random player
- If the chosen player is outside the house, or dead, the Wraith will return to idle phase
- If the chosen player is in the house and not dead, the Wraith will teleport to a spot within 3m of the chosen player, then return to idle phase
### Ghost interactions
- Ghosts have a 5-in-12 chance of doing random interactions such as opening doors, throwing props, and writing in ghost books, after satisfying the sanity check above
- Random ghost interactions, including sounds, turning on faucets, moving items, teleporting items, etc, do not generate EMF spots.
- Being within 3m of a Jinn instantly drops your sanity by 25%

#### Ghost Events
- Ghost events for the optional objective occur as part of random ghost interactions
- Ghosts have a 10-in-15 chance (which changes to 10-in-13 on higher difficulties) to enter a randomEvent phase
- After all these checks, it can either:
  - appear to the player, and create a Ghost Appeared (Level 4) EMF Spot
  - turn off light switches
  - slam doors shut, and create a Ghost Appeared (Level 4) EMF Spot
  - spawn a random player's model, play a scream if the spawned model or ghost is within 2m of the player
    - This one doesn't seem to be implemented, as the game contains code to select a random player model, but never uses it
- A ghost event will _increase_ a random player's sanity by 20% (not sure if this is intended)
### Ghost phases
Ghost phases are determined by average player sanity, multiplied by a hunting multiplier, and alternatively multiplied by an Oni or Wraith multiplier.
#### In truck
Ghosts stay in their favourite room until the main door has been  unlocked

#### Setup phase
Setup phase is when the timer in the truck is non-zero:
- Amateur has 5 minutes of setup phase
- Intermediate has 2 minute of setup phase
- Professional has 0 minutes of setup phase

- Ghosts cannot enter hunting phase during this time, but will instead be sent straight to their favourite room.
- "Hunting phase" starts immediately after setup phase, but this merely means regular phase, and not a true hunting phase
- Entering a recognised phrase into a Ouija Board has a 1-in-3 chance of ending setup phase

#### Idle
- All ghosts have an idle timer of 2-6 seconds, set when they return to an idle state
- Once their idle timer has elapsed, the ghost has a 1-in-2 chance of attempting to enter a hunting phase, depending on the team's average sanity and the current hunting multiplier.
  - If 50 > average player sanity + hunting multiplier > 25, the ghost has a 1-in-6 chance of entering hunting phase
  - If average player sanity + hunting multiplier <= 25, the ghost has a 1-in-4 chance of entering hunting phase
- If a random number generated between 0 and the ghost's random activity value (discussed in [Ghost Activity](#ghost-activity)) is greater than their current activity multiplier, the ghost has a 1-in-2 chance of doing an [interaction](#ghost-interactions), using their [ghost ability](#ghost-powers), or entering Wander phase.

#### Wander
- Ghosts can wander up to 3m from their current location at a time.
- Wraiths can occasionally teleport to the player's location, leaving a Ghost Interaction EMF (Level 2) at their new location

- If a smudge stick is within 6m of the ghost, it will:
  - add a random value between 20 and 30 to their activity multiplier, 
  - stop the ghost from hunting for 90s (or 180s for Spirits)
  - stop Yureis from wandering for 90s

- Banshees choose a new target when their current target dies, or if they leave the building. If no players are in the building, they will return to their favourite room.
  - Banshees choose the first player in the player list who is not dead
- Revenants move 1.5x slower than other ghosts when not chasing a player. When they are chasing a player, they are 2x faster than other ghosts.
- Jinns move at 2x Unity's default navmesh movement speed when you are more than 4m away from it, and the fusebox is on.

#### Ghost appearing
- Ghosts have a random chance of appearing only as a shadow, but only for alive players. Dead players will always see the full model.
- 1-in-2 chance if it is part of a random event. 1-in-3 chance as part of a hunt, appearance, or player kill

#### Hunting
- Hunting phases last for 25 seconds in Amateur, 35 seconds in Intermediate, and 50 seconds in Professional.

- When a hunting phase starts, the following will occur:
  - Ghosts pick a player to chase at the start of a hunting phase. This changes during a phase if the target player leaves the ghost's line of sight, or for non-Banshees, if another player steps closer to the ghost than the previous target player.
  - While chasing a player, the ghost uses one raytrace to check for line of sight, from its raycastPoint to the player's head position. This means crouching behind objects may prove useful.
  - While not chasing a player, ghosts cast two rays to check for line of sight
     - One ray directly from their raycastPoint (likely located near their head, but unknown at this time)
     - If the direct ray fails, one ray 0.167m to the right of their raycastPoint
     - If both of these raytraces fail, it will default to chasing the original target player
     - If the ghost is not a Banshee, it will cast two rays at each player to check for line of sight
        - One ray from their raycastPoint
        - One ray 0.01m to the right of their raycastPoint
     - If both of these raytraces fail, it will default to targeting the first non-dead player
  - A Ghost Appeared EMF (Level 3) reading will be created where they spawn
  - All entries to the house will be locked
  - If a crucifix is within 3m of the ghost (or 5m for banshees), the hunt will be cancelled, and the ghost will return to its favourite room.
  - If the ghost is a shade and there is more than 1 player in the room, hunting phase will not initiate, and the ghost will return to their favourite room.

- During hunting phases, the following occurs:
  - The ghost will start flickering the fusebox and all flashlights to indicate the successful start of a hunting phase (i.e. phase was not cancelled by anything listed above)
  - Phantoms are visible every 1-2 seconds, 0.3-1 seconds for all other ghost types.
  - If all players leave the ghost's line of sight, and the ghost is not a Banshee, it will start wandering around the house.

- The ghost will return to its favourite room when the player they are targeting is dead.

- If a smudge stick is within 1.5m of their teleport destination, they will return to their favourite room
- After killing a player, the ghost will teleport back to their favourite room, and reset to idle phase. They cannot initiate hunting phase for the next 25 seconds.
### Window knocking
- Ghosts can occasionally knock on windows. When they do, they create fingerprint evidence on the window, and create a Ghost Interaction EMF (Level 2) on the window


## Items
### Candles
- Candles can stay on for a random amount of time between 2.5 and 5 minutes. This value is determined when the candle is lit.
### Cameras
- If a ghost enters the view of a camera (i.e. the ghost is in the same room as the camera) when the fusebox is off, and no lights in that room are on, the ghost will turn off the camera and leave a Ghost Interaction EMF (Level 2) on the camera.

### EMF Reader
The EMF Reader tells you different things about the ghost depending on what level you detected:
- Level 1: Default
- Level 2: The ghost interacted with whatever something near you
  - Level 2 EMF spots have a 1-in-4 chance of showing as EMF Level 5, if your ghost has EMF Level 5 as a trait
- Level 3: The ghost recently threw something near you
  - Level 3 EMF spots have a 1-in-4 chance of showing as EMF Level 5, if your ghost has EMF Level 5 as a trait
- Level 4: The ghost appeared there recently
  - This happens at the beginning of hunting phases, and during random appearance events
- Level 5: Simply used for evidence

- EMF Spots only last for 20 seconds after the ghost has done something in that area, and will disappear after this time has elapsed. This will never change, regardless of whether the ghost is still in the room or not.
- The EMF reader does not actually work based on look direction. It simply gets activated based on its location, and whether it is colliding with any EMF triggers in the map. 
- Currently unknown how large these triggers are
- EMF reading levels can change while turning if the EMF reader enters another trigger which overlaps the current one, as the EMF reader just shows the level of the highest EMF zone that it overlaps
  
#### Truck EMF Data
- The truck's EMF graph shows the sum of all EMF spots in the house, capped at 10
### Ouija Board
- A player has to either wear a VR headset or use local push to talk if Local Push to Talk is enabled in settings. The ghost will always hear your voice if you use Voice Activation mode.
  - In solo games, Push to Talk is not needed, regardless of the menu setting
- Multiple people can be in the room when using the board
- A player must be within 5m of the board for it to work
- Entering a recognised phrase into a Ouija Board has a 1-in-3 chance of doing all of the following:
    - flickering all lights in the player's room
    - decreasing your sanity by 40%
    - Ending setup phase
- The ouija board responds to victim, age, dead, room amount, and location questions. The English questions are below. All languages can be found in localisation.csv found in this repository (Look for Ouija_ entries)
    - How old are you
    - What is your age
    - Are you old
    - Are you young
    - How long have you been dead
    - How many years ago did you die
    - How long have you been here
    - How long ago did you die
    - When did you die
    - Where are you
    - What is your favourite room
    - Where is your room
    - What is your room
    - Are you here
    - Are you close
    - Are there any spirits
    - Are you near
    - Are you around
    - How many are in this room
    - How many people are in this room
    - How many people are here
    - How many ghosts are in this room
    - How many ghosts are here
    - Are you alone
    - Are we alone
    - Who is here
    - Who is in this room
    - Who did you kill
    - Who is your victim
    - What is the name of the person you killed
    - What is the name of the person you murdered
    - What is your victim
    - Did you murder
    - Who did you murder
    - Who died
- All ghosts besides Demons will decrease your sanity by a random value between 5 and 10% upon receiving an answer
### Photo camera
- You must be within 5m of the evidence you are taking a photo of for it to count in your journal
- Photos of journal evidence are detected via a raycast from your camera's world position to the evidence's world position. For it to register properly in your journal, ensure no one is standing between the camera and the evidence.
- Photos of any evidence in the level count for your journal:
  - EMF spots
  - Ouija board
  - Fingerprints
  - Footsteps
  - DNA (Bone)
  - Ghosts
  - Dead bodies (You do not need to take a picture in order to be granted insurance)
  - Dirty water
- The journal can hold up to 10 photos. Additional photos will NOT count toward cash rewards.
- The game performs a simple ray cast from the camera's world position to the ghost's "raycastPoint", which may not be at its head. As a result, you should frame the entire ghost in your photos to complete the mission.
- Phantoms disappear after having their photo taken. This removes their sanity draining effect as well.
### Salt
- Ghosts only walk on salt spots once
- Ghosts do not affect salt spots during hunting phase

### Smudge Sticks
- When a smudge stick is used (i.e. starts smoking), the game checks if the smudge stick is within 6m of the ghost. If this is the case, the ghost will:
  - add a random value between 20 and 30 to their activity multiplier, 
  - stop the ghost from hunting for 90s (or 180s for Spirits)
  - halt a hunting ghost for 5 seconds 
  - stop Yureis from wandering for 90s

### Sound Sensors / Parabolic Microphone
- Sound sensors and parabolic microphones work in similar ways. Sound sensors provide data on the loudest sound source they detect, while parabolic microphones provide the sum of all sound sources it detects
- Both systems work based on intersection with Noise triggers placed around the map. These are variable-sized box colliders that have volumes attached to them
- As with the EMF Reader, these devices are NOT dependant on look angles, but uses the device's world position for intersection calculations. This means that the device may provide different readings while stationary and looking around if you are standing at the very border of two intersecting triggers.
- Sound sensor values are updated in the truck every 5 seconds
- Parabolic microphone values are updated at random intervals between 1 and 2 seconds
- Sound sources
  - Car
  - Door
  - EMF Reader
  - Spirit Box
  - Footsteps
  - Fuse box
  - Ghost throws
  - Camera shutters
  - Using a lighter
  - Motion sensors
  - Ouija boards
  - Sanity pills
  - Paintings falling
  - Player push to talk (local and global)
  - Salt
  - Sinks
  - Thermometers
  - Torches
### Thermometer
- As explained in [Freezing Temperatures](#freezing-temperatures), non-freezing temperature ghosts will drop a room down to 5C, and freezing temperature ghosts drop the room down to -10C. Your thermometer shows values +- 3C from the actual value, so if you see any values below 3C (37.4F), you have a ghost with Freezing Temperatures.
- The thermometer shoots a 6m ray from the bottom of the handle, not the actual front of the gun, and displays the temperature for the location of the first thing the ray hit
- If the 6m raycast fails, it will not update the temperature on the thermometer's display
- This is a visual representation of 6m in the game:
    ![](img/6m.jpg)

## Misc
### Anti-Cheat
- Err...the game just checks if you have over $250,000 and sets you back to $100. Seems legit.

### Insurance
- The game grants you insurance if you die during a game
- You are granted 1/2 the value of all items you brought into a contract when playing on Amateur, and 1/4 on Intermediate. There is no insurance on Professional.
- You do not need to take a photo of a Dead Body for the person to be granted insurance (in Multiplayer).