# Concentration

## Technologies
CSS, Javascript

## Brief overview
Poker cards are lain face down in 4 rows, with 4 cards on each row. At every turn, the player flips over two poker cards. If it matches(same suit and rank), the cards are removed from the board.
If it doesn't, the cards are flipped back. And the next turn begins. The player has to clear the board within the time limit given.

After the 4 * 4 board, the next and last level is the 6 * 6, with a slightly longer time limit given. 

## Process 

1. Obtain the card Images. Credits to [Stuart Myers](https://github.com/LaustinSpayce) for providing the card images, without which this game wouldn't be possible. 

2. Coming up with the array of card objects.
  {
    id:
    suit:
    rank:
    img_link:
  }
3. Writing the loop which populates the images on a 4 * 4 or 6 * 6 grid.
4. Learning the flip animation from Google.
5. Coming up with match condition.
6. Coming up with logic which prevents the player from gaming the system.
   i.e. A match resulting simply from double-clicking, flipping more than 2 cards, 
7. Adding sounds and colours.

## Key Takeaways

## Things I have learnt 

#### Modularisation. Abstracting out variables frequently used.
  * Timer and lagtime are made global variables, which are located at the beginning of the script. This is so that changes can be easily implemented, facilitating the testing process.
  If I wanted change the timer for stage1, all I have to do is to change the value for stage1Timer, which is conveniently located on Line 25(at the very top where the actual code starts after the comments). </br>
    
 
``` javascript

var stage1Timer = 3;
var stage2Timer = 3;

// Lag time before card is flipped back 
var waitTime = 550;

```

 If I had not done this, I will have a troublesome time locating the stage1Timer variablee which is located on line 472 

``` javascript 
function setTime(){
	if (stage == 1){
		time = stage1Timer;
	} else if(stage == 2){
		time = stage2Timer;
	}
}
```





* Abstracting out blocks of code to their individual functions adheres to the Single Responsibility Principle. This makes each function easier to test and troubleshoot.

``` javascript
function generateFullBoard(n){
	// Clear JavaScript Board
	JSBoard = [];
	// Generate the HTML DOMs for the board
	createDomElements(n);
	// Add query selectors
	addEventListener();
	// Shuffle cards
	cards = shuffle(cards);
	// Choose the matching pairs
	transferStepOne(n);
	// Shuffle their position
	tempArray = shuffle(tempArray);
	// Make the board an n*n array 
	transferStepTwo(n);
	// Add in images of card when it is flipped over
	populateBackImages(n);
	// Define number of matches to win
	setMatchesToWin();
	// Set the time;
	setTime();
	// So that timer function will only be called on the first click
	hasGameStarted = false;		
}
```
#### State design pattern. 
I learnt this from a Youtube Tutorial on Flappy Bird which also tracks the game state using a similar method.

``` javascript
// Object which tracks the game state
const gameState = {
	current: 0,
	inGame: 0,
	endGame: 1
}

//Board generated from the start
function changeState(){
	switch(gameState.current){
		case(0):
			//Reset Global Variables
			score = 0;
			var scoreDisplay = document.querySelector("#scoreDisplay");
			scoreDisplay.innerText = "Score: "+score

			stage = 1;
			generateFullBoard(4);
			
			//Populate past scores
			var pastScore = document.querySelector('#PastScore');
			pastScore.innerHTML = ""
			for(let i = 0; i < pastScores.length; i++){
				var record = document.createElement('li')
				record.innerText = pastScores[i].name+" "+pastScores[i].yourScore
				var mainBoard = document.querySelector('#mainBoard');
				pastScore.appendChild(record)
			}
			break;

		case(1):
			showGameOverScreen();
			break;
	}
}

```

#### HTML DOM Manipulation
* The cards are Document Object Models - they are created and removed by a function call. 
* Every DOM card has an id which represents its position in the backend Javascript board, which is an array of arrays. e.g. an id of col-11 means that the card is in row 1, col 1 of the Javascript board.
* Each element in the JS Board contains the attribute of each card, namely: the image link, a unique identifier, rank and suit. 
* The position of the cards in the backend Javascript board is randomly generated everytime a new game is created.
* Each DOM card has a click event listener, which contains all the logic that follows after the click i.e. checking if the cards match, removing cards when they match, checking win condition etc. 

#### CSS Flip Animation
```html
<div class="flipper">
  <div class="front">
    <img src="Cards/cardBack_red5.png"></img>
  </div>
  <div class="back">
    <img src="Cards/cardBack_Queen7.png"></img>
  </div>
</div>
```
A card has a 2 sides. As the card is faced down during the game, the side facing up shows the logo and the side facing down shows the rank and suit. </br>
The side facing up is a `div` with `class='front'`, while the side facing down is a `div` with `class='back'`

Looking at the CSS, `div` with `class='front'` has a z-index of 2, meaning that it is on top of the div with `class='back'`. 
The div with `class='back'` is also transformed 180 degrees away, meaning that it is facing down(opposite of where `div` with `class='front'` is facing.

The CSS attribute `transform-style: preserve-3d` in `.flipper` allows the front and back div's 3d-position within the `.flipper` div to be preserved(i.e. `.front` being on top of `.back`, such that a 180 degree rotation of `.flipper` along the y-axis will mimic the rotation of an actual flip, such that the `.back` side will face up after the flip(and `.front` side will face down).

```css
.flipper {
    transition: 0.5s;
    transform-style: preserve-3d;
    position: relative;
    margin: 0px;
}

.front, .back {
    backface-visibility: hidden;
    position: absolute;
    top: 0;
    left: 0;
}

/* front pane, placed above back */
.front {
    z-index: 2;
    /* for firefox 31 */
    transform: rotateY(0deg);
}

/* back, initially hidden pane */
.back {
    transform: rotateY(180deg);
}

.col.flipped .flipper {
        transform: rotateY(180deg);
    }


```
## Conclusion

Overall, this project familiarised me with the Javascript language and CSS.
