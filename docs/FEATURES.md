## Pig Game — Comprehensive Feature Documentation

- **App name**: Pig Game
- **Scope**: Two-player dice game implemented with `index.html`, `style.css`, and `script.js`
- **Primary goal**: First player to reach 100 points

### Table of contents
- **1. Two-player gameplay**
- **2. Dice roll mechanics**
- **3. Current round score accumulation**
- **4. Penalty on rolling 1**
- **5. Hold to bank score**
- **6. Win condition (>= 100 points)**
- **7. New game reset**
- **8. Visual indicators (active, winner, dice visibility)**
- **9. Layout and responsiveness**
- **10. Assets and dependencies**
- **11. State model and events**
- **12. Edge cases and constraints**
- **13. Extensibility ideas**

---

## 1. Two-player gameplay
- **Description**: The game alternates turns between Player 1 and Player 2.
- **UI elements**: `section.player--0` and `section.player--1` display each player's name, total score, and current (round) score.
- **State**: `activePlayer` is `0` or `1` indicating whose turn it is.
- **Activation**: On initialization, Player 1 (`player--0`) starts active.
- **Relevant code**: `activePlayer` is toggled inside `switchPlayer()`.

## 2. Dice roll mechanics
- **Action**: Click the "🎲 Roll dice" button.
- **Randomization**: Uses `Math.trunc(Math.random() * 6) + 1` to produce an integer from 1 to 6.
- **Display**: Sets `diceEl.src = \`dice-${dice}.png\`` and removes `hidden` from the dice image.
- **Outcome**: If result is 2–6, it is added to the current round score. If 1, see Feature 4.
- **Asset example**:
  
  ![Dice example](../dice-5.png)

## 3. Current round score accumulation
- **Description**: Values 2–6 accumulate in the current turn's round score (`currentScore`).
- **Display**: Shown in `#current--0` or `#current--1` depending on `activePlayer`.
- **Reset**: `currentScore` resets to 0 when the turn ends (hold or roll 1) or when a new game starts.

## 4. Penalty on rolling 1
- **Trigger**: Rolling a 1 on your turn.
- **Effect**: `currentScore` is lost (not banked), UI current score resets to 0, and turn immediately passes to the other player.
- **Implementation**: Calls `switchPlayer()` which toggles the active styles and resets `currentScore`.

## 5. Hold to bank score
- **Action**: Click the "📥 Hold" button.
- **Effect**: Adds `currentScore` to the active player's total (`scores[activePlayer]`).
- **UI**: Updates `#score--0` or `#score--1` accordingly.
- **Next step**: Checks win condition; if not met, turn switches to the other player.

## 6. Win condition (>= 100 points)
- **Threshold**: First player to reach at least 100 total points wins.
- **End state**: Sets `playing = false`, hides the dice, adds `player--winner` to the winner panel, and removes `player--active`.
- **UI outcome**: Winner panel has dark background and highlighted name.

## 7. New game reset
- **Action**: Click the "🔄 New game" button.
- **Reset behavior**: Sets total scores to 0, current scores to 0, `activePlayer` to 0, `playing = true`, hides dice, removes `player--winner`, and sets `player--0` active.
- **Purpose**: Returns the game to its initial state for a fresh match.

## 8. Visual indicators (active, winner, dice visibility)
- **Active player**: Panel has a lighter background; `.player--active .name` and `.player--active .score` use bolder weights.
- **Current box**: Higher opacity when active; indicates the area where round points are accumulating.
- **Winner**: `.player--winner` changes background color and name styling.
- **Dice**: Has class `hidden` when not visible (initially, and after game end).

## 9. Layout and responsiveness
- **Layout**: Desktop-first, centered `main` container of `100rem x 60rem` with two equal `.player` columns.
- **Responsiveness**: Uses `rem` units, but there are fixed positional values for buttons and dice. The UI is primarily optimized for typical desktop widths.
- **Interaction**: Mouse/touch clicks on the three buttons.

## 10. Assets and dependencies
- **Images**: `dice-1.png` through `dice-6.png` displayed within the `.dice` element.
- **Fonts**: Google Fonts `Nunito` via `@import` in `style.css`.
- **No build/runtime deps**: Pure HTML/CSS/JS, no external JS frameworks.

## 11. State model and events
- **State variables**: `scores: number[2]`, `currentScore: number`, `activePlayer: 0|1`, `playing: boolean`.
- **Core functions**:
  - `init()`: Initializes/reset game state and UI.
  - `switchPlayer()`: Resets current round score and toggles active player styling/flag.
- **Events**:
  - `btnRoll.click`: If `playing`, roll dice, update current score or switch player on 1.
  - `btnHold.click`: If `playing`, bank current score, check win, or switch player.
  - `btnNew.click`: Always resets via `init()`.

## 12. Edge cases and constraints
- **Guarded input**: All gameplay actions only proceed when `playing === true`.
- **Holding at 0**: Holding with `currentScore = 0` is allowed and simply switches turn if win is not reached.
- **Initial HTML values**: Any placeholder totals in `index.html` are overwritten by `init()` to 0.
- **Exact player count**: Exactly two players; not configurable at runtime.
- **Winning threshold**: Fixed at 100 in code; not configurable without code changes.
- **Persistence**: No storage; scores reset on page reload.
- **Accessibility**: No ARIA roles, labels, or keyboard shortcuts implemented.

## 13. Extensibility ideas
- **Configurable settings**: Winning score target, number of players, starting player.
- **Keyboard support**: Map roll/hold/new-game to keys; manage focus states.
- **Mobile polish**: Responsive layout for narrow screens; larger tap targets.
- **Animations**: Subtle dice roll animation and score transitions.
- **Persistence**: Save last match or high scores to localStorage.
- **Audio/visual feedback**: Sounds on roll/hold/win; confetti on win.