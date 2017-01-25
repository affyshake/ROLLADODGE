# ROLLADODGE
coursework_2
Dodge a Roll
 


Contents

•	Game and AI description
•	Game code description
•	AI code description
•	User guide and System Requirements






Game and AI description
The game is called Dodge a Roll. The player controls a ball inside a rectangular arena. Every 20 seconds, a cube spawns. The goal of the player is to move the ball with the arrow keys and keep avoiding the cubes for 2 minutes. The spawning cubes will have 1 out of 3 random AIs placed in them. The first AI type moves randomly around the arena. The second AI type follows the player’s position. Finally, the third AI type moves towards the future position of the ball, taking into consideration the ball’s current speed and position. If the ball collides with any cube, the game resets to the beginning. If the player manages to avoid the cubes for 2 minutes, the application closes as the game has finished.




















Game code description

 
This is the code for the GameManager script, the entity that controls the game. This first snippet shows the attributes that are going to be used:
•	player: The prefab that is going to be used as the player character
•	startPoint: The starting point of the player
•	spawnPoints: The points from which the cubes spawn
•	cubes: The possible cubes that can spawn. There are three prefabs, corresponding to each type of AI
•	timeLimit: The amount of time the game will last. It is set as 120 seconds in the editor
•	spawnTime: The amount of seconds that must pass for each new spawn. It is set to 20 in the editor
•	currentSpawnTime: Variable used to keep track of how many seconds have passed since the last spawn
•	currentTime: Variable used to keep track of game time passed
•	currentCubes: This is a list that stores the cubes currently in play
 
Next we have the Start and Update methods. The Start method initializes the current time and the list of cubes. It also spawns the player character and one cube. The Update method first checks if time is up. If it is, it calls the EndGame method. If time is not up, it checks if the time passed since the last spawn is greater than the actual spawn time. If it is, a new cube is spawned. Finally, the Update method adds the time passed since the last frame to our time variables, so time can be tracked. Notice that the time passed since the last spawn does not seem to ever be reset. This is because it is set to 0 once a new cube is spawned
 
Finally, we have the support methods. SpawnCube simply spawns one of the possible cubes at one of the possible spawn points at random, adds the newly spawned cube to the cube list and then sets the time passed since a cube spawned to 0. EliminateCubes destroys every cube from the list of cubes and empties the list. Reset reloads the current scene so the game can start again. EndGame closes the game.



AI code description
 
Before explaining the cube AI scripts, it is important to explain the ball’s code, written as the BallController script. It starts by getting a reference of the ball’s rigidbody and a reference to the active game manager. On every update step, the ball’s speed is set to the value of the directional input axes multiplied by the speed. The speed can be set in the editor. Finally, the ball constantly checks for collisions with other cubes. If the ball collides with an object tagged as “Cube”, it calls the game manager’s Reset method.
 
This is the code for the cube that moves randomly. The inspector variables available are the speed, and two values to determine how long the cube moves in a single direction before changing it. The private variables are as follows:
•	dx and dz: These arrays, when taken with the same index, describe a direction the cube is moving towards. Together, these arrays describe 8 possible directions.
•	currentSwitchTime: The time left before a direction switch happens
•	currentDir: The current index to use when getting directions from dx and dz
The Start method simply obtains the reference to the rigidbody. The Update method checks if enough time has passed since the last switch. If positive, the time to wait for the next switch is chosen randomly between minSwitchTime and maxSwitchTime. currentDir is also randomly set to one of the array’s position. Finally, the velocity of the cube is set, using currentDir to determine the direction

 
This is the code for the cube that follows the player’s position. On the Start method, it sets the target as the object on scene tagged as “Ball”. In Update, it refreshes the target’s reference if it is lost, then it sets the velocity as the position vector of the target minus the cube’s position vector normalized, multiplied by the speed. This ensures that the speed vector will point exactly towards the ball’s position when setting the origin in the cube.
 
This is the code for the cube that follows the player by predicting their next position. It is really similar to the follower cube, but instead of subtracting the cube’s position vector to the target’s position vector, it subtracts it to a vector that is the sum of the target’s position and the target’s velocity.


User Guide and System Requirements
In this game, the player controls the red ball’s position using the arrow keys. The goal is to avoid colliding with any blue cube for 2 minutes. There are no minimal system specs required to play this game. To open it, just double click DodgeARoll.exe
