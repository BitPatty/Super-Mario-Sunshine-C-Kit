> Note: This repository was forked from MasterF0x whose GitHub Account disappeared and doesn't represent the code from the original repository 100% (it was about 8 commits ahead / 8 behind when the repository disappeared). If YOU happen to have a backup of the latest version from their original code please let me know.

# Super Mario Sunshine - C Kit

A C library for everyone's favorite game: Super Mario Sunshine. The tools in this will allow you to compile C code which can be patched into Super Mario Sunshine.

## What can this do?

The library allows you to compile PPC code that replace functions and interact with objects in Super Mario Sunshine. The code can be either put into a gecko code with a max size of 229 lines or be patched into Super Mario Sunshine's Start.dol.

## Neato!... Well, how do I set this up?

0. Clone or download the c kit.
1. Install devkitPro for PPC somewhere on your harddrive. The batch files are set up for it to be located at C:\devkitPro\devkitPPC, but you can edit the batch files to account for your install location.
2. If you want to patch Start.dol, extract it from your Super Mario Sunshine image and copy it into the c kit directory.
3. To compile C code, drag the source file onto build.bat for a gecko code or patchdol.bat to patch Start.dol.
4. The gecko code should be copied into your clipboard or a new .dol file with your source files name will be created.

## Alright. Do you have any examples to show?

Yes, actually. Here's an item spawning example from Miluaces:

```
include "sms.h"

int laststate; //Last state

int OnUpdate(MarDirector* director) {
	int (*GameUpdate)(MarDirector* director) = GetObjectFunction(director, Director_GameUpdate);

	MarioActor* mario = GetMarioHitActor();
	ItemActor* item = 0;

	if (!laststate){
		if (ControllerOne->buttons & PRESS_DU)	//Dpad up spawns a coin
			item = MakeObjAppear(*gpItemManager, OBJ_COIN);
		else if (ControllerOne->buttons & PRESS_DL) //Dpad left spawns a 1-up
			item = MakeObjAppear(*gpItemManager, OBJ_ONEUP);
		else if (ControllerOne->buttons & PRESS_DR) //Dpad right spawns a rocket nozzle
			item = MakeObjAppear(*gpItemManager, OBJ_ROCKETNOZZLE);
		else if (ControllerOne->buttons & PRESS_DD) //Dpad down spawns a turbo nozzle
			item = MakeObjAppear(*gpItemManager, OBJ_TURBONOZZLE);
	}

	// If no dpad buttons are pressed, reset laststate
	if (!(ControllerOne->buttons & (PRESS_DU | PRESS_DL | PRESS_DR | PRESS_DD)))
		laststate = 0;

	//If item was assigned, set it up
	if (item != 0){
		//Set laststate to 1 to prevent more than one item spawning from one button press
		laststate = 1;

		//Set item position
		item->position.x = mario->position.x;
		item->position.y = mario->position.y + 200.0f;
		item->position.z = mario->position.z;

		//Set item velocity
		item->velocity.y = 20.0f;

		//Make item moveable
		item->flags = item->flags & ~ITEMFLAG_STATIC;
	}

	return GameUpdate(director);
}
```

There are more examples in the repository.

## My code compiled! What do I do now?

Either build a new SMS image with your dol file or enter your gecko code to see your code run in game.
