# Phasmophobia Mechanics
#### Compiled by Azsry

## Anti-Cheat
Err...the game just checks if you have over $250,000 and sets you back to $100. Seems legit.

## EMF Reader
The EMF Reader tells you different things about the ghost depending on what level you detected:
- Level 1: Default
- Level 2: The ghost interacted with whatever you are looking at, or something near there
- Level 3: The ghost recently threw something near where you are looking. You can also use this information to narrow down your options to ghosts that interact with objects
- Level 4: The ghost appeared there recently
- Level 5: Simply used for evidence

EMF Spots only last for 20 seconds after the ghost has done something in that area, and will disappear after this time has elapsed. This will never change, regardless of whether the ghost is still in the room or not.

## Freezing Temperatures / Thermometer
Ghosts that have freezing temperatures as an attribute will make the room they are in go down to -10C (14F), while all other ghosts make the room drop to 5C (41F). As soon as you see your thermometer go below 5C or 41F, or if you see cold puffs when you are in a room, you can tick off Freezing Temperatures.

## Candles
Candles can stay on for a random amount of time between 2.5 and 5 minutes. This value is determined when the candle is lit.

## Car
The car alarm can be turned on when the ghost is within 2m of the car. When the alarm is on, the car's headlights will also be on, and it will play some sounds. To turn it off, you need the car keys and hit Use on the car. As soon as the car alarm turns on, a Ghost Interaction EMF (Level 2) spot will be created at the car's position.

## Cameras
If a ghost enters the view of a camera (i.e. the ghost is in the same room as the camera) when the fusebox is off, and no lights in that room are on, the ghost will turn off the camera and leave a Ghost Interaction EMF (Level 2) on the camera.

## Ghost activity
Ghosts have ability values set at the start of an idle phase, based on difficulty. If a random number is greater than or equal to their current activity level, including all ghost-specific multipliers, they have a chance of interacting with objects, or going to their favourite room, appear, use the fusebox, etc. 

## Ghost phases
Ghost phases are determined by average player sanity, multiplied by a hunting multiplier, and alternatively multiplied by an Oni or Wraith multiplier.
### In truck
Ghosts stay in their favourite room until the main door has been  unlocked
### Idle
### Hunting
When a hunting phase starts, the ghost will start flickering the fusebox and all flashlights.

During hunting phases, Phantoms appear for every 1-2 seconds, while all other ghost types appear every 0.3-1 seconds.

After killing a player, the ghost will teleport back to where it was just before the hunting phase began, and reset to idle phase. They cannot initiate hunting phase for the next 25 seconds.

## Doors
- As soon as a ghost with fingerprints touches a door, it will spawn the fingerprint evidence, but only on one side of the door. Fingerprint evidence does not spawn Ghost Interaction EMF
- Doors always lock during hunting phase, and will remain locked for a random amount of time between 10 and 20 seconds
- Doors all start with a locked timer between 5-20 seconds
- Ghosts make door knocking sounds when they are within 3m of the door, and create a Ghost Interaction EMF (Level 2) at the door's audio source location.
- Ghosts can close and lock doors randomly, which do NOT trigger Ghost Interaction EMFs.

## Ghost Orbs
Ghost orbs only spawn in the ghost's favourite room. This favourite room is designated on ghost spawn

## Spirit Box
- Ghost responses only work if you are using Local Push to Talk or have a VR headset on
- Ghosts will not respond if you are more than 3m away from them, or are on a different floor
- Ghosts that only respond to one person are shy
- Ghosts will not respond if the fusebox is on and a light switch in your current room is on
- Ghosts will not respond unless you say their name
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

## Ghost interactions
- Random ghost interactions, including sounds, turning on faucets, moving items, teleporting items, etc, do not generate EMF spots.
- Being within 3m of a Jinn instantly drops your sanity by 25%