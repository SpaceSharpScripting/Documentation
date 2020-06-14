# Scripting

```js
// This function return milliSeconds since 1970
getMilliSeconds()

// A function to log messages into the SpaceSharp Script-Logging-Window
log(string logMsg)

// Clears Log Window
clearLogs()

// Activates logging console output into a file on script unload
toggleDebugLogs(bool debug)

// Returns the slot containing your requested item. If no slot was found, returns 0.
// Item code can be found on LoL Wiki
// For example BOTRK can be found here: https://leagueoflegends.fandom.com/wiki/Blade_of_the_Ruined_King
// BOTRK code is 3153
getItemSlot(int code)

// Draws a text on screen.
// After toggleTime the text will be removed
drawText(string txt, int x, int y, int toggleTime = 20, int fontSize = 16, string hexColor = "#133713", double opacity = 1d)

// Returns the percentage health of the player
getPercentageHealth()

// Returns attack speed of player
getAttackSpeed()

// Returns movement speed of player
getMovementSpeed()

// Returns movement speed of player in pixels (care it is only a rough appxoimation)
getMovementSpeedInPixels()

// Disables movement (rightclicks) during kiting
disableMovementWhileKiting(bool disable = true)

// Returns the current mouse position as an array [x,y]
getMousePos()

// Does a left click on the current position of mouse
mouseClickLeft()

// Does a right click on the current position of mouse
mouseClickRight()

// Returns the position of player champ while kiting as an array (this function is also quite inaccurate, so use with care)
getPlayerPosWhileKiting()

// Check if a key on keyboard is down
isKeyDown(int keyCode)

// Presses Key
pressKey(int keyCode)

// Holds key down until keyUp is called
keyDown(int keyCode)

// Releases key
keyUp(int keyCode)

/* Returns a bunch of information regarding the player. these information are:
abilityPower
armor
attackDamage
attackRange
bonusArmorPenetrationPercent
bonusMagicPenetrationPercent
cooldownReduction
critChance
critDamage
currentHealth
healthRegenRate
lifeSteal
magicResist
maxHealth
moveSpeed
resourceMax
resourceRegenRate
resourceType
resourceValue
currentGold
level
abilityLevelQ
abilityLevelW
abilityLevelE
abilityLevelR

For example:
var stats = getChampionStats();
log(stats['level']); <-- Will print current champion level
*/
getChampionStats()

// Returns current player health% as double
getCurrentHealth()

// Converts ingame range to range in pixels 
convertIngameRangeToRangeInPixel(int ingameRange)

// Checks whether an given point (x,y) is in given range while kiting (e.g. useful for checking if speel will hit enemy)
// width is the width of the hitbox in pixels and height the height of hitbox in pixels
isInRangeWhileKiting(int x, int y, int range, int width = 1, int height = 1)

// Returns the dimensions of the screen as {"width": value, "height": value}
getScreenDimensions()

// Searches for a given color (red, green, blue) on screen with respect to some tolerance. Therefore the color can slightly differ
// startX and startY are the start position of pixel search, where width and height are the width and height of the area to search through
// returns only one found pixel at a time as [x,y] array
// If pixel was not found, will return [-1,-1]
searchPixel(int red, int green, int blue, int tolerance = 0, int startX = 0, int startY = 0, int width = -1, int height = -1)

// Searches the screen for HP bar of player (only working in color blind mode) and returns array [x,y] of left top point found hp bar
getPlayerHPBarPos()

// This function allows to check whether a point (or rectangle if width and height >1) lies inside of the attack range ellipse
// centerEllipseX and centerEllipseY is the center point of the ellipse
// range is the ingame range
// x and y is the point to check whether it lies inside the ellipse (if using rectangle, x and y describe the top left corner of rectangle)
isInRange(int centerEllipseX, int centerEllipseY, int range, int x, int y, int width = 1, int height = 1)

// Gets the color at a given x,y position of image (care it is quite slow and the image may change between two consecutive calls due to next image was caught in buffer)
// Returns: {"r": value, "b":value, "g":value}
getPixelColor(int x, int y)

// This function allows to do some optical number recognition on screen in a given area
// Make sure the area is not way too big otherwise it may detect numbers where no are
readNumberFromScreen(int x, int y, int width, int height)

// A function which searches a cluster of similar colors on screen
// This function searches alles pixels with given colors as 2D array and clusters them afterwards in regions
// tolerance defines the global maximum difference between given colors and pixel found
// --> tolerance of 5 would mean that the given colors can be up to a difference of 5 different (e.g. r=10, g=20, b=30; found pixel: r=12,g=18, b=30; with tolerance =3 would successfully find this pixel)
// minPixelAmount is the minimum amount needed to form a valid region/pixel-cluster, which is returned
// maxPixelAmount is the maximum amount of pixels per region allowed
// startX and startY are the start position of pixel search, where width and height are the width and height of the area to search through
// This function returns an array of regions found:
/*
[
    {
        "x": 123, // left x value
        "y": 452, // top y value
        "width": 182,   /
        "height": 473
    },
    // ... next region
]
*/
searchPixelCluster([[red,green,blue],...], int tolerance = 0, int minPixelAmount = 10, int maxPixelAmount = 1000000, int startX = 0, int startY = 0, int width = -1, int height = -1)

// Callback is executed every 10ms and runs parallel to onForever.
// This allows to listen on keys or similar on onForeverFast and do heavy processing work in onForever
onForeverFast()

// This function searches the whole screen for enemy champs and returns them as array.
// the return value will look like this:
/*
[
    {
        "x": 123, // x position of champ
        "y": 452,
        "xHp": 182,   // x position of very left point in hp bar
        "yHp": 473
    },
    // ... next enemy
]
*/
searchAllEnemyChamps()

// Returns all enemy champs found on screen. This function is quite fast as it retrieves the champs from an internal buffer.
// However use with care as the data provided by this function may be up to ~200ms old. 
// It will definitely most of the time not work if you click on an enemy champion based on the returned values in this function!
// You should better add some kind of pixelSearch or similar of HP bar afterwards and find the enemy closest to the position returned by this function to make sure you got the latest position of enemy!
// the return value will look like this:
/*
[
	{
		"hpPercentage": 0.3,
		"x": 123,
		"y": 452,
		"xHp": 182,   // x position of very left point in hp bar
		"yHp": 473
	},
	// ... next enemy
]
*/
getEnemyChamps()

// Returns all minions found on screen. This function is quite fast as it retrieves the minions from an internal buffer.
// However use with care as the data provided by this function may be up to ~200ms old. 
// It will definitely most of the time not work if you click on a minion based on the returned values in this function!
// You should better add some kind of pixelSearch or similar of minion HP bar (restricting it to the area you found enemy) afterwards and find the minion closest to the position returned by this function to make sure you got the latest position of minion!
// the return value will look like this:
/*
[
	{
		"hpPercentage": 0.3,
		"x": 123,
		"y": 452,
		"xHp": 182,  // x position of very left point in hp bar
		"yHp": 473,
		"minionType": "caster", // can be: caster, melee, cannon
		"isLastHitable": true  // if true minion will die if player uses simple auto attack on it
	},
	// ... next minion
]
*/
getMinions()

// Returns enemy champion stats (champion name and skin id for now)
// the return value will look like this:
/*
[
	{
		"championName": "Nasus",
		"skinID": 0,
	},
	// ... next enemy
]
*/
getEnemyChampionStats()

// Moves mouse to x,y position
moveMouseToPos(int x, int y)

// Returns x,y position of enemy as an array, which is next to mouse
getEnemyNextToMouse(bool checkForInRange)

// Sets the attackspeed to a fixed value and will never get updated again by SpaceSharp
setAttackSpeedTo(double attackSpeed)

// Disables auto attacks while kiting (e.g. useful when implementing Cassio ability kiting)
disableAAWhileKiting(bool disable=true)

// Sleeps given amount of milliseconds
sleep(int sleepTime)

// Returns true when the player is dead,
isDead()

// Allows to send a GET-Request to an abitrary url
doGETRequest(string url)

//
//	Callbacks
//	The following functions will be invoked by C# at given times of execution
//

// This function is invoked when the "Start/Stop" toogle is set to "Start"
function onStart() {}

// This function is invoked when the "Start/Stop" toogle is set to "Stop"
function onStop() {}

// This function is invoked before actually doing the "real heal" of Spacesharp (e.g. usefull for activating heal and barrier at the same time)
// The parameter supplied is the current HP of player (values reach from 0 to 1)
function onHeal(hp) {}

// This function is invoked before the auto attack while kiting is done.
// But the mouse cursor is already on the enemy
// The parameters are the x and y coordinate of the found enemy
function onBeforeAttackWhileKiting(x,y) {}

// This function is invoked before the auto attack while kiting is done.
function onAfterAttackWhileKiting(x,y) {}

// Gets called after windup time is over
// Useful to implement auto attacks in combos
function onAfterWindupWhileKiting(x, y) {}

// Gets called after releasing Attack Everything key
function onAfterAttackEverything() {}

// Gets called after pressing Attack Everything key
function onBeforeAttackEverything() {}

// Gets called after releasing Attack Champion key
function onAfterAttackChampion() {}

// Gets called after pressing Attack Champion key
function onBeforeAttackChampion() {}

// This function is called every few milliseconds
// You could do things like checking if Key is pressed, etc.
function onForever() {}
