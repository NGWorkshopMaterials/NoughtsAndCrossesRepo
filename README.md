# Naughts and Crosses code snippets

## Exercise 1 - Creating the board
#### index.html
```
 <div class="board" id="board">
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
    <div class="cell" data-cell></div>
  </div>
```

#### style.css
```
.board {
  width: 100vw;
  height: 100vh;
  display: grid;
  justify-content: center;
  align-content: center;
  justify-items: center;
  align-items: center;
  grid-template-columns: repeat(3, auto);
}

.cell {
  width: var(--cell-size);
  height: var(--cell-size);
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  cursor: pointer;
}
```

## Exercise 2 – Win conditions and game start
```
const X_CLASS = 'x'
const CIRCLE_CLASS = 'circle'
const WINNING_COMBINATIONS = [
  [0, 1, 2]
]
```

```
[
  [0, 1, 2],
...
  [x, y, z]
]
```

```
const cellElements = document.querySelectorAll('[data-cell]');
const board = document.getElementById('board');
```

```
circleTurn = false
```

```
function startGame() {
  circleTurn = false
  cellElements.forEach(cell => {
    cell.classList.remove(X_CLASS)
    cell.classList.remove(CIRCLE_CLASS)
    cell.removeEventListener('click', handleClick)
    cell.addEventListener('click', handleClick, { once: true })
  })
}
```


## Exercise 3 – Click Handler
```
const cell = e.target;
const currentClass = circleTurn ? CIRCLE_CLASS : X_CLASS;
```

```
function placeMark(cell, currentClass) {
  cell.classList.add(currentClass)
}
```

## Exercise 4 – End game
```
function handleClick(e) {
  const cell = e.target
  const currentClass = circleTurn ? CIRCLE_CLASS : X_CLASS
  placeMark(cell, currentClass)
  if (checkWin(currentClass)) {
    endGame(false)
  } else if (isDraw()) {
    endGame(true)
  } else {
    swapTurns()
    setBoardHoverClass()
  }
}

function endGame(draw) {
  if (draw) {
    winningMessageTextElement.innerText = 'Draw!'
  } else {
    winningMessageTextElement.innerText = `${circleTurn ? "O's" : "X's"} Wins!`
  }
  winningMessageElement.classList.add('show')
}

function isDraw() {
  return [...cellElements].every(cell => {
    return cell.classList.contains(X_CLASS) || cell.classList.contains(CIRCLE_CLASS)
  })
}

function placeMark(cell, currentClass) {
  cell.classList.add(currentClass)
}

function swapTurns() {
  circleTurn = !circleTurn
}

function checkWin(currentClass) {
  return WINNING_COMBINATIONS.some(combination => {
    return combination.every(index => {
      return cellElements[index].classList.contains(currentClass)
    })
  })
}
```


## Exercise 5 – Graphical updates for gameplay
```
.cell.x,
.cell.circle {
  cursor: not-allowed;
}

.cell.x::before,
.cell.x::after,
.cell.circle::before {
  background-color: black;
}

.board.x .cell:not(.x):not(.circle):hover::before,
.board.x .cell:not(.x):not(.circle):hover::after,
.board.circle .cell:not(.x):not(.circle):hover::before {
  background-color: lightgrey;
}

.cell.x::before,
.cell.x::after,
.board.x .cell:not(.x):not(.circle):hover::before,
.board.x .cell:not(.x):not(.circle):hover::after {
  content: '';
  position: absolute;
  width: calc(var(--mark-size) * .15);
  height: var(--mark-size);
}

.cell.x::before,
.board.x .cell:not(.x):not(.circle):hover::before {
  transform: rotate(45deg);
}

.cell.x::after,
.board.x .cell:not(.x):not(.circle):hover::after {
  transform: rotate(-45deg);
}

.cell.circle::before,
.cell.circle::after,
.board.circle .cell:not(.x):not(.circle):hover::before,
.board.circle .cell:not(.x):not(.circle):hover::after {
  content: '';
  position: absolute;
  border-radius: 50%;
}

.cell.circle::before,
.board.circle .cell:not(.x):not(.circle):hover::before {
  width: var(--mark-size);
  height: var(--mark-size);
}

.cell.circle::after,
.board.circle .cell:not(.x):not(.circle):hover::after {
  width: calc(var(--mark-size) * .7);
  height: calc(var(--mark-size) * .7);
  background-color: white;
}
```


## Exercise 6 – Endgame messages
```
<div class="winning-message" id="winningMessage">
    <div data-winning-message-text></div>
    <button id="restartButton">Restart</button>
  </div>
```

```
.winning-message {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, .9);
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 5rem;
  flex-direction: column;
}

.winning-message button {
  font-size: 3rem;
  background-color: white;
  border: 1px solid black;
  padding: .25em .5em;
  cursor: pointer;
}

.winning-message button:hover {
  background-color: black;
  color: white;
  border-color: white;
}

.winning-message.show {
  display: flex;
}
```

```
const winningMessageElement = document.getElementById('winningMessage');
const restartButton = document.getElementById('restartButton');
const winningMessageTextElement = document.querySelector('[data-winning-message-text]');
```

```
restartButton.addEventListener('click', startGame);
```

```
winningMessageElement.classList.remove('show')
```

```
function startGame() {
  circleTurn = false
  cellElements.forEach(cell => {
    cell.classList.remove(X_CLASS)
    cell.classList.remove(CIRCLE_CLASS)
    cell.removeEventListener('click', handleClick)
    cell.addEventListener('click', handleClick, { once: true })
  })
  winningMessageElement.classList.remove('show')
}
```


## Exercise 7 – Mouse hovering
```
function setBoardHoverClass() {
  board.classList.remove(X_CLASS)
  board.classList.remove(CIRCLE_CLASS)
  if (circleTurn) {
    board.classList.add(CIRCLE_CLASS)
  } else {
    board.classList.add(X_CLASS)
  }
}
```

```
function startGame() {
  circleTurn = false
  cellElements.forEach(cell => {
    cell.classList.remove(X_CLASS)
    cell.classList.remove(CIRCLE_CLASS)
    cell.removeEventListener('click', handleClick)
    cell.addEventListener('click', handleClick, { once: true })
  })
  setBoardHoverClass()
  winningMessageElement.classList.remove('show')
}
```

```
.cell.x,
.cell.circle {
  cursor: not-allowed;
}
```


## Exercise 8 – Final stylisation
```
.cell:nth-child(1), .cell:nth-child(2), .cell:nth-child(3) {
  border-top: none;
}
```

```
.cell:nth-child(1), .cell:nth-child(2), .cell:nth-child(3) {
  border-top: none;
}

.cell:nth-child(1), .cell:nth-child(4), .cell:nth-child(7) {
  border-left: none;
}

.cell:nth-child(3), .cell:nth-child(6), .cell:nth-child(9) {
  border-right: none;
}

.cell:nth-child(7), .cell:nth-child(8), .cell:nth-child(9) {
  border-bottom: none;
}
```
