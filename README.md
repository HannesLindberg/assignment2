# DIT044 Assignment 2

## UML diagram for codebase
<img width="1083" height="550" alt="image" src="https://github.com/user-attachments/assets/0c4da285-dfb6-493b-9bb5-5ee779330f9d" />

### Explanation
|Relation|Type|Reason|
|-|-|-|
| `CarApplication` -> `CarController`, `CarView` | uses | It creates them and wires them together at startup. |
| `CarController` -> `CarView`| has | It tells the view to repaint after each timer step. |
| `CarView` -> `CarController` | has | It calls `gas()` and the other controller methods when buttons are pressed. |
| `CarView` -> `DrawPanel` | has | The view is built from it, and it draws the cars. |
| `CarController` -> `Car` | has | It holds the cars and moves them each tick. |
| `CarController` -> `TimerListener` | uses | It creates the listener for the timer. |
| `Volvo95`, `Saab240` -> `Car` | extends | They share speed and position logic. |
| `CarView` -> `JFrame` | extends | The view is a window. |
| `DrawPanel` -> `JPanel` | extends | It is a Swing panel that paints the cars. |
| `Car` -> `Moveable` | implements | `Moveable` is the contract for anything that can move. |
| `TimerListener` -> `ActionListener` | implements | `ActionListener` is the contract for reacting to timer events. |
