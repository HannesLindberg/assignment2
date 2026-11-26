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

## Responsibilities

### Responsibilities and reasons to change

| Class | Responsibilities | Reasons to change |
|-|-|-|
| `CarApplication` | Starts the program and wires the controller and view together. | Startup or wiring changes, for example which cars are created at start. |
| `CarController` | Runs the game timer, handles user commands (`gas`, `brake`, ...), updates the cars and tells the view to repaint. | The tick rate changes, a new command is added, or the way the view is updated changes. |
| `CarView` | Builds the window and all buttons, holds `gasAmount`, and forwards button presses to the controller. | The UI layout changes, or the gas input changes. |
| `DrawPanel` | Loads images, stores car positions and paints them. | The image files change, the painting changes, or a new car type needs drawing. |
| `Car` | Holds speed and position and implements moving, gas and brake. | The movement physics change. |
| `Volvo95`, `Saab240` | Define their own speed factor and traits (trim, turbo). | The traits of that specific model change. |
| `Moveable` | Defines what it means to be movable. | The movement contract changes. |
| `TimerListener` | Defines what happens on each timer tick. | The per-tick behaviour changes. |

### Classes that should be split

- **`CarController`** has two kinds of responsibility, driving the simulation and translating user input, so it has several reasons to change. Move the simulation loop into its own class (for example `Simulation`/`World`) that owns the cars and the timer. The controller then only translates user input.
- **`CarView`** both builds the UI and holds state (`gasAmount`). The state could live in the controller, or the buttons could go in a separate `ControlPanel` class so layout changes don't touch the rest.
- **`DrawPanel`** mixes image loading, position storage and painting. Separate image loading (an `ImageLoader`) from painting, and keep positions in the model.

### Classes that could be fused

- Nothing needs fusing. `TimerListener` is tiny, but it is an inner class of `CarController`, so that is already fine.
- If you have two cars with copied `speedFactor` logic, pull the shared part into `Car` instead of duplicating it.
