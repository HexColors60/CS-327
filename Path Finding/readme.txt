dungeon_generation.c:

**1.01** VERSION 0.1-0.6

Thus far the program accomplishes the following in order:
     1. generates a 80x21 dungeon filled with rock
     2. based on the difficulty provided, randomly generates and places the
        rooms on random (x,y) coordinates in the 80x21 grid.
     3. draws corridors from room 1 to room n sequentially. (1-2 2-3..(n-1)-n)
     4. prints the screen at the end.

A couple notable functions are generate_rooms() & connect_rooms();

   generate_rooms():
	So this function has a couple special things happening at once. Firstly
	it generates a percentage of the room to be filled based on the diffic-
	ulty provided. It selects a random room to be placed inside the switch
	statement. It then loops through the array dungeon[21][80] and
	attempts to place these rooms through the is_room_placeable() function.

	Next, it performs a selection sort on the array of rooms once all the
	rooms have been placed properly.


   connect_rooms():
	This one was what took the most time for me. It starts by identifying
	whether the room next to the one being checked is either: 1- above it,
	2- below it, or 3- directly parallel to it. In case 1, it attempts to
	path up to the second rooms y coordinate + height/2, and then path
	right to the second rooms x coordinate at height/2. Similarly, case 2
	paths down and then right. Case 3 just paths right.

Want to change the global array rooms[60][4] to malloc at some later date because as of now, it's worst case senario (max difficulty * percentage/ 4x4 room).

**1.02** VERSION 0.7

I've added two main functions to the program: save_dungeon() and load_dungeon(). These operate just as they sound, and they perform based off of command line arguments. If the user prompts for a --save & --load it will always load it first and then save it. File format is as follows:
     BYTES     	    



Both functions save to the location: $HOME/User/.rlg327/

     save_dungeon():
	The gist of this is to read in a specified file format and convert it
	to a generated dungeon. The dungeon then can be loaded at a later time
	by knowing the name you saved it under.

     load_dungeon():
	The basis of this function is to load a dungeon based on a specified
	file in which the user passes in. It can either be a new dungeon, or a
	previously saved one.

Example run arguments:

1	./dungeon_generation
	
2	./dungeon_generation --save
	> name_of_file

3	./dungeon_generation --load
	> name_of_file
	
4	./dungeon_generation --load --save
	> name_of_file

**1.03** VERSION 0.8

I started this weeks portion of the project by splitting my one file into many files and naming them according to what they do. I have the following so far.

  1. dungeon_generation.c
  2. dungeon_generation.h
  3. main.c (unused)
  4. path_finding.c

There are other files that were provided for us.

  1. binheap.c
  2. binheap.h

I bridge all these together into a binary named RLG327 (Roguelike Game 327).

What I've done after that is I basically had to write a way to implement Djikstras Algorithm with path finding. On top of this I had to check the hardness of each cell and make the monster's movement be based on that and the distane to the PC. Also, since some monsters were able to tunnel and some weren't, I had to have that check in there as well. After that, my priority queue allowed the stack "decrease_key" in order to get the shortest distance to the PC.

I had to change the way I was generating my dungeon around a little bit as well. Previously I was using a global array, but now I'm using a struct which holds all the useful information I need to check in path_finding.c like hardness, tunnel ability/not, x, y, etc..
