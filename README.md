# guitarcade-speedup
Hello! This is a WIP/proof of concept :] While the mod works, the result may be unstable and you may need to exit/re-enter your Guitarcade game (or even the whole game) to fix things.

You can download straight from the repo or access releases here: https://github.com/errantSquam/guitarcade-speedup/releases

## How to Install
**UPDATE: CHANGED TO XDELTA PATCHES FOR LEGAL REASONS. Please download xdeltaUI and patch them onto your .psarc files! Let me know if anything doesn't work. Will look into a proper installer eventually.**

**To use the mod**, go into the guitarcade folder of your Rocksmith 2014 install, find the relevant .psarc, and use the bundled xdeltaUI to patch your respective game. Please create a backup of the old file before you add your mod!
(You can also rename the old file to \[name].psarc.bak and remove the ".bak" part if you wish to revert changes.)

No "idiot-proof" installation tutorial for now, sorry! Please report any bugs you experience in Issues (but no promises I'll get to it because I'm busy with life.)


### MOD TYPES
- old_no_scoreboard_unstable - Skips through the scoreboard. High score should be preserved, but no missions.
- outro-skip-stable - Only skips through the outro. Has a lower chance of glitching out the game, but you have to wait for the scoreboard to finish. Missions and high score work.
- **scoreboard_skip (Recommended)** - Skips through the outro, GUI is the same, functionality preserved, but you can mash Start any time after the game ends to start a new one. May still have particle errors (e.g. string skip bullets don't work, need to leave and re-enter game)


See here for known issues: https://github.com/errantSquam/guitarcade-speedup/issues

Theoretically, high scores/missions should work, but assume the worst and use this mod more for practice than for chasing high scores.

# Rough documentation for future modders:
The functionality for switching screens can be found in the unpacked .psarc files, under gamexblocks/guitarcade/(7z archive)/behaviors/guitarcade/esegcgameflow.lua. You can use the Song Creator Toolkit to unpack the files (just ignore the error messages)

I tend to edit the file directly within 7z because extracting and then re-compressing caused problems for me sometimes. 

Currently, progress is being documented here: https://github.com/errantSquam/guitarcade-speedup/issues/3

## CHANGES IN THE CODE
Tick function - Handles state management and calls per tick in game.

### StartGame/ChangeStateStartGame Function:

Some extra g_gameover = false states being issued for stability purposes (I have no way of testing whether this improves things, but it feels like it does in my opinion). This is because the projectile manager for String Skip only runs when gameover is set to false.
![image](https://github.com/user-attachments/assets/36e99d27-2810-4693-9e82-dc0c9caa433a)

### (Tick) GameEnd State:
![image](https://github.com/user-attachments/assets/d3b5eab1-e93e-47da-bf17-6fe353683aa9)

Skips the outro call and goes straight to calling game end functionality by calling ChangeStateEndGame (normally called in outro state, which is skipped)

Removal of Various ActionScript Functions:

![image](https://github.com/user-attachments/assets/05a7db67-cead-43ed-98d6-4d273e374767)

These functions don't seem to affect GUI elements in any way but blocks the user from skipping through the scoreboard screen. Mission/Highscore data appears to be saved with these removed.

Also, removed GUI for leaderboard challenges because they're annoying and waste time.

Premature PressStart State:

![image](https://github.com/user-attachments/assets/fa19d5ad-81ae-4286-8706-953732962022)

Enabling this allows the user to "mash" through the scoreboard/mission GUI immediately after the game ends, and start a new game. All functionality (highscore update, missions, assigning rocksmith points, leaderboard challenges) appears to be preserved and already processed before the GUI is called. The player can also choose to watch these screens normally (like vanilla Rocksmith functionality)

### Random Logic in the EndGame function call:

![image](https://github.com/user-attachments/assets/2009983d-fb2c-4432-8fa9-f71d83820cd1)

Honestly not sure if these are necessary anymore but I haven't noticed any weird bugs with them commented out. Good to trim the fat.


## OLD DOCUMENTATION FOR SKIPPING SCOREBOARD COMPLETELY
Under the EndGame function, I comment out the high score logic, extract the relevant chunk for updating the high score, and place it outside.

See below:

![image](https://github.com/user-attachments/assets/30623f16-f72f-464d-9acb-a9d68fc8e1c5)

