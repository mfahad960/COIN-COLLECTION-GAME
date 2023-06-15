## DESCRIPTION:
The user provides input to the game using A, D, W or Spacebar keys. A to move left, D
to move right, W or Spacebar to jump/ moveup. Coins will appear over the player
character (^), and the user will have to jump to collect them and to score. At the start of
the game, the 3 lives are given for every difficulty. Missing a coin will result in loss of
one life. Lives also spawn over the player and are collectible. Every difficulty has a
different number of lives appearing after the set time.
The score increments upon collection of each coin. The player can either quit by pressing
the 'Esc' key during the game, or the game ends when the player runs out of lives. At the
end of the game, a message displays the current score. Also, if the current score is greater
than the highscore saved in "highscore.txt" file, another message box displays an
appropriate prompt and the new high score is updated in the file.

## USER INTERFACE:
The first screen is the introduction window, displayed after executing the project code.
The second window shows following difficulty settings.
After selecting the difficulty the game starts. The top left side of the console window
shows the current score, lives left, the difficulty selected by the player, and the highscore
saved in file.

## INSTRUCTIONS:
The player is depicted as 'X' on the screen. The objective is to collect as many coins to rack up as high a score as possible.
Coins will appear for a set time, which differs for each difficulty.
The player is asked to choose between three difficulty settings at the start of the game.
Each difficulty setting affects the game as described below.
### Difficulties:
	Easy -> Coin time on screen 16 seconds.
	Medium -> Coin time on screen 12 seconds.
	Hard -> Coin time on screen 12 seconds.
If the player cannot collect a coin, a life will be reduced.
Lives will also appear on screen as collectible.
### Difficulties:
	Easy -> Life will appear every 10 seconds.
	Medium -> Life will appear every 15 seconds.
	Hard -> Life will appear every 20 seconds.
Lives will not despawn until collected by the player.
### Controls:
	W or Space Bar -> Jump/ Move up.
	A -> Move left.
	D -> Move right.
	Esc -> Exit game.
If all lives are lost(zero lives remaining), then the game will end.
The player can also quit at any time during the game, by pressing 'Esc'(escape) key(after
choosing difficulty).
