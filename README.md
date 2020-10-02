# Phasmophobia Mechanics
#### Compiled by Azsry

- [Phasmophobia Mechanics](#phasmophobia-mechanics)
      - [Compiled by Azsry](#compiled-by-azsry)
  - [Anti-Cheat](#anti-cheat)
  - [Sanity](#sanity)
  - [Sanity effects](#sanity-effects)
  - [Difficulty levels](#difficulty-levels)
  - [EMF Reader](#emf-reader)
  - [Truck EMF Data](#truck-emf-data)
  - [Freezing Temperatures / Thermometer](#freezing-temperatures--thermometer)
  - [Candles](#candles)
  - [Car](#car)
  - [Cameras](#cameras)
  - [Ghost activity](#ghost-activity)
  - [Ghost phases](#ghost-phases)
    - [In truck](#in-truck)
    - [Setup phase](#setup-phase)
    - [Idle](#idle)
    - [Wander](#wander)
    - [Ghost appearing](#ghost-appearing)
    - [Hunting](#hunting)
  - [Doors](#doors)
  - [Dirty Water](#dirty-water)
  - [Ghost Orbs](#ghost-orbs)
  - [Spirit Box](#spirit-box)
  - [Favourite Rooms](#favourite-rooms)
  - [Footsteps](#footsteps)
  - [Fusebox](#fusebox)
  - [Ghost Writing](#ghost-writing)
  - [Ghost interactions](#ghost-interactions)
  - [Generic VOIP recognition](#generic-voip-recognition)
  - [Ouija Board](#ouija-board)
  - [Photo camera](#photo-camera)
  - [Salt](#salt)
  - [Window knocking](#window-knocking)
  - [Sound Sensors / Parabolic Microphone](#sound-sensors--parabolic-microphone)

## Anti-Cheat
- Err...the game just checks if you have over $250,000 and sets you back to $100. Seems legit.

## Sanity
- Game internally uses insanity, which is then displayed as sanity in the truck as (100 - insanity) + Random.Range(-2, 3)

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

- Insanity
  - Capped at 50 during setup phase
  - Uncapped to 100 during regular play
  - Increases every frame by ((deltaTime * setup/normal drop rate) * difficulty rate (below))
  - Insanity does not increase when you are in light or are outside the house
  - Light status updates every 2 seconds
  - Insanity level is synced to other players every 5 seconds
  - Being within 3m of a Jinn instantly increases your insanity by 25%
  - Entering a recognised phrase into a Ouija Board has a 1-in-3 chance of increasing your insanity by 40%
  - Sanity pills decrease your insanity by 40%
  - Poltergeists increase your insanity by double the number of props they are allowed to move
- If the ghost is visible, or is in hunting phase, and is within 10m of the player, the player's insanity raises by deltaTime (time since last frame) * sanity drain strength
    - Ghosts have a default sanity drain factor of 0.2
    - Phantoms have a sanity draining strength of 0.4

## Sanity effects
- The game will attempt to spawn ghosts at a window when the player is inside the house at a random interval between 10s and 30s if the player's insanity is above 50, otherwise at a random interval between 10s and 20s.
- This ghost will start far away from the window and dash towards the window until it is 0.2m away, then disappear

## Difficulty levels
- Tutorial is 0.5 difficulty
- Amateur is 1 difficulty
- Intermediate is 1.5 difficulty
- Professional is 2 difficulty

## EMF Reader
The EMF Reader tells you different things about the ghost depending on what level you detected:
- Level 1: Default
- Level 2: The ghost interacted with whatever something near you
- Level 3: The ghost recently threw something near you
- Level 4: The ghost appeared there recently
- Level 5: Simply used for evidence

- EMF Spots only last for 20 seconds after the ghost has done something in that area, and will disappear after this time has elapsed. This will never change, regardless of whether the ghost is still in the room or not.
- The EMF reader does not actually work based on look direction. It simply gets activated based on its location, and whether it is colliding with any EMF triggers in the map. 
- Currently unknown how large these triggers are
- EMF reading levels can change while turning if the EMF reader enters another trigger which overlaps the current one, as the EMF reader just shows the level of the highest EMF zone that it overlaps

## Truck EMF Data
- The truck's EMF graph shows the sum of all EMF spots in the house, capped at 10

## Freezing Temperatures / Thermometer
- Ghosts that have freezing temperatures as an attribute will make the room they are in go down to -10C (14F), while all other ghosts make the room drop to 5C (41F). As soon as you see your thermometer go below 5C or 41F, or if you see cold puffs when you are in a room, you can tick off Freezing Temperatures.

- Your thermometer shows values +- 2C from the actual value, so if you see any values below 3C (37.4F), you have a ghost with Freezing Temperatures.
Your thermometer displays the temperature for the location 6m in front of you

## Candles
- Candles can stay on for a random amount of time between 2.5 and 5 minutes. This value is determined when the candle is lit.

## Car
- The car alarm can be turned on when the ghost is within 2m of the car. When the alarm is on, the car's headlights will also be on, and it will play some sounds. 
- To turn it off, you need the car keys and hit Use on the car. As soon as the car alarm turns on, a Ghost Interaction EMF (Level 2) spot will be created at the car's position.

## Cameras
- If a ghost enters the view of a camera (i.e. the ghost is in the same room as the camera) when the fusebox is off, and no lights in that room are on, the ghost will turn off the camera and leave a Ghost Interaction EMF (Level 2) on the camera.

## Ghost activity
- Ghosts have ability values set at the start of an idle phase, based on difficulty. If a random number is greater than or equal to their current activity level, including all ghost-specific multipliers, they have a chance of interacting with objects, or going to their favourite room, appear, use the fusebox, etc. 

- Amateur: 100
- Intermediate: 115
- Professional: 130

- Oni multiplier: 30 if the ghost is an Oni and there are people in the room
- Wraith multiplier: 50 if ghost is a Wraith and they have walked in salt

- Hunting multiplier
    - Starts at 0
    - Does not increase steadily over time
    - Decreases by 10 if ghost type is a Mare and a light switch in their current room is on
    - Increases by 10 if ghost type is a Mare and no lights in their current room are on, or there are no light switches in their current room
    - Increases by 15 if the ghost type is a Demon

- If 50 < average player insanity + hunting multiplier < 75, the ghost has a 1-in-5 chance of entering hunting phase
- If average player insanity + hunting multiplier >= 75, the ghost has a 1-in-3 chance of entering hunting phase

- Ghosts can throw any prop around the house. A prop moving does not indicate the ghost being nearby.
Poltergeists throw objects with a random force between ((-5, 5), (-2.5, 2.5), (-3, 3)), while other ghosts throw them with a random force between ((-2.5, 2.5), (-2, 2), (-2.5, 2.5))

## Ghost phases
Ghost phases are determined by average player sanity, multiplied by a hunting multiplier, and alternatively multiplied by an Oni or Wraith multiplier.
### In truck
Ghosts stay in their favourite room until the main door has been  unlocked

### Setup phase
Setup phase is when the timer in the truck is non-zero:
- Amateur has 5 minutes of setup phase
- Intermediate has 2 minute of setup phase
- Professional has 0 minutes of setup phase

- Ghosts cannot enter hunting phase during this time, but will instead be sent straight to their favourite room.
- "Hunting phase" starts immediately after setup phase, but this merely means regular phase, and not a true hunting phase
- Entering a recognised phrase into a Ouija Board has a 1-in-3 chance of ending setup phase

### Idle
All ghosts have an idle timer of 2-6 seconds, set when they return to an idle state

Once their idle timer has elapsed, they have a chance of entering hunting phase or interaction phase, depending on the team's average insanity and the current hunting multiplier.

### Wander
- Ghosts can wander up to 3m from their current location at a time.
- Wraiths can occasionally teleport to the player's location, leaving a Ghost Interaction EMF (Level 2) at their new location

- If a smudge stick is within 6m of the ghost, it will:
  - add a random value between 20 and 30 to their activity multiplier, 
  - stop the ghost from hunting for 90s (or 180s for Spirits)
  - stop Yureis from wandering for 90s

- Banshees choose a new target when their current target dies, or if they leave the building. If no players are in the building, they will return to their favourite room.
- Revenants move 1.5x slower than other ghosts when not chasing a player. When they are chasing a player, they are 2x faster than other ghosts.
- Jinns move at 2x Unity's default navmesh movement speed when you are more than 4m away from it, and the fusebox is on.

### Ghost appearing
- Ghosts have a random chance of appearing only as a shadow, but only for alive players. Dead players will always see the full model.

### Hunting
- Ghosts can teleport between 2 and 15m at the start of a hunting phase
- Ghosts pick a player to target and chase at the start of a hunting phase. This does not change during the phase.
- When a hunting phase starts, the ghost will start flickering the fusebox and all flashlights, an Ghost Appeared EMF (Level 3) reading will be created where they spawn, and all entries to the house will be locked.
- During hunting phases, Phantoms appear every 1-2 seconds, while all other ghost types appear every 0.3-1 seconds. Hunting phases last for 25 seconds in Amateur, 35 seconds in Intermediate, and 50 seconds in Professional.
- If the ghost is a shade and there is more than 1 player in the room, hunting phase will be cancelled, and the ghost will return to their favourite room.
- The ghost will return to its favourite room when the player they are targeting is dead.
- If a crucifix is within 3m of the ghost (or 5m for banshees), the crucifix will be used, and the ghost will be sent back to their favourite room.
- If a smudge stick is within 1.5m of their teleport destination, they will return to their favourite room
- After killing a player, the ghost will teleport back to where it was just before the hunting phase began, and reset to idle phase. They cannot initiate hunting phase for the next 25 seconds.

## Doors
- As soon as a ghost with fingerprints touches a door, it will spawn the fingerprint evidence, but only on one side of the door. Fingerprint evidence does not spawn Ghost Interaction EMF
- Doors always lock during hunting phase, and will remain locked for a random amount of time between 10 and 20 seconds
- Doors all start with a locked timer between 5-20 seconds
- Ghosts make door knocking sounds when they are within 3m of the door, and create a Ghost Interaction EMF (Level 2) at the door's audio source location.
- Ghosts can close and lock doors randomly, which do NOT trigger Ghost Interaction EMFs.

## Dirty Water
- Ghosts will use a tap and spawn dirty water as soon as they enter a bathroom
- Ghosts leave a Ghost Interaction EMF (Level 2) on the sink

## Ghost Orbs
- Ghost orbs only spawn in the ghost's favourite room, and can be seen in the doorway to that room from a hallway. This favourite room is designated on ghost spawn
- Ghost orbs can be seen before entering the house if a fixed camera is pointing at the door/hallway to their favourite room.

## Spirit Box
- Ghosts can only respond once every 10 seconds
- "Nothing detected" means the game recognised your phrase, but it triggered a fail check. However, fail checks and answers do not seem to be mutually exclusive.
- Ghosts that do not use spirit box will never respond
- Ghost responses only work if you are using Local Push to Talk or have a VR headset on
- Ghosts will not respond if you are more than 3m away from them, or are on a different floor
- Ghosts that only respond to one person are shy
- Ghosts will not respond if the fusebox is on and a light switch in your current room is on
- Ghosts will not respond if you have said their name in isolation (i.e. anger them with a phrase) recently
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

## Favourite Rooms
Ghosts stay in their favourite room for 30 seconds, then return to their idle phase

## Footsteps
- Wraiths do not make footsteps in salt
- Footsteps are rendered during hunting phases
- Ghost footsteps play at 5% volume during non-hunting and non-appearing phases
- When the ghost has appeared, footsteps are immediately played at full volume when in line of sight, then at 30% volume while you can still see the ghost
- During hunting phase, random hunting noises are played at full volume, then at 30% while you can still see the ghost

## Fusebox
- Small houses can have a max of 10 lights on
- Medium houses can have a max of 9 lights on
- Large houses can have a max of 8 lights on
- The ghost spawns a Ghost Interaction EMF (Level 2) when they use the fusebox
- Ghosts have a 1-in-5 chance of turning the fusebox back on

## Ghost Writing
- Ghosts have a 1-in-3 chance to write in a spirit book
- Ghosts instantly write in the spirit book if they are willing to

## Ghost interactions
- Random ghost interactions, including sounds, turning on faucets, moving items, teleporting items, etc, do not generate EMF spots.
- Being within 3m of a Jinn instantly drops your sanity by 25%

## Generic VOIP recognition
- Ghosts can react with your voice all throughout the game, even when you are not holding down a VOIP key
- You must be in the ghost room to trigger a response
- You must be in a room for more than 2 seconds for phrases to be recognised
- Shy ghosts will not respond to you if there are multiple people in the room
- Ghosts can react to their name at any time, even when you are not holding down a VOIP key, with a 20 second delay in between reactions
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




## Ouija Board
- A player has to either wear a VR headset or use local push to talk, and be alone in a room for the ouija board to work
  - Appears to still work with multiple people in the room if any player is currently using local push to talk
- A player must be within 5m of the board for it to work
- Entering a recognised phrase into a Ouija Board has a 1-in-3 chance of doing all of the following:
    - flickering all lights in the player's room
    - increasing your insanity by 40%
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
- All ghosts besides Demons will increase your insanity by a random value between 5 and 10% upon receiving an answer


## Photo camera
- You must be within 5m of the evidence you are taking a photo of for it to count in your journal
- Photos of journal evidence are detected via a raycast from your camera's world position to the evidence's world position. For it to register properly in your journal, ensure no one is standing between the camera and the evidence.
- Photos of any evidence in the level count for your journal:
  - EMF spots
  - Ouija board
  - Fingerprints
  - Footsteps
  - DNA (Bone)
  - Ghosts
  - Dead bodies
  - Dirty water
- The game performs a simple ray cast from the camera's world position to the ghost's "raycastPoint", which may not be at its head. As a result, you should frame the entire ghost in your photos to complete the mission.
- Phantoms disappear after having their photo taken. This removes their sanity draining effect as well.

## Salt
- Ghosts only walk on salt spots once
- Ghosts do not affect salt spots during hunting phase

## Window knocking
- Ghosts can occasionally knock on windows. When they do, they create fingerprint evidence on the window, and create a Ghost Interaction EMF (Level 2) on the window

## Sound Sensors / Parabolic Microphone
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