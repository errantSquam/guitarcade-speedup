# guitarcade-speedup
Hello! This is a WIP/proof of concept :] While the mod works, the result may be unstable and you may need to exit/re-enter your Guitarcade game (or even the whole game) to fix things.

**UPDATE: CHANGED TO XDELTA PATCHES FOR LEGAL REASONS. Please download xdeltaUI and patch them onto your .psarc files! Let me know if anything doesn't work. Will look into RSmod installer eventually.**

To use the mod, drag and drop the file into the guitarcade folder of your Rocksmith 2014 install. Please create a backup of the old file before you add your mod!
(You can also rename the old file to \[name].psarc.bak and remove the ".bak" part if you wish to revert changes.)

### MOD TYPES (String Skip only so far)
- old_no_scoreboard_unstable - The default for all the xdelta patches! Skips through the scoreboard. High score should be preserved, but no missions.
- outro-skip-stable - Only skips through the outro. Has a lower chance of glitching out the game, but you have to wait for the scoreboard to finish. Missions and high score work.
- outro-mash-through-leaderboard - Skips through the outro, GUI is the same, functionality preserved, but you can mash Start any time after the game ends to start a new one. May still have particle errors (e.g. string skip bullets don't work, need to leave and re-enter game)

No "idiot-proof" installation tutorial for now, sorry! Please report any bugs you experience in Issues (but no promises I'll get to it because I'm busy with life.)

High score updating not fully tested yet. (See: https://github.com/errantSquam/guitarcade-speedup/issues/1) Assume it won't be saved for now.

# Rough documentation for future modders:
The functionality for switching screens can be found in the unpacked .psarc files, under gamexblocks/guitarcade/(7z archive)/behaviors/guitarcade/esegcgameflow.lua. You can use the Song Creator Toolkit to unpack the files (just ignore the error messages)

I tend to edit the file directly within 7z because extracting and then re-compressing caused problems for me sometimes. 

Under the EndGame function, I comment out the high score logic, extract the relevant chunk for updating the high score, and place it outside. The BehaviorAPI reference to the Leaderboard is also removed (I assume that calls the GUI)

See below:

![image](https://github.com/user-attachments/assets/30623f16-f72f-464d-9acb-a9d68fc8e1c5)

