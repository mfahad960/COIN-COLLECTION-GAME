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
USER INTERFACE:
The first screen is the introduction window, displayed after executing the project code.
The second window shows following difficulty settings:
1. Easy: 16 seconds coin despawn time. 10 seconds timer to spawn a life (a life will
appear after every 10 seconds).
2. Medium: 12 seconds coin despawn time. 15 seconds timer to spawn a life (a life
will appear after every 15 seconds).
3. Hard: 8 seconds coin despawn time. 20 seconds timer to spawn a life (a life will
appear after every 20 seconds).
After selecting the difficulty the game starts. The top left side of the console window
shows the current score, lives left, the difficulty selected by the player, and the highscore
saved in file.
SOURCE CODE:
;COAL Project
;Coin Collection Game
;Created By Muhammad Fahad - 20K-0441 - BCS-3C
;-----------------------------------------------------------------------------------------------------------
-------------------------;
;Instructions:
;The player is depicted as 'X' on the screen.
;The objective is to collect as many coins to rack up as high a score as possible.
;Coins will appear for a set time, which differs for each difficulty.
;The player is asked to choose between three difficulty settings at the start of the game.
;Each difficulty setting affects the game as described below.
;Difficulties: Easy -> Coin time on screen 16 seconds.
; Medium -> Coin time on screen 12 seconds.
; Hard -> Coin time on screen 12 seconds.
;If the player cannot collect a coin, a life will be reduced.
;Lives will also appear on screen as collectible.
;Difficulties: Easy -> Life will appear every 10 seconds.
; Medium -> Life will appear every 15 seconds.
; Hard -> Life will appear every 20 seconds.
;Lives will not despawn until collected by the player.
;Controls: W or Space Bar -> Jump/ Move up.
; A -> Move left.
; D -> Move right.
; Esc -> Exit game.
;If all lives are lost(zero lives), then the game will end.
;The player can also quit at any time during the game, by pressing 'Esc'(escape) key(after
choosing difficulty).
;Code Starts:
INCLUDE Irvine32.inc
BUFFER_SIZE=10
.data
str5 BYTE "COAL PROJECT", 0
str6 BYTE "COIN COLLECTION GAME", 0
str7 BYTE "Created By Muhammad Fahad -> 20K-0441 -> BCS-3C", 0
str8 BYTE "Difficulty Settings: ", 0
str9 BYTE "Press Esc key to exit...", 0
buffer byte BUFFER_SIZE dup(?), 0
filename BYTE "highscore.txt", 0
fileHandle HANDLE ?
filesize DWORD ?
fmt BYTE "%d", 0
strHS BYTE "High Score: ", 0
strScore BYTE "Score: ", 0
score DWORD 0
strLives BYTE "Lives: ", 0
lives DWORD 3
strDifficulty BYTE "Difficulty: ", 0
lenDifficulty BYTE 0
Easy BYTE "Easy", 0
Medium BYTE "Medium", 0
Hard BYTE "Hard", 0
xMax BYTE 119
yMax BYTE 29
xPos BYTE 20
yPos BYTE 20
xCoinPos BYTE ?
yCoinPos BYTE ?
xLifePos BYTE ?
yLifePos BYTE ?
inputChar BYTE ?
str1 BYTE "Choose difficulty: ", 0
str2 BYTE "1) Easy", 0
str3 BYTE "2) Medium", 0
str4 BYTE "3) Hard", 0
gameover BYTE "GAME OVER! Your score is: ", 10 DUP(0)
restart BYTE "Do you want to play again?", 0
title1 BYTE "Game Ended", 0
scoreSTR BYTE 10 DUP(?), 0
highscore BYTE "You have beaten the highscore!", 0
gg BYTE "GGWP", 0
invalid BYTE "Invalid choice enter again: ", 0
coin_startTime DWORD ?
life_startTime DWORD ?
coin_timer DWORD 0
life_timer DWORD 0
lifespawned BYTE 0
lenGO DWORD 26
Str_concat PROTO,
source:PTR BYTE, ; source string
target:PTR BYTE, ; target string
len:DWORD
.code
main PROC
; initial window
mov eax, black (lightcyan * 16)
call SetTextColor
call clrscr
mov dl, 52
mov dh, 10
call gotoxy
mov edx, OFFSET str5
call WriteString
mov dl, 47
mov dh, 11
call gotoxy
mov edx, OFFSET str6
call WriteString
mov dl, 37
mov dh, 12
call gotoxy
mov edx, OFFSET str7
call WriteString
mov dl, 45
mov dh, 13
call gotoxy
call WaitMsg
; difficulty window
START:
call clrscr
mov score, 0
mov lives, 3
mov dl, 45
mov dh, 12
call gotoxy
mov edx, OFFSET str8
call WriteString
mov dl, 45
mov dh, 13
call gotoxy
mov edx, OFFSET str2
call WriteString
mov dl, 45
mov dh, 14
call gotoxy
mov edx, OFFSET str3
call WriteString
mov dl, 45
mov dh, 15
call gotoxy
mov edx, OFFSET str4
call WriteString
mov dl, 45
mov dh, 16
call gotoxy
mov edx, OFFSET str1
call WriteString
read:
call ReadDec
cmp eax, 1
je e
cmp eax, 2
je m
cmp eax, 3
je h
jmp inv
e:
mov edi, offset Easy
mov lenDifficulty, lengthof Easy
mov coin_timer, 16000 ; 16 seconds coin appearance time for easy difficulty
mov life_timer, 10000 ; 10 seconds timer for random life reappearance
jmp next
m:
mov edi, offset Medium
mov lenDifficulty, lengthof Medium
mov coin_timer, 12000 ; 12 seconds coin appearance time for easy difficulty
mov life_timer, 15000 ; 15 seconds timer for random life reappearance
jmp next
h:
mov edi, offset Hard
mov lenDifficulty, lengthof Hard
mov coin_timer, 8000 ; 8 seconds coin appearance time for easy difficulty
mov life_timer, 20000 ; 20 seconds timer for random life reappearance
jmp next
inv:
mov dl, 45
mov dh, 17
call gotoxy
mov edx, offset invalid
call writestring
jmp read
next:
call clrscr
; draw ground at (0,yMax):
call DrawGround
call DrawPlayer
call CreateRandomCoin
call DrawCoin
call Randomize
; read highscore from file
call ReadScore
; timers start for first time
call GetMseconds
mov coin_startTime, eax
mov life_startTime, eax
gameLoop:
; check lives timer:
cmp lifespawned, 1
je chklives
mov dl, 0
mov dh, 4
call gotoxy
call GetMseconds
sub eax, life_startTime
cmp eax, life_timer
jl notcollectinglives
; spawn lives
call CreateRandomLife
call DrawLife
mov lifespawned, 1
; check life intersection:
chklives:
mov bl, xPos
cmp bl, xLifePos
jne notcollectinglives
mov bl, yPos
cmp bl, yLifePos
jne notcollectinglives
; player is intersecting life:
inc lives
; restart life timer as life collected
call GetMseconds
mov life_startTime, eax
mov lifespawned, 0
notcollectinglives:
; getting points:
mov bl, xPos
cmp bl, xCoinPos
jne notCollecting
mov bl, yPos
cmp bl, yCoinPos
jne notCollecting
; player is intersecting coin(scored):
inc score
call CreateRandomCoin
call DrawCoin
; restart coin timer as new coin placed
call GetMseconds
mov coin_startTime, eax
jmp notCollecting
missed:
dec lives
cmp lives, 0
je exitGame
call UpdateCoin
call CreateRandomCoin
call DrawCoin
; restart timer as new coin placed
call GetMseconds
mov coin_startTime, eax
notCollecting:
call GetMseconds
sub eax, coin_startTime
cmp eax, coin_timer ; coin timer implementation
jge missed
mov eax, black (lightcyan * 16)
call SetTextColor
;read and draw highscore:
mov dl, 20
mov dh, 0
call Gotoxy
mov edx, OFFSET strHS
call WriteString
mov edx, offset buffer
call Writestring
; draw score:
mov dl, 0
mov dh, 0
call Gotoxy
mov edx, OFFSET strScore
call WriteString
mov eax, score
call WriteInt
; draw lives:
mov dl, 0
mov dh, 1
call Gotoxy
mov edx, OFFSET strLives
call WriteString
mov eax, lives
call WriteInt
mov eax, red (lightcyan * 16)
call SetTextColor
mov al, 03h
call writechar
; draw difficulty:
mov eax, black (lightcyan * 16)
call SetTextColor
mov dl, 0
mov dh, 2
call Gotoxy
mov edx, OFFSET strDifficulty
call WriteString
mov edx, edi
call WriteString
; exit msg:
mov eax, black (lightcyan * 16)
call SetTextColor
mov dl, 0
mov dh, 3
call Gotoxy
mov edx, OFFSET str9
call WriteString
; gravity logic(initial charachter fall):
gravity:
cmp yPos, 27
jg onGround
; make player fall:
call UpdatePlayer
inc yPos
call DrawPlayer
mov eax, 80
call Delay
jmp gravity
onGround:
; get user key input:
call ReadChar
mov inputChar, al
; exit game if user types 'x':
cmp inputChar, 27 ; "ECS" escape key
je exitGame
cmp inputChar, " "
je moveUp
cmp inputChar, "w"
je moveUp
cmp inputChar, "a"
je moveLeft
cmp inputChar, "d"
je moveRight
jmp onGround
moveUp:
; allow player to jump:
mov ecx, 1
jumpLoop:
call UpdatePlayer
dec yPos
call DrawPlayer
mov eax, 60
call Delay
loop jumpLoop
jmp gameLoop
moveLeft:
cmp xPos, 0
je gameloop
call UpdatePlayer
dec xPos
call DrawPlayer
jmp gameLoop
moveRight:
cmp xPos, 119
je gameloop
call UpdatePlayer
inc xPos
call DrawPlayer
jmp gameLoop
jmp gameLoop
exitGame:
; final output message
call clrscr
push score
push offset fmt
push offset ScoreSTR
call wsprintf
Invoke Str_concat, ADDR ScoreSTR, ADDR gameover, lenGO
; message box showing score
mov edx, OFFSET gameover
mov ebx, OFFSET title1
call MsgBox
; if current score greater then high score then save to file
mov al, buffer
cmp scoreSTR, al
jbe exit1
mov edx, OFFSET highscore
mov ebx, OFFSET gg
call MsgBox
call SaveScore
exit1:
mov edx, offset restart
mov ebx, 0
call MsgBoxAsk
cmp eax, 6
je START
exit2:
exit
main ENDP
DrawGround PROC
mov dl, 0
mov dh, yMax
call gotoxy
movzx ecx, xMax
inc ecx
L3:
mov al, "-"
call writechar
inc dl
call gotoxy
loop L3
ret
DrawGround ENDP
DrawPlayer PROC
; draw player at (xPos,yPos):
mov dl, xPos
mov dh, yPos
call Gotoxy
mov al, 94
call WriteChar
ret
DrawPlayer ENDP
UpdatePlayer PROC
mov dl, xPos
mov dh, yPos
call Gotoxy
mov al, " "
call WriteChar
ret
UpdatePlayer ENDP
DrawLife PROC
mov eax, red (lightcyan * 16)
call SetTextColor
mov dl, xLifePos
mov dh, yLifePos
call Gotoxy
mov al, 03
call WriteChar
ret
DrawLife ENDP
CreateRandomLife PROC
L1:
movzx eax, xMax
call RandomRange
cmp al, xCoinPos
je L1
mov xLifePos, al
mov al, yMax
sub al, 2
mov yLifePos, al
ret
CreateRandomLife ENDP
DrawCoin PROC
mov eax, yellow (lightcyan * 16)
call SetTextColor
mov dl, xCoinPos
mov dh, yCoinPos
call Gotoxy
mov al, 04
call WriteChar
ret
DrawCoin ENDP
UpdateCoin PROC
mov dl, xCoinPos
mov dh, yCoinPos
call Gotoxy
mov al, " "
call WriteChar
ret
UpdateCoin ENDP
CreateRandomCoin PROC
L2:
movzx eax, xMax
call RandomRange
cmp al, xLifePos
je L2
mov xCoinPos, al
mov al, yMax
sub al, 2
mov yCoinPos, al
ret
CreateRandomCoin ENDP
Str_concat PROC USES eax ecx esi edi,
source:PTR BYTE, ; source string
target:PTR BYTE, ; target string
len:DWORD
mov ecx, eax ; EAX = length source
mov esi, source
mov edi, target
add edi, len
cld ; direction = forward
rep movsb ; copy the string
ret
Str_concat ENDP
ReadScore PROC
mov edx, offset filename
call openinputfile
mov fileHandle, eax
mov edx, offset buffer
mov ecx, sizeof buffer
call readfromfile
jnc no
mov buffer, "0"
no:
mov eax, filehandle
call closefile
ret
ReadScore ENDP
SaveScore PROC
mov edx, offset filename
call createoutputfile
mov filehandle, eax
mov edx, offset scoreSTR
mov ecx, lengthof scoreSTR
call writetofile
mov eax, filehandle
call closefile
ret
SaveScore ENDP
END main
